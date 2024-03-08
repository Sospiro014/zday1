- Title: Internet Banking System - Authenticated RCE
- Application: Internet Banking System
- Date: 08.03.2024
- Bugs: Remote code execution
- Exploit Author: SoSPiro
- Vendor Homepage: https://codeastro.com/author/nbadmin/
- Software Link: https://codeastro.com/internet-banking-system-in-php-with-source-code/
- Version: 1.0
- Tested on: Windows 10 64 bit Wampserver


## Vulnerability Description:

- There is a file upload security vulnerability in the pages_account.php file of the Client Portal. This vulnerability allows users to upload malicious files that could potentially impact the system. Due to the lack of file type checks, attackers can upload unwanted files.


## Proof of Concept (PoC):

- An attacker registers through the Client Portal and logs into the system. Subsequently, they access the Account page (/client/pages_account.php) and follow the steps below to upload a malicious file in the Profile Picture section:


1. The attacker logs in through the Client Portal.
2. They are redirected to the Account page.
3. The attacker fills out the file upload form in the Profile Picture section.
4. Choosing a file using the "Choose File" option, they select a malicious file for upload.
5. The attacker completes the file upload process and updates the profile picture.

- As a result of this attack, it is demonstrated that an attacker can upload a malicious file to the system, potentially leading to undesired consequences.


## Vulnerable Code Section:

- Security checks during the file upload process are missing. The code snippet below represents the vulnerable part:

```
$profile_pic  = $_FILES["profile_pic"]["name"];
move_uploaded_file($_FILES["profile_pic"]["tmp_name"], "../admin/dist/img/" . $_FILES["profile_pic"]["name"]);
```
- This code lacks file type checks and secure file name generation. An attacker could upload malicious files, introducing security vulnerabilities.



