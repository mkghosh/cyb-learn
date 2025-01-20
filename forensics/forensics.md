# Forensics document

## Introduction
Performing live forensic file system analysis is often an early part of incident response and is crucial in assessing and determining potential security breaches. This process involves examining digital artefacts, system logs, users, and file structures to uncover evidence of unauthorised access, malicious activities, or data compromise.

Forensics is the application of science to investigate crimes and establish facts. With the use and spread of digital systems, such as computers and smartphones, a new branch of forensics was born to investigate related crimes: computer forensics, which later evolved into, digital forensics.

## Tools for digital forensics
1. pdfinfo used to read metadata from a pdf file
- $pdfinfo doc.pdf
2. exiftool used to read and write metadata to a different range of files e.g. JPG, JPEG etc
- $exiftool name.jpg
3. file used to see an overview of the metadata present in any file.
- $file filename
4. **dd and dc3dd:** These command-line utilities create exact bit-by-bit copies of hard drives. While dd is the foundational tool dc3dd offers additional features such as progress indicators and error summaries, making it more suitable for forensic tasks.
5. **Guymager:** This graphical disk imaging tool for Linux supports various disk image formats, provides a user-friendly interface, and maintains forensic integrity. It also provides write-blocking functionality and can generate checksums to verify data integrity.
6. **FTK Imager:** This forensic imaging tool offers a comprehensive approach to disk imaging. It supports various media types, such as hard drives, CDs, DVDs, and USB drives. Some of its functionalities allow data previewing and provide multiple output formats.
7. **Volatility:** This robust framework is essential for extracting digital artefacts from bolatile memory (RAM) dumps. Its usage is perfect for analysing memory dumps to uncover running processes, open network connections, and other volatile data that might be lost in a cold system analysis.
8. **The Sleuth Kit (TSK):** This is a collection of command-line tools for analysing disk images and recovering deleted data with granular control. TSK tools like fls and icat are used to list files and directories, while other components provide advanced analysis capabilities.
9. **Autopsy:** Built on top of TSK, Autopsy provides a graphical interface for TSK funcitons, making it more accessible to non-technical users. It offers keyword searches, timeline analysis, and file carving features.
10. **EnCase:** EnCase is a professional-grade forensic tool used for deep forensic investigations. It has extensive capabilities for disk imaging, data recovery, and analysis.
11. **[LiME](https://github.com/504ensicsLabs/LiME):** LiME is forensics tool for dumping volatile memory in linux.

## Process
Step 1. **Obtaining & imaging forensic data:** Imaging is a very important and crucial step whenever a forensic examiner investigates a case. The purpose of imaging is to copy and create a replica of every single bit of original data to use it for analysis while ensuring that the original data remains untouched. This ensures the integrity and preservation of the data throughout the analysis process.

Step 2. **Forensic Request:** A forensic request is a request to investigate a case within a given outline, including the purpose, scope, authority, methodology, expected deliverables, and confidentiality measures. This outline provides a clear understanding of the case.

Step 3. **Preparation/Extraction:** The preparation in digital forensic is very important phase in which the forensic environment is prepared and selection and testing of necessary tools is done and ensuring the legal requirements met or not. The proper documentation is maintained to show step-by-step process and steps taken by examiners to have better understanding about the investigation for any new examiner. After that the extraction of data which contains system files, deleted files, system logs, network data and application data.

Step 4. **Identification:** Identification of relevent data from the extracted data which includes files, logs, mails, and other data. The identification is important because it helps you to not mislead or stuck in the case and can help you to speed up to solve the case.

Step 5. **Analysis:** Analysis of identified data to find the hidden evidence and patterns. In the analysis process we can analyze about who created, and modified files and application also where it is found and when it is created and where it is sent. The analysis tells what happened with the system when the specific file is transmitted.

Step 6. **Forensic Reporting:** Creating a detailed report about the case findings proper chain of custody, tools used, methodologies used, conclusion and proper documentation which is presentable in legal contexts.

Step 7. **Case level Analysis:** The final analysis by studying and reviewing the case again with all findings and evidence to made understanding and to find out the final conclusion.

## Types of forensics
There are several types of digital forensics that can be used in cybersecurity, including:
## Computer forensics
It has two distinct categories 
   - Live Forensics
   - Cold Forensics
   
## Network forensics
## Mobile device forensics
## Database forensics
## Memory forensics
## Digital media forensics
## Malware forensics
## Email forensics
## Cloud forensics

### Cold Forensics
In our cyber security journey, forensics plays a crucial role in gathering evidence and putting together the puzzle of a breach. While live forensics tackles actively running systems, cold system forensics focuses on dormant or powered-off machines, aiming to preserve the system's state and any potential evidence. 

Cold system forensics emerged as a direct response to a malicious technique known as a *cold boot attack*. Attackers would use this technique to take advantage of the fact that data in a computer's RAM persits for a short period, depending on the RAM's temperature, after the system is powered off. The cooler the RAM chips are, the longer the data can be retained. Attackers could rapidly reboot a compromised machine to access sensitive information, such as encryption keys, passwords, or in-memory data, before it is completely erased. To counter this threat, reserchers began exploring methods to preserve and analyse the contents of a system's volatile memory even when powered off, marking the initiation of cold system forensics.

Cold vs. Live Forensics
|Aspect|Cold Forensics|Live Forensics|
|---|---|---|
|System State|Examines powered-off systems|Examines running systems|
|Evidence Integrity|Minimal risk|High risk|
|Data Capture|Comprehensive capture of entire drives|Limited to active data|
|Volatile Memory Access|Cannot retrieve|Captures volatile data (memory, network)|
|Time Efficiency|Time-consuming|Faster identification of threats|
|Data Volume Handling|Suitable for large data volumes|Limited by system resources|
|Legal Suitability|Ideal for legal cases where integrity is paramount|May alter evidence, less suitable for legal cases|
|Access to Encrypted Files|Risk of losing access if passwords are reset|Immediate access while the system is running|
|

## Common Scenarios for cold system forensics
- **Risk of modifying evidence:** As live analysis can alter critical evidence, cold system forensics must ensure the evidence remains unaltered and admissible in court.
- **Comprehensive data capture:** This is necessary when a thorough examination of all data is necessary.
- **Incident response:** To preserve evidence from compromised systems without risk of alteration, shutting down a compromised server and performing cold analysis ensures the evidence ramians intact.
- **Legal proceedings:** Cold system forensics ensures uncontaminated evidence that meets legal standards. Courts require a clear chain of custody and unaltered data, best achieved through cold analysis.
- **Data recovery/File carving:** This process retrieves deleted or lost files from a system. 
- **Legacy systems:** Where live analysis is not feasible dure to outdated or unstable systems, cold system forensics is handy. Live analysis tools may not be supported in environments with legacy systems, such as older industrial control systems in manufacturing plants. Therefore, cold analysis allows forensic experts to examine these systems wihtout risking further instability or data loss.
- **Cloud and virtualised environments:** In today's world, where cloud and virtualised environments are commonly used, cold system forensics can be used to analyse virtual machines whithout impacting running services.

# Data Acquisition and Preservation
## Order of volatility
A typical order from the most bolatile to the leat volatile might look as follows

- **CPU registers and cache:** These hold the most volatile data, typically lost once the host is powered down.
- **Routing Table, ARP Cache, Process Table, Kernel Statistics, and RAM:** Data here changes rapidly, and capturing it may reveal information about running processes and network connections, which can help identify any malicious activity.
- **Temporary File Systems:** Data in temporary files is often cleared on reboot and thus changes frequently. The data can uncover recently accessed files or applications on a host.
- **Hard Disk:** Disk data is less volatile but can undergo alterations and deletions. Imaging the disk provides a comprehensive snapshot of all stored data, including deleted files and fragments.
- **Remote Logging and Monitoring Data:** These logs are relatively stable and less likely to change.
- **Physical Configuration and Network:** Documenting this data helps analysts understand the infrasture and the context of the investigation.
- **Archival media:** This data is stored offline such as tables, and optical discs.

# Guidelines to follow during forensics
- **Document every step:** No matter how small an action is during the forensic analysis, it must be documented and aligned with whoever is handling the evidence, what time was taken, and the reasons behind their access to the evidence.
- **Secure transport:** Tamper-proof packaging should be utilised to transport disk drives and other evidence securely.
- **Hasing:** Using cryptographic hash functions such as MD5 and SHA-1 to create unique data fingerprints and verify that is has not been altered.
- **Write Blocking:** Using write blockers helps prevent any modification of the original data.

# Skills need to be successful in forensics
- Keyword Searching
- File signature analysis 
- Registry Analysis 
- Email Extraction and 
- Web history extraction
