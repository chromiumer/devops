#### MySQL install

mysql 5.7 install

wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz  
tar -xvf mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz -C /opt/mysql

useradd -s /sbin/nologin mysql  
mkdir -p /opt/mysql/data  
chown -R mysql:mysql /opt/mysql  
cd /opt/mysql  
bin/mysqld --user=mysql --basedir=/opt/mysql --datadir=/opt/mysql/data --initialize
