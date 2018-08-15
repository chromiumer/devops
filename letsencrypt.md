
## let's encrypt


```
1. install unit

yum install gcc gcc-c++ libffi-devel python-devel openssl-devel

git clone https://github.com/certbot/certbot
cd cerbot
python setup.py install

```

2. gen domain chanins

```

./certbot-auto certonly --manual --email xxx@gmail.com -d www.xxx.com


http://www.xxx.com/.well-known/acme-challenge/xxxxx

cat <<EOF>> /root/certbot/xxxxx
yyyyyy
EOF


nginx config location below:

...

location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   alias /root/certbot/;
}


location = /.well-known/acme-challenge/ {
   return 404;
}
...
```

3. config nginx

```
server {
       listen       443 ssl;
       server_name  www.xxx.com;
       access_log  /usr/local/nginx/logs/www.xxx.com.access.log  main;
       error_log   /usr/local/nginx/logs/www.xxx.com.error.log;

       ssl_certificate /etc/letsencrypt/live/www.xxx.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/www.xxx.com/privkey.pem;

       ssl_ciphers  HIGH:!aNULL:!MD5;
       ssl_prefer_server_ciphers  on;

       location / {
             proxy_pass http://127.0.0.1:8888;
       }
}

```