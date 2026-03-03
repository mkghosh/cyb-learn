# Burp Suite

## What is burp suite

In essence, Burp Suite is a Java-based framework designed to serve as a comprehensive solution for conducting web application penetration testing. It has become the industry standard tool for hands-on security assessments of web and mobile applications, including those that rely on applicaiton programming interfaces (APIs).

Simply put, Burp Suite captures and enables manipulation of all the HTTP/HTTPS traffic between a browser and a web server. This fundamental capability forms the backbone of the framework. By intercepting requests, users have the flexibility to route them to various components within the Burp Site framework. The ability to intercept, view, and modify web requests before they reach the target server or even manipulate responses before they are received by our browser makes Burp Suite an invaluable tool for manual web application testing.

Burp Suite is abailable in different editions. For our purposes, we will focuss on the Burp Suite Community Editions, which is freely accessible for non-commercial use within legal boundaries. However, it's worth noting that Burp Suite also offers Professional and Enterprise editions, which come with advanced features and require licensing:

1. Burp Suite Professional is an unrestricted version of Burp Suite Community. It comes with features such as:
   - An automated vulnerability scanner.
   - A fuzzer/brute-forcer that isn't rate limited.
   - Saving projects for future use and report generation.
   - A built-in API to allow integration with other tools.
   - Unrestricted access to add new extensions for greater functionality.
   - Access to the Burp Suite Collaborator (effectively providing a unique request catcher self-hosted on running on a Portswigger-owned server).

In short, Burp Suite Professional is a highly potent tool, making it a preferred choice for professionals in the field.
2. Burp Suite Enterprise, in contrast to the community and professional editions, is primarily utilized for continuous scanning. It features an automated scanner that periodically scans web applications for vulnerabilities, similar to how tools like Nessus perform automated infrastructure scanning. Unlike the other editions, which allow manual attacks from a local machine, Burp Suite Enterprise resides on a server and constantly scans the target web applications for potential vulnerabilities.

## Features of Burp Community

Although Burp Suite Community offers a more limited feature set compared to the Professional edition, it still provides an impressive array of tools that are highly valuable for web application testing. Let's explore some of the key features:

- **Proxy:** The Burp Proxy is the most renowned aspect of Burp Suite. It enables interception and modification of requests and responses while interacting with web applications.
- **Repeater:** Another well-known feature. Repeater allows for capturing, modifying, and resending the same request multiple times. This functionality is particularly useful when crafting payloads through trial and error (e.g, in SQLi-Structured Query Language Injection) or testing the funcionality of an endpoint for vulnerabilities.
- **Intruder:** Despite rate limitations in Burp Suite Community, Intruder allows for spraying endpoints with requests. It is commonly utilized for brute-force attacks of fuzzing endpoints.
- **Decoder:** Decoder offers a valuable service for data transformation. It can decode captured information or encode payloads before sending them to the target. While alternative services exist for this purpose, leveraging Decoder within Burp Suite can be highly efficient.
- **Comparer:** As the mane suggents, Comparer enables the comparison of two pieces of data at either the word or byte level. While not exclusive to Burp Suite, the ability to send potentially large data segments directly to a comparison tool with a single keyboard shortcut significantly accelerates the process.
- **Sequencer:** Sequencer is typically employed when assessing the randomness of tokens, such as session cookie values or other suppposedly randomly generated data. If the algorithm used for generating these values lacks secure randomness, it can expose avenues for devastating attacks.

Beyond the built-in features, the Java codebase of Burp Suite facilitates the development of extensions to enhance the framework's functionality. These extensions can be written in Java, Pyton (using the Java Jython interpreter), or Ruby (using the Java JRuby interpreter). The Burp Suite Extender module allows for quick and easy loading of extensions into the framework, while the marketplace, known as BApp Store, enables downloading of third-party modules. While certain extesnions may require a professional license for integration there are still a considerable number of extensions available for Burp Community. For instance Logger++ module can extend the built-in logging functionality of Burp Suite.

## The Dashboard

Upon opening Burp Suite for the first time, you might encounter a screen with training options. It is highly recommended to go through these training materials when you have the time.

The Burp Dashboard is divided into four quadrants, as lableeled in counter-clockwise order starign from the top left:

1. **Tasks:** The Tasks menu allows you to define background tasks that Burp Suite will perform while you use the application. In Burp Suite Community, the default "Live Passive Crawl" task, which automatically logs the pages visited, is sufficient for our purposes in this module. Burp Suite Professional offers additional features like on-demand scans.
2. **Event log:** The Event log provides information about the actions performed by Burp Suite, such as starting the proxy, as well as details about connections made through Burp.
3. **Issue Activity:** This section is specific to Burp Suite Professional. It displays the vulnerabilities identified by the automated scanner, ranked by severity and filterable based on the certainty of the vulnerability.
4. **Advisory:** The Advisory section provides more detailed information about the identified vulnerabilities, including references and suggested remediations. This information can be exported into a report. In Burp Suite Community, this section may not show any vulnerabilities.

