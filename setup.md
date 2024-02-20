![image](https://github.com/med3633/Deploy-django/assets/160378998/6c4782a2-af00-45f2-8021-902713c1bd4b)
# create django project 
```bash
django -admin startproject django_project
```
# start app
```bash
python mange.pyy startapp todo
```
# add requirement.txt for django project inside django_project 
```bash
Django==3.0.8
gunicorn==20.0.4
```
## add Dockerfile inside django_project 
```bash
FROM python:3.8.5-alpine
# directory of working
WORKDIR app
#upgrade pip
RUN pip install --upgrade pip
# copy req insde /app
COPY ./requirements.txt .
# install req
RUN pip install -r requiremennts.txt
# copy todo inside /app
COPY ./todo /app
#copy bash file in racine
COPY ./entrypoint.sh /
#excute the bash script
ENTRYPOINT ["sh", "/entrypoint.sh"]
```
## create entrypoint file inside django_project
```bash
#!/bash/bin
# I will create the step of running django project
python manage.py migrate --no-input
# collect all static file needed for this
python manage collectstatic --no-input
#run django server
gunicorn todo.wsgi:application --bind 0.0.0.0:8000
```
# create docker compose django_projet
```bash
version: '3.7'
services:
  backend:
    volumes:
      - static:/static
# have env variable for u backend container
    env_file:
      - .env
    build:
      context: .
    ports:
      - "8000:8000"
  nginx:
    build: ./nginx
    volumes:
      - static:/static
    ports:
      - "80:80"
    depends_on:
      - backend

volumes:
  static:
```
# create file .env in todo
```bash
SECRET_KEY='dgfdgdhfn,ggnnjfhfhdfbgnxgxfdhhg=='
DEBUG=True
```
# update the setting.py
```bash
SECRET_KEY = os.getenv('SECRET_KEY')
DEBUG= os.getenv('DEBUG')
ALLOWED_HOST = [*]
STATIC_ROOT = '/static/'
STATIC_URL = '/static/'
```
#  create nginx folder inside django_project and create on a default.conf
```bash
upstream django {
  server backend:8000
}

server {
 listen 80;
 location / {
  proxy_pass http://django
}
location /static/ {
  alis /static/
}
}
```
# create nginx folder inside django_project and create on a Dockerfile
```bash
FROM nginx:1.19.0-alpine
#copy the configuration file of nginx
COPY ./default.conf /etc/nginx/conf.d/default.conf
```
# in terminal
```bash
docker-compose up --build
```






