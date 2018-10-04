
# LAMP

Install LAMP - Linux Apache(Nginx) MySQL PHP.


## Install Linux

Устанавливаем `debian based linux` например [Mint](https://linuxmint-installation-guide.readthedocs.io/ru/latest/) или чистый [Debian](https://www.debian.org/releases/stable/i386/index.html.ru).
После устаовки надо обновить репозиторий пакетов:
```bash
sudo apt-get update
```

## Install Nginx

Устанавливаем web server Nginx, на текущий момент считается самым продвинутым для LAMP стека.

Устанавливаем следующие пакеты:
```bash
sudo apt-get install nginx curl
``` 

Пакет `curl` нужен нам для проверки работоспособности `nginx` сервера.

Проверяем следующим образом:
```bash
curl -v http://localhost/
```

В ответ вернется что-то вроде этого:
```
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 80 (#0)
> GET / HTTP/1.1
> Host: localhost
> User-Agent: curl/7.52.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.10.3
< Date: Thu, 04 Oct 2018 15:40:15 GMT
< Content-Type: text/html
< Content-Length: 612
< Last-Modified: Thu, 04 Oct 2018 15:37:59 GMT
< Connection: keep-alive
< ETag: "5bb633d7-264"
< Accept-Ranges: bytes
< 
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
* Curl_http_done: called premature == 0
* Connection #0 to host localhost left intact
```

## Install MySQL