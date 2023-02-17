---
title: "Installing LWT on arch linux"
date: 2023-01-13
---
LWT (learning with text) is a very handy programme for learning a language by reading text. It allows you to collect all the pieces in one basket:
- all texts in one system
- all the words you know in one system
- you can easily search for word definitions
- you can easily add words to learn
- you can test yourself
- you can easily reread the text and recall the words you marked as unknown (or just use the test function).

Problem with official guid for LWT installing that there is no guid for Arch Linux, and if you not familiar with LAMP you will have hard time to understand all at once.
The official guid has a path for Ubuntu, but there are a few configuration changes for Arch Linux.
Since I've already been through this, I'd like to make it easy for those who decide to install LWT on Arch Linux.

As a reminder, it should probably be written that each line of the command, must be run separately, they are not intended to copy the code of the whole stage and run them as copied.

again, 1 line = 1 run

step 1: Installation of LAMP

`sudo pacman -Syu apache php php-apache mariadb`

step 2: Configuration of LAMP

For the sake of uniformity, we would use the `$EDITOR` variable, so that everyone can choose the text editor they have and are more familiar with, so that there are fewer problems with running commands.

```
export EDITOR=<editor of your choice>
$EDITOR /etc/httpd/conf/httpd.conf
```

comment this line if not already
```
[...]
# LoadModule unique_id_module modules/mod_unique_id.so
[...]
```
set lines like this
```
[...]
#LoadModule mpm_event_module modules/mod_mpm_event.so
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
[...]
```
add to the end of the file this lines
```
LoadModule php_module modules/libphp.so
AddHandler php-script php
Include conf/extra/php_module.conf
```
run commands bellow in a terminal or use this pre installed (with MariaDB) script → `sudo mysql_secure_installation`

(use any password you want, that is just example, you will need to write it in the following steps)
```
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
sudo systemctl start mariadb
mysql -u root -p
ALTER USER root@localhost IDENTIFIED VIA mysql_native_password;
SET PASSWORD = PASSWORD('abcxyz');
FLUSH privileges;
QUIT;
```

next

`$EDITOR /etc/php/php.ini`

find this lines and uncomment them
```
extension=pdo_mysql
extension=mysqli
```
Step 3:

Open a browser and go to https://sourceforge.net/projects/learning-with-texts/files/

download the latest zip archive learning_with_texts_2_0_2_complete.zip.

(version may be different)

Unzip it.
```
mkdir path/to/folder
unzip path/to/learning_with_texts_2_0_2_complete.zip -d path/to/folder
```

Step 4:

Now unzip the archive `lwt_v_2_0_2.zip`.
```
mkdir path/to/folder
unzip path/to/lwt_v_2_0_2.zip -d path/to/folder
```

Step 5:

Rename the new directory `lwt_v_2_0_2` to `lwt`.
```
mv path/to/lwt_v_2_0_2 path/to/lwt
```
The zip archive lwt_v_2_0_2.zip may be deleted.

Step 6:

Rename the file `connect_xampp.inc.php` in `/path/to/lwt/` to `connect.inc.php`.
```
cd path/to/lwt
mv connect_xampp.inc.php connect.inc.php
```

Step 7:

Edit this file `connect.inc.php` and set the MySQL password in line 
```
$EDITOR /path/to/lwt/connect.int.php
```
find line `$passwd = "";`

Change it to

`$passwd = "abcxyz";`

(or password you wrote in previous steps)

Save the edited file `connect.inc.php`

Move `lwt` folder you just created to this place

`sudo mv path/to/lwt /srv/http/`

Step 8:

Open a Terminal window,

type and execute the following commands (one at a time)
```
sudo chmod -R 755 /srv/http/lwt
sudo systemctl enable --now httpd
sudo systemctl enable --now mariadb
```

Since I don't want these servers to start with the system startup, I wrote a script:
```
#!/bin/sh

sudo systemctl start httpd &&
sudo systemctl start mariadb &&
<browser of your choice> http://localhost/lwt;
```
to use this script you need to give this file permission to execute

`chmod u+x path/to/file`

and place it in your $PATH

to find out what directories in your PATH
```
echo $PATH
mv path/to/file path/to/PATH/directory
```

Step 9:

LWT can now be started in your web browser,

go to: `http://localhost/lwt`.

Step 10:

You may install the LWT demo database, 

or define the first language you want to learn. 

if you used `systemctl ... enable`, after next machine boot, all neccesary services will start automaticaly, if you used `systemctl ... start` variant, you will need manualy start `httpd` and `mariadb` services, after which you can start using lwt in browser.
