# docker-compose.yml
```bash
services:
  helloworld:
    container_nanme: helloword
    image: crccheck/hello-world

  nginx:
    container_name: nginx
    restart: unless-stopped
    image: nginx
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
# for ssl
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot

#container certbot
  certbot:
    image: certbot/certbot
    container_name: certbot
    volumes:
# for ssl
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    command: certonly --webroot -w /var/www/certbot --force-renenel --email blibechmedamine@gmail.com -d azouz.ddns.net --agree-tos
```

  
# nginx.conf
```bash
 events {
    worker_connections 1024;
}

http {
    server_tokens off;
    charset utf-8;

# always redirect to https
    server {
      listen 80 default_server;
      server_name _;

#      location / {
#        proxy_pass http://helloworld:8000/;
#      }

# location of ssl location
      location ~ /.well-know/acme-challenge/ {
        root /var/www/certbot;
      }
      return 301 https://$host$request_uri;
    }
    server {
      listen 443 ssl http2;
      # use the certificates
      ssl_certificate     /etc/letsencrypt/live/azouz.ddns.net/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/azouz.ddns.net/privkey.pem;
      server_name azouz.ddns.net;
      root /var/www/html;
      index index.php index.html index.htm;

      location / {
        proxy_pass http://helloworld:8000/;
      }
# location of ssl location
      location ~ /.well-know/acme-challenge/ {
        root /var/www/certbot;
      }
    }
}
```
# renew certificat in crontab every 5.00 AM, on the first of the month , every second month (febrary) , every year
```bash
crontab -e
```
```bash
0 5 1 */2 * /usr/bin/docker compose -f /home/ubuntu/setup-ssl-nginx-docker/docker-compose.yml up certbot
```

# to deploy django show this article
```bash
https://medium.com/@akshatgadodia/deploying-a-django-application-with-docker-nginx-and-certbot-eaf576463f19
```
