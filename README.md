# Reverse-Proxy-for-Google
[Nginx] configurar Nginx como un proxy inverso para Google
solo hay que modificar el archivo default de la carpeta sites-available
nano /etc/nginx/sites-available/default        y pegar lo siguiente:

#### Configuration
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass https://www.google.com;
        proxy_set_header Host www.google.com;
        proxy_set_header Referer https://www.google.com;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        sub_filter google.com example.com;
        sub_filter_once off;
    }
}
```
OK, type in your domain name on the browser, you can access Google now, even in `China`. Keep on going
<br>
#### Using SSL
```nginx
server {
    listen 80;
    server_name example.com;
    location / {
        rewrite ^/(.*)$ https://$host$1 permanent;
    }
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate      /etc/ssl/example.crt;
    ssl_certificate_key  /etc/ssl/example.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    location / {
        proxy_pass https://www.google.com;
        proxy_set_header Host www.google.com;
        proxy_set_header Referer https://www.google.com;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        sub_filter google.com example.com;
        sub_filter_once off;
    }
}
```
esta funciona para v2ray local y remoto. y ademas para google

server {
    listen 8080;
    server_name example.com;

    location /trl3aEyg/ {
        proxy_pass http://www.pararay.tk;
        proxy_set_header Host www.parary.tk;
        proxy_set_header Referer http://www.pararay.tk;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        #proxy_set_header Host $host;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP 123.123.123.123;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For 123.123.123.123;

        sub_filter google.com example.com;
        sub_filter_once off;
    }
}
