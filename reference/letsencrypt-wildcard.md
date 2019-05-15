### 使用 let's encrypt 申请通配符域名

1.命令
```
wget https://dl.eff.org/certbot-auto

chmod +x certbot-auto

./certbot-auto certonly  -d "*.chromiumer.com" -d "chromiumer.com" --manual --preferred-challenges dns-01  --server https://acme-v02.api.letsencrypt.org/directory

```

2.添加记录

```
根据提示对域名添加两条TXT记录验证域名所有权.
```

3.证书文件
```
文件列表
/etc/letsencrypt/archive/chromiumer.com/cert1.pem
/etc/letsencrypt/archive/chromiumer.com/chain1.pem
/etc/letsencrypt/archive/chromiumer.com/fullchain1.pem
/etc/letsencrypt/archive/chromiumer.com/privkey1.pem

Nginx配置
......
  ssl_certificate /etc/letsencrypt/archive/chromiumer.com/fullchain1.pem;
  ssl_certificate_key /etc/letsencrypt/archive/chromiumer.com/privkey1.pem;
  ssl_ciphers  HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers  on;
......

```
