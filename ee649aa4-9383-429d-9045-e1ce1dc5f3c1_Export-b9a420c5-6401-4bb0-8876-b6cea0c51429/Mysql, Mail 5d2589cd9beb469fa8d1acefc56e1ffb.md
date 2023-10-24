# Mysql, Mail

### srv01

```html
root@srv01:~# apt install postfix dovecot-imapd -y
```

OK!

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled.png)

```html
root@srv01:~# vim /etc/dovecot/conf.d/10-auth.conf
```

```html
10 disable_plaintext_auth = no\
51 auth_username_format = %n
```

```html
root@srv01:~# systemctl restart dovecot
```

---

### Client

```html
root@client:~# apt install thunderbird -y
```

```html
 root@srv01:~# startx
```

thunderbird 실행

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%201.png)

Configure manually 클릭

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%202.png)

Connection security  = None

---

### ISP

```html
root@ISP:~# openssl req -out mail.req -newkey rsa:2048 -nodes -keyout mail.key -subj '/CN=mail.worldskills.org'
```

```html
root@ISP:~# openssl ca -in mail.req -out mail.crt
```

---

### srv01

```html
root@srv01:~# scp 1.1.1.1:/etc/ssl/mail.* /etc/ssl
```

```html
root@srv01:~# vim /etc/postfix/main.cf
```

```html
27 =/etc/ssl/mail.crt
28 =/etc/ssl/mail.key
```

```html
root@srv01:~# vim /etc/postfix/master.cf
```

```html
17 ~ 19 주석X
ssl tls 
29 ~31
993 465
```

```html
root@srv01:~# systemctl restart postfix 
```

```html
root@srv01:~# vim /etc/dovecot/conf.d/10-ssl.conf
```

```html
12 =</etc/ssl/mail.crt
13 =</etc/ssl/mail.key
```

```html
root@srv01:~# systemctl restart dovecot
```

---

### Client

```html
terminel
scp root@1.1.1.1:/etc/ssl/CA/cacert.pem ./NAVER-CA.crt
```

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%203.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%204.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%205.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%206.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%207.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%208.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%209.png)

메일 전송 확인 

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2010.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2011.png)

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2012.png)

---

## Mysql

### SRV01

```html
root@srv01:~# apt install mariadb-server
```

```html
root@srv01:~# mysql
```

```html
MariaDB [(none)]> 
```

```html
MariaDB [(none)]> create database WORLDSKILLS_DATABASE;
```

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2013.png)

```html
MariaDB [(none)]> show databases;
```

```html
MariaDB [(none)]> use WORLDSKILLS_DATABASE;
Database changed
MariaDB [WORLDSKILLS_DATABASE]>
```

```html
MariaDB [WORLDSKILLS_DATABASE]> create table USER_LIST(
     -> id int(10),
     -> name varchar(100),
     -> password varchar(100) default 'Pa$$worD');
Query OK
```

```html
MariaDB [WORLDSKILLS_DATABASE]> show tables;
USER_LIST
```

```html
MariaDB [WORLDSKILLS_DATABASE]> desc USER_LIST;
```

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2014.png)

```html
MariaDB [WORLDSKILLS_DATABASE]> INSERT USER_LIST(id,name) values(1,'Jang Yooseong');
```

```html
MariaDB [WORLDSKILLS_DATABASE]> select id,name,password from USER_LIST;
or
select * from USER_LIST;
```

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2015.png)

```html
MariaDB [WORLDSKILLS_DATABASE]> INSERT USER_LIST(id,name,password) values(2,'Park Jongmin','pjm1024cl');
```

```html
MariaDB [WORLDSKILLS_DATABASE]> drop table USER_LIST;
OK!
```

```html
MariaDB [WORLDSKILLS_DATABASE]> drop database WORLDSKILLS_DATABASE;
OK!
```

```html
MariaDB [(none)]> grant all on *.* to 'admin'@'192.168.0.2' identified by 'Skill39';
```

```html
MariaDB [(none)]> grant all on *.* to 'admin'@'%' identified by 'Skill39';
```

---

### SRV01

```html
root@srv01:~# vim /etc/mysql/mariadb.conf.d/50-server.cnf
```

```html
30 0.0.0.0
```

```html
root@srv01:~# mysql -u admin -pSkill39 -h 192.168.0.1    
```

---

### Client

```html
apt install mariadb-client -y
```

```html
terminel
mysql -u admin -pSkill39 -h 192.168.0.2

Mariadb [(none)]>
```

---

## Web Mail

### SRV01

```html
root@srv01:~# apt install roundcube -y
```

YES

```html
root@srv01:~# mysql -u root

MariaDB [(none)]> 
```

```html
MariaDB [(none)]> show databases;
```

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2016.png)

```html
MariaDB [(none)]> use roundcude
```

```html
MariaDB [roundcube]> show tables;
MariaDB [roundcube]> exit
Bye
```

```html
root@srv01:~# apt list apache2
Listing ... Done

root@srv01:~# cd /etc/roundcube
```

```html
root@srv01:/etc/roundcube# vim debian-db.php
```

```html
12 = 'admin';
16 = '192.168.0.2';
```

```html
root@srv01:/etc/roundcube# vim config.inc.php
```

```html
q36 = 'ssl://mail.worldskills.org';
48 = 'ssl://mail.worldskills.org';
51 = 465
55 = ''
59 = ''
```

```html
root@srv01:/etc/roundcube# vim defaults.inc.php
```

```html
147 = 'ssl://mail.worldskills.org';
150 = '993'
160 ~ 166 주석해제
162 = false,
168 #
270 = 'ssl://mail.worldskills.org';
273 = 465
308 ~ 314 주석해제
310 false,
316 #
558 = 'worldskills.org';

```

```html
root@srv01:/etc/roundcube# cd /etc/apache2/sites-available
```

```html
root@srv01:/etc/apache2/sites-available# cp 000-default.conf roundcube.conf
```

```html
root@srv01:/etc/apache2/sites-available# a2ensite roundcube.conf
```

```html
root@srv01:/etc/apache2/sites-available# vim roundcube.conf
```

```html
9 ServerName mail.wordskills.org
12 DocumentRoot /usr/share/roundcube
```

```html
root@srv01:/etc/apache2/sites-available# systemctl restart apache2
```

```html
root@srv01:~# login user02
Skill39
```

---

### Client

FireFox

mail.worldskills.org

![Untitled](Mysql,%20Mail%205d2589cd9beb469fa8d1acefc56e1ffb/Untitled%2017.png)

user02

Skill39