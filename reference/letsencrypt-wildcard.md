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
