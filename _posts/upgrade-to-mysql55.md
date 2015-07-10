


To list Old MySql

yum list installed | grep -i mysql
To remove Old MySql

yum remove mysql mysql-*
Remi Dependency on CentOS 6 and Red Hat (RHEL) 6

rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
Install MySQL server

yum --enablerepo=remi,remi-test install mysql mysql-server
To list New MySql

yum list installed | grep -i mysql
start MySql server

/etc/init.d/mysqld start ## use restart after update

OR

service mysqld start ## use restart after update

chkconfig --levels 235 mysqld on
Last

mysql_upgrade -u root -p
Now my MySql version is 5.5.32

Ref:

http://www.webtatic.com/packages/mysql55/

http://www.if-not-true-then-false.com/2010/install-mysql-on-fedora-centos-red-hat-rhel/

