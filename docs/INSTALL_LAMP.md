
# LAMP

Install LAMP - Linux Apache(Nginx) MySQL PHP.

* [Установка Linux](#install-linux)
* [Установка Nginx](#install-nginx)
* [Усьановка MySQL](#install-mysql)
* [Установка PHP](#install-php)


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

<details><summary>В ответ вернется что-то вроде этого:</summary>

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

</details>

## Install MySQL

Устанавливаем БД сервер MySQL.

```bash
sudo apt-get install mysql-server
```

После установки необходимо создать под себя пользователя, т.к. под root пользователем нельзя работать.
А также создать базу данных с которой мы потом будем работать и предоставить полные права созданому пользователю.

Нужно войти в БД

```bash
sudo mysql -u root
```

Откроется командная строка БД, где создадим пользователя. В моем случаи имя пользователя `den`.

```sql
CREATE USER den IDENTIFIED BY 'your_password';
```

Далее создаем базу данных `smsgw`.

```sql
CREATE DATABASE smsgw;
```

После этого назначаем полные права на эту БД нашему пользователю. И обновляем привелегии.

```sql
GRANT ALL PRIVILEGES ON smsgw.* TO den;
FLUSH PRIVILEGES;
```

В дальнейшем практически вся работа с БД будет проводится через прослойку PHP абстракций. Но знание SQL строго рекомендуется, т.к. анализ данных в БД в последствии очень важен. 

## Install PHP

Устанавливаем пакеты PHP:

```bash
sudo apt-get install php-cli php-fpm php-curl php-gd php-enchant php-bcmath php-bz2 php-imap php-intl php-json php-mbstring php-mcrypt php-soap php-xml php-xmlrpc php-zip php-redis php-yaml php-mysql php-xdebug
```

В нашей конфигурации окружения мы будем использовать связку `nginx` + `php-fpm`, [FPM - FastGCI Process Manager](http://php.net/manual/ru/install.fpm.php).

Для проверки работоспособности `php-fpm` нам нужно настроить хост в `nginx` и создать тестовый php скрипт.

Создаем в директории `/var/www/` новый каталог `lamp`, а в этом каталоге создаем `index.php` файл.

```bash
sudo mkdir /var/www/lamp
echo '<?php phpinfo(); ?>' | sudo tee /var/www/lamp/index.php > /dev/null
```

Добавляем новое доменное имя в `/etc/hosts`

```bash
sudo nano /etc/hosts
```

![Меняем доменное имя](https://github.com/hurdos/smsgw/blob/master/imgs/lamp2hosts.png).
