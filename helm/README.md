# Install dependencies

* apt install certbot nginx python3-certbot-nginx ufw

# UFW

Run

```shell
ufw allow ssh
ufw allow http
ufw allow https
ufw enable
```

# Nginx

* Create a Nginx config file in /etc/nginx/sites-available/elastic.example.com:

```nginx
upstream elastic {
    server 127.0.0.1:5601;
}

server {
    listen 80;
    server_name elastic.example.com;

    location / {
        proxy_pass  http://elastic;
    }
}
```

* Enable, configure and start Nginx

```shell
cd /etc/nginx/sites-enabled
ln -s ../sites-available/elastic.example.com .
nginx -t
certbot
```
