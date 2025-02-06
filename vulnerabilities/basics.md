# Basics of Vulnerabilities

Topics to discuss in this file

1. What vulnerabilities are
2. Why they're worthy of learning
3. Haw are vulnerabilities rated
4. Databases for vulnerability research

## What vulnerabilities are

A vulnerability in cybersecurity is defined as a weakness or flaw in the design, implementation or behaviours of a system or application.. An attacker can ecploit these weaknesses to gain access to unauthorised information or perform unauthorised actions. The term vulnerability has many definitions by cybersecurity bodies. However, there is minimal variagtion between them all.

For example, NIST defines a vulnerability "as weakness in an information system, system security procedures, internal controls, or implementation that could be exploited or triggered by a threat source."

There are five main categories of vulnerabilities:

|Vulnerability|Description|
|---|---|
|Operating System|These types of vulnerabilities are found within Operating Systems (OSs) and often result in privilege escalation.|
|(Mis)Configuration-based|These types of vulnerability stem from an incorrectly configured application or service. For example, a website exposing customer details.|
|Weak or Default Credentials|Applications and services that have an element of authentication will come with default credentials when installed. For example, an administrator dashboard may have the username and password of admin. These are easy to guess by an attacker|
|Application Logic|These vulnerabilities are a result of poorly designed applications. For example, poorly implemented authentication mechanisms that may result in an attacker being able to impersonate a user.|
|Human-Factor|Human-Factor vulnerabilities are vulnerabilities that leverage human behaviour. For example, phishing emails are designed to trick humans into believing they are legitimate.|
|||

## Vulnerability Scoring

Vulnerability management is the process of evaluating, categorising and ultimately remediating threats (vulnerabilities) faced by an organisation. It is arguably impossible to patch and remedy every single vulnerability in a network or computer system and sometimes a waste of resources.

After all, only approximately **2%** of vulnerabilities only ever end up being exploited. Instead, it is all about addressing the most dangerous vulnerabilities and reducing the likelihood of an attack vector being used to exploit a system.

Two different scoring systems are discussed below:

### Common Vulnerability Scoring System

First introduced in 2005, the Common Vulnerability Scoring System (or CVSS) is a very popular framework for vulnerability scoring and has three major iterations. As it stands, the current version is CVSSv4

A vulnerability is given a classification (out of five) depending on the score that it has assigned.

|Rating|Score|
|---|---|
|None|0|
|Low|0.1-3.9|
|Medium|4.0-6.9|
|High|7.0-8.9|
|Critical|9.0-10.0|
|||

## Vulnerability Priority Rating

The VPR framework is a much more modern framework in vulnerability management - developed by Tenable, and inducstry solutions provider for vulnerability management. This framework is considered to be risk-driven; meaning that vulnerabilities are given a score with a heavy focus on the risk a vulnerability poses to the organisation itself, rather that factors such as impact (like with CVSS).

VPR uses a similar scoring rage as CVSS, but it does not have None category as with CVSS

|Rating|Score|
|---|---|
|Low|0-3.9|
|Medium|4.0-6.9|
|High|7.0-8.9|
|Critical|9.0-10.0|
|||

## Vulnerability Database

There are two very popular databases to look for any information related to vulnerability

1. [NVD (National Vulnerability Database)](https://nvd.nist.gov/vuln)
2. [Exploit-DB](https://www.exploit-db.com)

Definition of some important term

|Term|Definition|
|---|---|
|Vulnerability|A vulnerability is defined as a weakness or flaw in the design, implementation or behaviours of a system or application.|
|Exploit|An exploit is something such as an action or behaviour that utilises a vulneability on a system or application.|
|Proof of Concept (PoC)|A PoC is a technique or tool that often demonstrates the exploitation of a vulnerability|
|||
