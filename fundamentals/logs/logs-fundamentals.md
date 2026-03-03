# Logs Fundamentamentals

## Introduction

Attackers are clever. They avoid leaving maximum traces on the victim’s side to avoid detection. Yet, the security team successfully determines how the attack was executed and is even sometimes successful in finding who was behind the attack.

Suppose a few policemen are investigating the disappearance of a precious locket in a snowy jungle cabin. They observed that the wooden door of the cabin was brutally damaged, and the ceiling collapsed. There were some footprints on the snowy path to that cabin. Lastly, they discovered some CCTV footage from a neighbouring residence. By placing together all these traces, the police successfully determined who was behind the attack. Various traces are found in several such cases; putting all these together takes you closer to the criminal.

It seems like these traces play a big role in the investigations.

What if something happened within a digital device? Where do we find all these traces to investigate further?

There are various places inside a system where the traces of an attack could be fetched. The logs contain most of these traces. Logs are the digital footprints left behind by any activity. The activity could be a normal one or the one with malicious intent. Tracing down the activity and the individual behind the execution of that activity becomes easier through logs.

### Use Cases of Logs

The following are some key areas in which the logs play an integral role.

|Use Case|Description|
|---|---|
|Security Events Monitoring|Logs help us detect anomalous behavior when real-time monitoring is used.|
|Incident Investigation and Forensics|Logs are the traces of every kind of activity. It offers detailed information on what happened during the incident. The security team utilizes the logs to perform root cause analysis of incidents.|
|Troubleshooting|As the logs also record the errors in systems or applications, they can be used to diagnose issues and helpful in fixing them.|
|Performance Monitoring|Logs can also provide valuable insights into the performance of applications.|
|Auditing and Compliance|Logs play a major role in Auditing and Compliance, making it easier with its capability to establish a trail of different kinds of activities.|
|||

This room will equip you with an understanding of various types of logs maintained in different systems. We will also be practically investigating logs as traces of different attacks.

## Types of Logs

There are several types of logs as discussed below:

|Log Type|Usage|Example|
|---|---|---|
|System Logs|The system logs can be helpful in troubleshooting running issues in the OS. These logs provide information on various operating system activities.|- System Startup and shutdown events<br>- Driver Loading events<br>- System Error events<br>- Hardware events|
|Security Logs|The security logs help detect and investigate incidents. These logs provide information on the security-related activities in the system.|-Authentication events<br>- Authorization events<br>- Security Policy changes events<br>- User Account changes events<br>- Abnormal Activity events|
|Application Logs|The application logs contain specific events related to the application. Any interactive or non-interactive activity happening inside the application will be logged here.|- User Interaction events<br>- Application Changes events<br>- Application Update events<br>- Application Error events|
|Audit Logs|The Audit logs provide detailed information on the system changes and user events. These logs are helpful for compliance requirements and can play a vital role in security monitoring as well.|- Data Access events<br>- System Change events<br>- User Activity events<br>- Policy Enforcement events|
|Network Logs|Network logs provide information on the network’s outgoing and incoming traffic. They play crucial roles in troubleshooting network issues and can also be handy during incident investigations.|- Incoming Network Traffic events<br>- Outgoing Network Traffic events<br>- Network Connection Logs<br>- Network Firewall Logs|
|Access Logs|The Access logs provide detailed information about the access to different resources. These resources can be of different types, providing us with information on their access.|- Webserver Access Logs<br>- Database Access Logs<br>- Application Access Logs<br>- API Access Logs|
||||

## Windows Event Logs Analysis

Like other operating systems, Windows OS also logs many of the activities that take place. These are stored in segregated log files, each with a specific log category. Some of the crucial types of logs stored in a Windows Operating System are:

- **Application:** There are many applications running on the operating system. Any information related to those applications is logged into this file. This information includes errors, warnings, compatibility issues, etc.
- **System:** The operating system itself has different running operations. Any information related to these operations is logged in the System log file. This information includes driver issues, hardware issues, system startup and shutdown information, services information, etc.
- **Security:** This is the most important log file in Windows OS in terms of security. It logs all security-related activities, including user authentication, changes in user accounts, security policy changes, etc.

Besides these, several other log files in the Windows operating system are designed for logging activities related to specific actions and applications.

Unlike other log files studied in the previous tasks, which had no built-in application to view them, Windows OS has a utility known as Event Viewer, which gives a nice graphical user interface to view and search for anything in these logs.

To open Event Viewer, click on the Start button of Windows and type ‘Event Viewer’. It will open the Event Viewer for you, as shown below. The highlighted area in the screenshot below shows the different available logs.

This is how a Windows event log looks. It has different fields. The major fields are discussed below:

- **Description:** This field has a detailed information of the activity.
- **Log Name:** The Log Name indicates the log file name.
- **Logged:** This field indicates the time of the activity.
- **Event ID:** Event IDs are unique identifiers for a specific activity.

Numerous event IDs are available in Windows event logs. We can use these event IDs to search for any specific activity. For example, event ID 4624 uniquely identifies the activity of a successful login, so you only need to search for this event ID 4624 when investigating successful logins.

Here is a table of some important Event IDs in Windows Operating System.

|Event ID|Description|
|---|---|
|4624|A user account successfully logged in|
|4625|A user account failed to login|
|4634|A user account successfully logged off|
|4720|A user account was created|
|4724|An attempt was made to reset an account’s password|
|4722|A user account was enabled|
|4725|A user account was disabled|
|4726|A user account was deleted|
|||

## Web Server Access Logs Analysis

We interact with many websites daily. Sometimes, we just want to view the website, and sometimes, we want to log in or upload a file into any available input field. These are just different kinds of requests we make to a website. All these requests are logged by the website and stored in a log file on the web server running that website.

This log file contains all the requests made to the website along with the information on the timeframe, the IP requested, the request type, and the URL. Following are the fields taken from a sample log from an Apache web server access log file which can be found in the directory: /var/log/apache2/access.log  

    IP Address: “172.16.0.1” - The IP address of the user who made the request.

    Timestamp: “[06/Jun/2024:13:58:44]” - The time when the request was made to the website.

    Request: The request details.
        HTTP Method: “GET” - Tells the website what action to be performed on the request.
        URL: “/” - The requested resource.

    Status Code: “200” - The response from the server. Different numbers indicate different response results.

    User-Agent: “Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36” - Information about the user’s Operating System, browser, etc. when making the request.

We can perform manual log analysis by using some command line utilities in the Linux operating system. The following are some commands that can be useful during manual log analysis. 

cat is a popular utility for displaying the contents of a text file. We can use the cat command to display the contents of a log file, as they are typically in the text format.
