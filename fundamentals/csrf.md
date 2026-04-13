# CSRF(Cross-site Request Forgery) Introduction

## Introduction

CSRF is a web vulnerability where an attacker tricks a user's browser into sending a request to a website where the user is already authenticated. Because the browser automatically includes session cookies with every request, the web application assumes the request was made intentionally by the user.

Instead of stealing login credentials, the attacker abuses the trust relationship between the browser and the web application. If the request performs a sensitive action, such as changing an email address or updating account settings, the attacker may be able to modify the victim's account without their knowledge.

For example, imagine a user is logged in to an internal portal while browsing the internet. If the user visits a mallicious webpage, that page can silently trigger a request to the portal. If the application does not verify the origin of the request, it may process it as if the user initiated it.

## How CSRF attack works

A typical CSRF attack usually follows three simple steps:

- The victim logs in to a legitimate web application, and their browser stores a session cookie.
- The attacker tricks the victim into visiting a malicious webpage containing a crafted request.
- The victim's browser automatically sends the request to the target application along with the stored session cookie.

Because the request contains the valid session cookie, the server treats it as a legitimate user action.

## Why this is dangerous

CSRF attacks can be used to perform many actions depending on the functionality of the target application, such as:

- Changing a user's email address
- Updating account settings
- Performing financial transactions
- Modifying security preferences

Even though the victim never intended to perform the action, the web application processes it as a legitimate request.

## Why CSRF works?

The vulnerability does not exist because browsers are broken. In fact, browsers are behaving exactly as, they were designed to. The problem occurs when a web application trusts requests too much.

When a user logs into a website, the server usually creates a session and sends a session cookie to the browser. This cookie acts like an identity card. Every time the browser sends a request to that website, the cookie is automatically included, so the server knows which user is making the request.

The key detial here is that the browser automatically sends cookies with every request to the same doamin. It dows not matter whether the request came from a legitimate page on the site or from a malicious webpage somewhere else on the internet.

### Key conditions for a CSRF Attack

For a CSRF attack to work, three main conditions usually have to exist:

1. The victim must be authenticated to the target application.
2. The application must perform a state-changing action such as updating settings or modifying account data.
3. The application must not verify whether the requst came from a trusted source.

If these conditions exist, an attacker may be able to perform actions on behalf of the victim without ever knowing their credentials.

## Finding CSRF vulnerabilites

Before exploiting CSRF, a pentester must first identify where the vulnerability might exist. Not every feature in a web application is a good target. CSRF attacks usually focus on actions that change data or modify account settings.

Whenever a user performs an action on a website, the browser sends an HTTP request to the server. Some requests simply retrieve information, while others modify the application's state. These state-changing requests are the primary targets for CSRF attacks.

As a pentester, you should carefully inspect application functionality and ask a simple question.

**Can this action be triggered without verifying that the request came from the user?**

If the answer is yes, the feature might be vulnerable to CSRF.

## Common features vulnerable to CSRF

Certain actions frequently appear in CSRF vulnerabilities because they modify important user data. While testing an application, pay special attention to requests that perform opearations such as:

1. Change Email Address
2. Update Account Password
3. Edit Profile Details
4. Alter Payment Info
5. Updating Payment Info
6. Submit User Preference Form

If these request do not include any protection mechanism, such as CSRF tokens, they may be susceptible to forged requests.

## GET vs POST - A common Misconception

Many developers assume that using the POST method automatically protects against CSRF attacks, This is not true.

Both GET and POST requests can be abused if the application does not verify that the request originated from a trusted page. In fact, many CSRF attacks rely on simple GET requests that can be triggered through links or images.

Because of this, pentestes should never rely solely on the requst method when testing for CSRF vulnerabilities.

## Best Practices

1. Focus on state-changing requests: Prioritise requests that modify data, such as password changes, email updates, account settings, or financial transactions. These endpoints are the most common targets for CSRF attacks.
2. Inspect request for CSRF tokens: Check whether sensitive actions include a CSRF token. If no token exists, or if the token appears static or predictable, the request may be vulnerable.
3. Analyse HTTP methods: Sensitive actions should typically use POST requests. If important operations are performed through GET requests, they may be easier to exploit using images or simple links.
4. Test the requests outside of the application: Copy the request and try to reproduce it from an external HTML page. If the action succeeds without additional verification, the endpoint is likely vulnerable to CSRF.
5. Observe cookie behaviour: Check whether authenticaiton relies only on session cookies. If the application automatically accepts requests containing the session cookie without validating the request origin, CSRF attacks become possible.
