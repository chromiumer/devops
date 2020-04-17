```
user  root;
worker_processes  auto;

error_log  logs/error.log;
error_log  logs/error.log  notice;
error_log  logs/error.log  info;

pid        logs/nginx.pid;

events {
    use   epoll;
    worker_connections  65535;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

	log_format main '$remote_addr $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '$http_user_agent $http_x_forwarded_for $request_time $upstream_response_time $upstream_addr $upstream_status';

    access_log  logs/access.log  main;

    sendfile        on;
    tcp_nopush     on;
    keepalive_timeout  90;
    gzip on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types       text/plain text/javascript text/css application/json application/javascript application/x-javascript application/xml image/svg+xml image/jpeg image/gif image/png;
    gzip_vary on;

    large_client_header_buffers 4 32k;
    server_names_hash_bucket_size 128;
    open_file_cache max=204800 inactive=20s;
    open_file_cache_min_uses 1;
    open_file_cache_valid 30s;

    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    client_max_body_size 100m;
    client_body_buffer_size 256k;
    proxy_connect_timeout 300;
    proxy_send_timeout 300;
    proxy_read_timeout 300;
    proxy_buffer_size 128k;
    proxy_buffers 32 32k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size 128k;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    server {
        listen       80;
        server_name  localhost;

        charset koi8-r;
        access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        error_page  404              /404.html;
        
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    include sub/*.conf;

}
```
