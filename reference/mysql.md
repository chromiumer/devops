#### MySQL install

mysql 5.7 install

useradd -s /sbin/nologin mysql  
mkdir -p /opt/mysql/data  
chown -R mysql:mysql /opt/mysql  
cd /opt/mysql  
bin/mysqld --user=mysql --basedir=/opt/mysql --datadir=/opt/mysql/data --initialize
