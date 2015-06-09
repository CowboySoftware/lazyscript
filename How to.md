
# Odoo + ubuntu

##### สิ่งแรกที่ต้องทำ

* เข้าไป  update และ upgrade
```sh
$ sudo apt-get update
$ sudo apt-get upgrade
```
``
ในกรณีที่มี Distribute upgrade ให้ ทำการ upgrade ด้วย (apt-get dist-upgrade)
``
* ทำการ reboot เครื่อง
```sh
$ sudo reboot
```

* เข้าเพิ่ม user ในระบบสำหรับ Odoo 
```sh
$ sudo adduser --system --home=/opt/odoo --group odoo
```
* ทำการติตตั้ง PostgresSQL
```sh
$ sudo apt-get install postgresql
```
* หลังจากติดตัง PostgresSQL เข้าไปใช้งาน User ของ Postgres เพื่อไปสร้าง User สำหรับ Database
```sh
$ sudo su - postgres
```

* สร้าง User ใน Database
```sh
$ createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo
```

* ใส่รหัสสำหรับ User ใน Database
```sh
$ Enter password for new role: ********
$ Enter it again: ******** 
```
* ไป Download script ที่สร้างขึ้นมา
```SH
$ wget https://raw.githubusercontent.com/CowboySoftware/lazyscript/master/python-odoo
```
* เข้าไปใช้ user ของ odoo
```SH
$ sudo su - odoo -s /bin/bash
```
* เข้าไป Download  Odoo มา
```
 $ git clone https://www.github.com/odoo/odoo --depth 1 --branch 8.0 --single-branch 
```
* แล้ว exit จาก odoo user

* สร้าง directory สำหรับ ตั้งค่า Odoo 
```sh
$ sudo mkdir /etc/odoo
```
* ไปคัดลอกไฟล์ตั้งค่า จาก  odoo GIT ไปไหวใน etc/odoo
```sh
$ sudo cp /opt/odoo/odoo/debian/openerp-server.conf /etc/odoo/odoo-server.conf
```
* ไปเปลี่ยน permission ของไฟล์ Config นั้น
```sh
$ sudo chown odoo: /etc/odoo/odoo-server.conf
```
* เข้าไปตั้งค่า Access ของไฟล์นั้นๆ 
```sh
$ sudo chmod 640 /etc/odoo/odoo-server.conf
```

* เข้าไปทำการแก้ไข ไฟล์ตั้งค่า
```sh
$ sudo vi /etc/odoo/odoo-server.conf
```
* เข้าไปเปลี่ยนค่า เช่น
```
db_password = รหัสที่เราตั้งไว้
addons_path = /opt/odoo/odoo/addons
;add log file path
logfile = /var/log/odoo/odoo-server.log
```
* เข้าไปตั้งค่า startup โดยไปคัดลอกจากไฟล์Startup ใน Odoo 
```
$ sudo cp /opt/odoo/odoo/debian/init /etc/init.d/odoo-server
```
* เข้าไปแก้ไข ไฟล์ startup 
```sh
$ sudo vi /etc/init.d/odoo-server
```
```sh
DAEMON = /opt/odoo/odoo/openerp-server
Config = /etc/odoo/odoo-server.conf
```
* เข้าไปเปลี่ยนสิทธิ์และการเข้าถึงไฟล์ Startup
```
$ sudo chown root: /etc/init.d/odoo-server
$ sudo chmod 755 /etc/init.d/odoo-server 
```
* เข้าไสร้าง Folder เก็บ log 
```
$ sudo mkdir /var/log/odoo
```
* เข้าไปเปลี่ยนสิทธิ์ ของ log
```
$ sudo chown odoo:root /var/log/odoo
```

* หลังจากนั้นทดลอง start odoo
```
$ sudo /etc/init.d/odoo-server start
```
* ถ้าใช้งานได้แล้วทำการ update startup
```
$ sudo update-rc.d odoo-server defaults
```
