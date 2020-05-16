#### 1.add user
```
useradd -M -s /sbin/nologin mysql
```

#### 2.init installation
```

tar -zxvf mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz -C /data/mysql

mkdir /data/mysql/data

cp /data/mysql/support-files/mysql.server /data/mysql/

/data/mysql/support-files/mysql.server
...
basedir=/data/mysql
datadir=/data/mysql/data
...

touch /data/mysql/error.log

chown -R mysql:mysql /data/mysql

mv /etc/my.cnf /tmp

/data/mysql/bin/mysqld --defaults-file=/data/mysql/my.cnf --initialize --user=mysql --basedir=/data/mysql --datadir=/data/mysql/data

password in /data/mysql/error.log.

/data/mysql/mysql.server start
```
