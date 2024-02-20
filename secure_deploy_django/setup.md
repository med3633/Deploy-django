## 1 settings.DEBUG = False
# Deploy over HTTPS(secure HTTP )  = HTTP over TLS = HTTP over SSL
## 2.use secure Cookie
# SESSION_COOKIE_SECURE = True => to check right click on site develloper setting => stockage => Cookie 
# CSRF_COOKIE_SECURE = TRUE
# Session + CSRF cookies only via HTTPS!
## 3. use HSTS (Http Strict Transport Security)
# SECURE_HSTS_SECONDS = 31536000 (=1 year)
# SECURE_HSTS_INCLUDE_SUBDOMAINS = True
# SECURE_HSTS_PRELOAD = True
# Please, HTTPS only!
## 4. SECURE_SSL_REDIRECT = True => secure http request
![image](https://github.com/med3633/Deploy-django/assets/160378998/578f2d13-fcb8-4842-9820-55fdcf53422c)
![image](https://github.com/med3633/Deploy-django/assets/160378998/7f884641-b32a-4f89-9278-5a7e898d6b01)

![image](https://github.com/med3633/Deploy-django/assets/160378998/f34fb374-b12b-4cb0-9ca1-45c9dc776da2)

# check your sttings file when there are red flags 

![image](https://github.com/med3633/Deploy-django/assets/160378998/74d56a5f-c7f6-4ca8-9672-4e373fe8bd91)

```bash
./manage.py check --deploy
```


