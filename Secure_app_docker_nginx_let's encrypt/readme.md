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
  

```bash
 events {
    worker_connections 1024;
}

http {
    server_tokens off;
    charset utf-8;

    server {
      listen 80 default_server;
      server_name _;
      location / {
        proxy_pass http://helloworld:8000/;
      }
    }
}
```
