# Server Side Template Injection

## Introduciton

Server Side Template Injection (SSTI) is a web exploit which takes advantage of an insecure implementation of a template engine.

## What is template engine?

A template engine allows you to create static template files which can be re-used in your application. Consider a page that stores information about a user, **/profile/<user>**. The code might look something like this in Python's Flask:

```Python
from flask import Flask, render_template_string
app = Flask(__name__)

@app.route("/profile/<user>")
def profile_page(user):
    template = f"<h1>Welcome to the profile of {user}!</h1>"
    return render_template_string(template)

app.run()
```

This code creates a template string, and concatenates the user input into it. This way, the content can be loaded dynamically for each user, while keeping a consistent page format.

Note: Flask is the web framework, while Jinja2 is the template engine being used.

## How is SSTI exploitable?

Consider the above code, specifically the template string. The variable **user** (which is user input) is concatenated directly into the template, rather than passed in as data. This means whatever is supplied as user input will be interpreted by the engine.

Note: The template engines themselves aren't vulnerable, rather an insecure implementation by the developer.

## What is the impact of SSTI?

As the name suggestes, SSTI is a server side exploit, rather than client side such as cross site scripting (XSS). This means that vulnerabilities are even more critical, because instead of an account on the website being hijacked (common use of XSS), the server instead gets hijacked. The possibilities are endless, however the main goal is typically to gain remote code execution.

## Detection

### Finding an injection point

The exploit must be inserted somewhere, this is called an injection point. There are a few places we can look within an application, such as the URL or an input box (make sure to check for hidden inputs).

### Fuzzing

Fuzzing is a technique to determine whether the server is vulnerable by sending multiple characters in hopes to interfere with the backend system. This can be done manually, or by an application such as BurpSuite's intruder.

Luckily for us, most template engines will use a similar character set for their "special functions" which makes it relatively quick to detect if it's vulnerable to SSTI. For example, the following characters are known to be used in quite a few template engines: **{{<%[%'"}}%**.

To manually fuzz all of these characters, they can be sent one by one following each other.

## Identification

Now that we have detected what charactes caused the application to error, it is time to identify what template engine is being used. In the best case scenario, the error message will include the template engine, which marks this step complete! However, if this is not the case, we can use a decision tree to help us identify the template engine:

```bash
                           --> Smarty
        --->a{*comment*}b -|                      -->Mako
        |                  --> ${"z".join("ab")}-|
${7*7} -|                                         -->Unknown
        |                            -->Jinja2
        |             -->{{7*'7'}} -|--> Twig
        ---> {{7*7}} -|              -->Unknown
                      --Not Vulnerable
```

To follow the decision tree, start at the very left and include the variable in your request. Follow the arrow depending on the output.

According to our enumeration the template engine seems to be Jinja2.

## Exploitation

At this point, we know:
    - The application is vulnerable to SSTI
    - The injection point
    - The template engine
    - The template engine syntax

### Planning

Let's first plan how we would like to exploit this vulnerability.

Since Jinja2 is a Python
