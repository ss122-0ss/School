# Simple school management system staff avatar upload point exists in any file upload

- **Exploit Title:** Simple school management system staff avatar upload point exists in any file upload
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

![图片](https://github.com/ss122-0ss/School/assets/131983607/d3cd4972-b65f-44a7-8be4-6043220f318f)


Afterward, click the "Add Profile Details" button, and then interupt and observe the HTTP request in Burp Suite.

payload

```
<?php phpinfo()?>
```

### Manually change the “filename” param to “shell.php”  and in Burp Suite(see picture below).



![图片](https://github.com/ss122-0ss/School/assets/131983607/eb7d2d9e-3804-4a64-875a-88913460abe4)


Send the data packet, accept the return data, and you can see that the modification is successful.

![图片](https://github.com/ss122-0ss/School/assets/131983607/6c6b68e9-b225-4482-9050-904c7dced91a)


By accessing the webshell file from the URL the malicious code is executed.

![图片](https://github.com/ss122-0ss/School/assets/131983607/aef0cf6d-91bf-452c-824d-5ad6ce51aea2)

The code does not perform any verification on the file name, and the code has great security

The code is located in teacher_home.php line 36

```php
<?php
						if(isset($_POST["submit"]))
						{
							$target="staff/";
							$target_file=$target.basename($_FILES["img"]["name"]);
							
							if(move_uploaded_file($_FILES['img']['tmp_name'],$target_file))
							{
								$sql="update staff set PNO='{$_POST["pno"]}',MAIL='{$_POST["mail"]}',PADDR='{$_POST["addr"]}',IMG='{$target_file}'where TID={$_SESSION["TID"]}";
								$db->query($sql);
								echo "<div class='success'>Insert Success</div>";
							}
							
						}
					
					
					?>
					
```


