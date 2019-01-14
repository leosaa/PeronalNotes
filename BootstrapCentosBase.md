# PeronalNotes
# Bootstrap Centos 6 Base

## install additional progs
```
yum -y install vim lsof xfsprogs wget net-tools
```
## Disable selinux
```
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux
```
## Change hostname
sed -i 's/HOSTNAME=localhost.localdomain/HOSTNAME=ppfe02/'

## I like whole directory in my prompt
```
sed -i 's/\\W\]/\\w\]/' /etc/bashrc
```

## Move var to a new partition, i.e. /mnt
```
mkfs.xfs /dev/xvdf1
mount /dev/xvdf1 /mnt
(cd /var && tar cf - . ) | (cd /mnt && tar xvfp -)
UUID=$(xfs_admin -u /dev/xvdf1  | tr -d " ")
echo "$UUID /var xfs defaults 0 0 " >> /etc/fstab
mv /var /var-old
mkdir /var
mount /var
```

## Installing MySQL
```
rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm
yum -y update
yum install mysql-community-server
/etc/init.d/mysqld start
```

## MySQL root password
```
grep "A temporary password is generated for root@localhost:" /var/log/mysqld.log
2018-08-13T07:23:45.801317Z 1 [Note] A temporary password is generated for root@localhost:XxXxXx
mysql -u root -pXxXxXx
mysql> SET GLOBAL validate_password_policy=LOW;
Query OK, 0 rows affected (0.00 sec)
ALTER USER 'root'@'localhost' IDENTIFIED BY 'secretpassWd';
```

### If you want to reset the root password
```
/etc/init.d/mysqld stop
mysqld_safe --skip-grant-tables --skip-networking &
mysql -uroot
FLUSH PRIVILEGES;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'secretpassWd';
kill `cat /var/run/mysqld/mysqld.pid `
/etc/init.d/mysqld start
mysql -uroot -psecretpassWd
```

## Enable EPEL
```
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
yum localinstall epel-release-latest-6.noarch.rpm
```
## Enable IUS
```
yum install https://$(rpm -E '%{?centos:centos}%{!?centos:rhel}%{rhel}').iuscommunity.org/ius-release.rpm
```

## Installing php 7.1
```
yum install php71u-common.x86_64 php71u-xml.x86_64 php71u-xmlrpc.x86_64 php71u-mcrypt.x86_64\
php71u-pdo.x86_64 php71u-json.x86_64 php71u-gd.x86_64 php71u-mysqlnd.x86_64\
php71u-mbstring.x86_64 php71u-cli php71u-imap 
```

## Installing php 5.6 (Deprecated)
```
yum install yum-utils
yum-config-manager --enable remi-php56
yum update
yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo
```