## Navigation

In Burp Suite, the default navigation is primarily done through the top menu bars, which allow you to switch between modules and access various sub-tabs within each module. The sub-tabs appear in a second menu bar directly below the main menu bar.

Here's how the navigation works:

1. **Module Selection:** The top row of the menu bar displays the available modules in Burp Suite. You can click on each module to switch between them.
2. **Sub-Tabs:** If a selected module has multiple sub-tabs, they can be accessed through the second menu bar that appears directly below the main menu bar. These sub-tabs often contain module-specific settings and options.
3. **Detaching Tabs:** If you prefer to view multiple tabs seperately, you can detach them into seperate windows. To do this, go to the window option in the application menu above the module selection bar. From there, choose the Detach option, and the selected tab will open in a seperate window. The detached tabs can be reattached using the same method.

Burp Suite also provides keyboard shortcuts for quick navigation to key tabs. By default, the following shortcuts are available:

| Shortcut | Tab |
| ---- | ---- |
| **Ctrl + Shift + D** | Dashboard |
| **Ctrl + Shift + T** | Target Tab |
| **Ctrl + Shift + P** | Proxy Tab |
| **Ctrl + Shift + I** | Intruder Tab |
| **Ctrl + Shift + R** | Repeater Tab |
| ---- | ---- |

## Options

There are two types of settings available in Burp Suite: Global Settings (also known as User Settings) and Project Settings.

- **Global Settings:** These settings affect the entire Burp Suite installation and are applied every time you start the application. They provide a baseline configuration for your Burp Suite environment.
- **Project Settings:** These settings are specific to the current project and apply only during the session. However, please note that Burp Suite community edition does not support saving projects, so any project-specific options will be lost when you close Burp.

## Burp Proxy

The Burp Proxy is a fundamental and crucial tool within Burp Suite. It enables the capture of requests and responses between the user and the target web server. This intercepted traffic can be manipulated, set to other tools for further processing, or explicitly allowed to continue to its destination.

### Key points to understand About the Burp Proxy

- **Intercepting Requests:** When requests are made through the Burp Proxy, they are intercepted and held back from reaching the target server. The requests appear in the Proxy tab, allowing for further actions such as forwarding, dropping, editing, or sending them to other Burp modules. To disable the intercept and allow requests to pass through the proxy without interruption, click the Intercept is on button.
- **Taking Control:** The ability to intercept requests empowers testers to gain complete control over web traffic, making it invaluable for testing web applications.
- **Capture and Logging:** Burp Suite captures and logs requests made through the proxy by default, even when the interception is turned off. This logging functionality can be helpful for later analysis and review of prior requests.
- **WebSocket Support:** Burp Suite also captures and logs WebSocket communicaiton, providing additional assistance when analysing web applications.
- **Logs and History:** The captured requests can be viewed in the HTTP history and WebSockets history sub-tabs, allowing for retrospective analysis and sending the requests to other Burp modules as needed.

## Connecting through the proxy

To use Burp Suite proxy, we need to configure our local web browser to redirect traffic through Burp Suite. Here are the steps to configure the Burp Suite Proxy with FoxyProxy:

1. Install FoxyProxy: Download and install the FoxyProxy Basic Extension from firefox marketplace.
2. Access FoxyProxy options: Once installed, a button will appear at the top right corner of the Firefox Browser. Click on the FoxyProxy button to access the foxyproxy options pop-up.
3. Create Burp Proxy Configuration: In the foxyproxy options pop-up, click the Options button. This will open a new browser tab with the FoxyProxy configurations. Click the Add button to create a new proxy configuration.
4. Add proxy details: On the Add Proxy page, fill in the following values:
   - Title: Burp (or any preferred name)
   - Proxy IP: 127.0.0.1
   - Port: 8080
5. Save configuration: Click save to save the Burp Proxy configuration.
6. Activate Proxy Configuration: Click the FoxyProxy icon at the top-right of the Firefox browser and select the Burp configuration. This will redrict your browser traffic through 127.0.0.1:8080. Note that Burp Suite must be running for your browser to make requests when this configuration is activated.
7. Enable Proxy Intercept in Burp Suite: Switch to Burp Suite and ensure that intercept is turned on in the Rroxy tab.
8. Test the Proxy: Open Firefox and try accessing a website such as tryhackme.com. Your browser will hang, and the proxy will populate withthe HTTP request.

