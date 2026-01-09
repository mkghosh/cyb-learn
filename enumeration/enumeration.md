# Common placess to Enumerate

## Registration Pages

- Web applications typically make the user registration process straightforward and informative by immediately indicating whether an email or username is available. While this feedback is designed to enhance user experience, it can inadvertently serve a dual purpose. If a registration attempt results in a message stating that a username or email is already taken, the application is unwittingly confirming its existence to anyone trying to register. Attackers exploit this feature by testing potential usernames or emails, thus compilling a list of active users without needing direct access to the underlying database.

## Password Reset Features

- Password reset mechanisms are designed to help users regain access to their accounts by entering their detials to receive reset instructions. However, the differences in the application's response can unintentionally reveal sensitive information. For example, variations in an application's feedback about whether a username exists can help attackers verify user identities. By analyzing these responses, attackers can refine their lists of valid usernames, substantially inproving the effectiveness of subsequent attactks.

## Verbose Errors

- Verbose error messages during login attempts or other interactive processes can reveal too much. When these messages differentiate between "username not found" and "incorrect password," they're intended to help users understand their login issues. However, they also provide attackers with definitive clues about valid usernames, which can be exploited for more targeted attacks.

## Data Breach Information

- Data from previous security breaches is a goldmine for attackers as it allows them to test whether compromised usernames and passwords are reused across different platforms. If an attacker finds a match, it suggests not only that username is reused but also potential password recycling, especially if the platform has been breached before. This technique demonstrates how the effects of a single data breach can ripple through multiple platforms, exploiting the connections between various online identities.

## Verbose errors can trun into a goldmine of information, providing insights such as

- Internal Paths: Like a map leading to hidden treasure, these reveal the file paths and directory structures of the applicaiton server which might contain configuration files or secret keys that aren't visible to a normal user.

- Database Details: Offering a sneak peek into the database, these errors might spill secrets like table names and column details.

- User Information: Sometimes, these errors can even hint at usernames or other personal data, providing clues that are crucial for further investigation.

## Inducing Verbose Errors

Attackers induce verbose errors as a way to force the application to reveal its secrets. Below are some common techinques used to provoke these errors:

1. Invalid Login Attempts: This is like knocking on every door to see which one will open. By intentionally entering incorrect usernames or passwords, attackers can trigger error messages that help distinguish between valid and invalid usernames. For example, entering a username that doesn't exist might trigger a different error message that entering one that does, revealing which usernames are active.

2. SQL Injection: This technique involves slipping malicious SQL commands into evtry fields, hoping the system will stumble and reveal information about its database structure. For example, placing a single quote (') in a login field might cause the database throw an error, inadvertently exposing details about its schema.

3. File Inclusion/Path Traversal: By manipulating file paths, attackers can attempt to access restricted files, coaxing the system into errors that reveal internal paths. For example, using directory traversal sequences like ../../ could lead to errors that disclose restricted file paths.

4. Form Manipulation: Tweaking form fields or parameters can trick the application into displaying errors that disclose backend logic or sensitive user information. For example, altering hidden form fields to trigger validation errors might reveal insights into the expected data format or structure.

5. Application Fuzzing: Sending unexpected inputs to various parts of the application to see how it reacts can help identify weak points. For example, tools like Burp Suite Intruder are used to automate the process, bombarding the application with varied payloads to see which one provoke informative errors.

## Password reset flow vulnerabilities

- Password reset mechanism is an important part of user convenience in modern web applications. However, their implementation requires careful security considerations because poorly secured password reset processes can be easily expoited.

1. Email-based reset

- When a user resets their password, the application sends an email containing a reset link or a token to the user's registered email address. The user then clicks on this link, which directs them to a page where they can enter a new password and confirm it, or a system will automatically generate a new password for the user. This method relies heavily on the security of the user's email account and the secrecy of the link or token sent.

2. Security Question-Based Reset

- This involves the user answering a series of pre-configured security questions they had set up when creating their account. If the answers are correct, the systen allows the user to proceed with resetting their password. While this method adds a layer of security by requiring information only the user should know, it can be compromised if an attacker gains access to personally identifiable information (PII), which can sometimes be easily found or guessed.

3. SMS-Based Reset

- THis functions similarly to email-based reset but uses SMS to deliver a reset code or link directly to the user's mobile phone. Onece the user receives the codee, they can enter it on the provided webpage to access the password reset functionality. This method assumes that access to the user's mobile phone is secure, but it can be bulnerable to SIM swapping attacks or intercepts.

Each of these methods has its own vulnerabilities:

- Predictable Tokens: If the reset tokens used in links or SMS messages are predictable or follow a sequential pattern, attackers might guess or brute-force their way to generate valid reset URLs.

- Token Expiration Issues: Tokens that remain valid for too long or don not expire immediately after use provide a window of opportunity for attackers. It's crucial that tokens expire swiftly to limit this window.

- Insufficient Validation: THe mechanisms for verifying a user's identity, like security questions or email-based authentication, might be weak and susceptible to exploitation if the questions are too common or the email account is compromised.

- Information Disclosure: Any error message that specifies whether an email address or username is registered can inadvertently help attackers in their enumeration efforts, confirming the existence of accounts.

- Insecure Transport: The transmission of reset links or tokens over non-HTTPS connections can expose these critical elements to interception by network eavesdroppers.

To crack basic authentication we can use burpsuite or hydra with password lists from [this](https://github.com/danielmiessler/SecLists/tree/master/Passwords) github repository

hydra usage

```bash
hydra -l (for specific user) -L (for user list) -p (for specific password) -P (for password lists) domain request type (http-get, http-post, ssh, ftp etc)
```

We can explore [wayback machine] (https://web.archive.org) for previous versions of the site we are trying to test.

Google dorks for finding unsecured urls password log files or backup sometimes

- to find administrative panels: site:example.com inurl:admin
- to unearth log files with passwords: filetype:log "password" site:example.com
- to discover backup directories: intitile:"index of" "backup" site:example.com
