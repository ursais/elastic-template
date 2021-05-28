# Installation

## Table of Contents
* [Dependencies](#Dependencies)
* [Kernel](#Kernel)
* [UFW](#UFW)
* [Nginx](#Nginx)
* [Systemd](#Systemd)

## Dependencies

```shell
apt install certbot git nginx python3-certbot-nginx ufw
cd /opt
git clone https://github.com/ursais/elastic-template elastic
```

## Kernel

Run
```shell
sysctl vm.max_map_count=262144
```

and the following at the end of `/etc/sysctl.conf`:
```unit file (systemd)
vm.max_map_count=262144
```

## UFW

Run

```shell
ufw allow ssh
ufw allow http
ufw allow https
ufw enable
```

## Nginx

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

## Systemd

Create `/etc/systemd/system/elastic.service` with:

```unit file (systemd)
[Unit]
Description=Elastic container starter
After=docker.service network-online.target
Requires=docker.service network-online.target

[Service]
WorkingDirectory=/opt/elastic
Type=oneshot
RemainAfterExit=yes

ExecStartPre=-/usr/local/bin/docker-compose pull --quiet
ExecStart=/usr/local/bin/docker-compose -f docker-compose.yml up -d

ExecStop=/usr/local/bin/docker-compose -f docker-compose.yml down

ExecReload=/usr/local/bin/docker-compose pull --quiet
ExecReload=/usr/local/bin/docker-compose -f docker-compose.yml up -d

[Install]
WantedBy=multi-user.target
```

and run:
```shell
systemctl daemon-reload
systemctl enable elastic
service elastic start
```
