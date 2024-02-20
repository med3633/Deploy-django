![image](https://github.com/med3633/Deploy-django/assets/160378998/b6fa8847-af50-4caf-befa-943156a95eaa)
# clone this project
```bash
https://github.com/med3633/docker-mastery-with-django
```
# make v env
```bash
py -m ven venv
```
# install dependencies
```bash
pip install -r requirements.txt
```
# to start django 
```bash
py manage.py runserver
```
# start react
```bash
npm i
npm start
```
# requirements.txt
```bash
asgieref==3.3.4
coverage==5.5
Django==3.2
django-cors-headers==3.7.0
djangorestframework==3.12.4
pytz=2021.
sqlparse==0.4.1
gunicorn==20.1.0
```
# -------------------
# conteneurize
# -------------------
```bash
FROM python:3.8-alpine
# output
ENV PYTHONUNBUFFERED 1
# directory on container
WORKDIR /django
# copy requirement in working directory django
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
```
```bash
FROM node:15.13-alpine
WORKDIR /react
ENV PATH="./node_modules/.bin:$PATH"
COPY . .
# to get build folder of my app react to be serve in production
RUN npm run build
```
```bash
version: '3'
services:
  backend:
    build:
      context: ./django
    command: gunicorn todo.wsgi --bind 0.0.0.0:8000
    ports:
      - "8000:8000"
  frontend:
    build:
      context: ./react/bloagapi
    volumes:
# bind = auto update in container , volume = specifier place ,be access by your web server (nginx) => replace build folder of react and allw nginx
      - react_build:/react/build
  nginx:
    image: nginx:latest
    ports:
      - 80:8080
    volumes:
      -   ./nginx/nginx-setup.conf:/etc/nginx/conf.d/default.conf:ro
      - react_build:/var/www/react
    depends_on:
      - backend
      - frontend

#initialoze volume
volumes:
  react_build:
```
# setup nginx to server react app 
```bash
server {
#connect to react in www
 listen 8080;
location / {
root /var/www/react
}
} 
```
## we find volume in windows
```bash
\\wsi$\docker-desktop-data\version-pack-data\community\docker\volumes\
```
 