## Site Map and Issue Definitions

The Target tab in Burp Suite provides more than just control over the sope of our testing. It consists of three sub-tabs:

1. Site Map: This sub-tab allows us to map out the web applications we are targeting in a tree structure. Every page that we visit while the proxy is active will be displayed on the site map. This feature enables us to automatically generate a site map by simply browsing the web application. In Burp Suite Professional, we can also use the site map to perform automated crawling of the target, exploring links between pages and mapping out APIs, as any API endpoints accessed by the web application will be captured in the site map.
2. Issue definitions: Although Burp Community does not include the full vulnerability scanning functionality available in Burp Suite Professional, we still have access to a list of all the vulnerabilities that the scanner looks for. The Issue definitions section provides an extensive list of web vulnerabilities, complete with descriptions and references. This resource can be valuable for referencing vulnerabilities in reports or assisting in describing a particular vulnerability that may have been idenfified during manual testing.
3. Scope settings: This setting allows us to control the target scope in Burp Suite. It enables us to include or exclude specific domains/IPs to define the scope of our testing. By managing the scope, we can focus on the web applications we are specifically targeting and avoid capturing unnecessary traffic.

Overall, the target tab offers features beyond scoping, allowing us to map out web applications, fine-tune our target scope, and access a comprehensive list of web vulnerabilities for reference purposes.

## The Burp Suite Browser

To start the Burp Browser, click the **Open Browser** button in the proxy tab. A Chromium window will pop up, and any request made in this browser will go through the proxy.

**Note:** There are many settings related to the Burp Browser in the project options settings.

However, if you are running Burp Suite on Linux as the root user, you may encounter an error preventing the Burp Browser from starting due to the inability to create a sandbox environment.

There are two simple solutions to this:

1. **Smart option:** Create a new user and run Burp Suite under a low-privilege account to allow the Burp Browser to run without issues.
2. **Easy option:** Go to ```Setting --> Tools --> Burp's browser``` and check the ```Allow Burp's browser to run without a sandbox``` option. Enabling this option will allow the browser to start without a sandbox. However, please be aware that this option is disabled by default for security reasons. If you choose to enable it, exercise caution, as compromising the browser could grant an attacker access to your entire machine. in the training environment of the AttackBox, this is unlikely to be a significant issue, but use it responsibly.

## Scoping and Targeting

Finally, we come to one of the most important aspects of using the Burp Proxy: Scoping

Capturing and logging all of the traffic can quickly become overwhelming and inconvenient, especially when we only want to focus on specific web applications. This is where scoping comes in.

By setting a scope for the project, we can define what gets proxied and logged in Burp Suite. We can restrict Burp Suite to target only the specific web application(s) we want to test. The easiest way to do this is by switching to the Target tab, right-clicking on our target from the list on the left, and selecting **Add to Scope**. Burp will then prompt us to choose whether we want to stop logging anything that is not in scope, and in most cases, we want to select yes.

To check our scope, we can switch to the Scope settings sub-tab within the Target tab. The Scope settings window allows us to control our target scope by including or excluding domains/IPs. This section is powerful and worth spending time getting familiar with.

However, even if we disabled logging for out-of-scope traffic, the proxy will still intercept everything. To prevent this, we need to got to the Proxy settings sub-tab and select ```And URL is in target scope``` from the "Intercept Client Requests" section.

Enabling this option ensures that the proxy completely ignores any traffic that is not within the defined scope, resulting in a cleaner traffic view in Burp Suite.

## Proxying HTTPS

When intercepting HTTP traffic, we may encounter an issue when navigating to sites with TLS enabled. For example, when accessing a site like <https://google.com/>, we may receive an error indicating that the PortSwigger Certificate Authority (CA) is not authorised to secure the connection. This happens because the browser does not trust the certificate presented by Burp Suite.

To overcome this issue, we can manually add the PortSwigger CA certificate to our browser's list of trusted certificate authorities. Here's how to do it:

1. Download the CA certificate: With Burp Proxy activated, navigate to <http://burp/cert>. This will download a file called cacert.der. Save this file somewhere on your machine.
2. Access Firefox Certificate Settings: Type ```about:preferences``` into your firefox URL bar and press enter. This will take you to the Firefox settings page. Search the page for certificates and click on the View Certificates button.
3. Import CA Certificate: In the Certificate Manager window, click on the import button. Select the cacert.der file that you downloaded earlier.
4. Set Trust for the CA certificate: In the subsequent window that appears, check the box that says "Trust this CA to identify websites" and click OK.

By completing these steps, we have added the PortSwigger CA certificate to our list of trusted certificate authorities. Now, we should be able to visit any TLS-enabled site without encountering the certificate error.
