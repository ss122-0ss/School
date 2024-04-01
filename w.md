# Simple School Management System Staff avatars exist at any File upload

- **Exploit Title:** Simple School Management System Staff avatars exist at any timeFile upload
- **Date:** 2024-04-01
- **Exploit Author:** shuo sheng,jixin zhang
- **Vendor Homepage:** https://code-projects.org/simple-school-management-in-php-with-source-code/
- **Software Link:** https://download.code-projects.org/details/d10e92aa-e37f-46fd-9bf8-45878956d7c0
- **Version:** 1.0

## Description:

The function for employees of Simple School Management System v1.0to upload avatars is not perfect, and the suffix of uploaded files is not strictly verified, resulting in arbitrary file upload vulnerabilities.



## Proof of Concept:

First register a staff account. The default account is used here.

```
name:ram
pass:1234
```

Go to this address:"http://localhost/School/teacher_home.php"

![image-20240401182009483](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401182009483.png)

Afterward, click the "Add Profile Details" button, and then interupt and observe the HTTP request in Burp Suite.

payload

```
<?php phpinfo()?>
```

### Manually change the “filename” param to “shell.php”  and in Burp Suite(see picture below).



![image-20240401182147659](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401182147659.png)

Send the data packet, accept the return data, and you can see that the modification is successful.

![image-20240401182433323](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401182433323.png)

By accessing the webshell file from the URL the malicious code is executed.

![image-20240401182519669](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401182519669.png)

