---
layout: article
title:  "LEMP (Linux, NginX, MySQL, PHP) server set up"
date:   2016-12-10 17:54:54 +0000
description: LEMP referes to a web server that runs Linux, NginX, MySQL, PHP. NginX. NginX, an alternative to Apache, is usually a little snapper than Apache..
categories: Linux
permalink: articles/lemp-linux-nginX-mysql-php-server-set-up.html
---
LEMP refers to a web server that runs Linux, NginX, MySQL, PHP. NginX. NginX, an alternative to Apache, is usually a little snapper than Apache.


## Prepartion
Update / upgrade the operating system by installing any pending system updates.
```
$ sudo apt-get update
$ sudo apt-get -y upgrade
```


## Installation
Install nginx
```
$ sudo apt-get install -y nginx
```

Enable UFW access if UFA is active.
```
$ ufw allow 'Nginx HTTP'
```

Install MySQL server.
```
$ apt-get install -y mysql-server
```

Install PHP 7.0, along with a couple of PHP extras.
```
$ sudo apt-get install -y php7.0
$ sudo apt-get install -y php-fpm
$ sudo apt-get install -y php-mysql
$ sudo apt-get install -y php-mbstring
$ sudo apt-get install -y php-gettext
$ sudo apt-get install -y php-xml
$ sudo apt-get install -y php-zip
```

Configure PHP.
```
$ nano /etc/php/7.0/fpm/php.ini
```

Set:
```
cgi.fix_pathinfo = 0
```

Restart nginx-php.
```
$ systemctl restart php7.0-fpm
```

Restart nginx.
```
$ systemctl reload nginx
```

Install git.
```
$ apt-get install git
```

Install composer.
```
$ curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

Install phpunit
```
$ wget https://phar.phpunit.de/phpunit-6.0.phar
$ chmod +x phpunit-6.0.phar
$ sudo mv phpunit-6.0.phar /usr/local/bin/phpunit
$ phpunit --version
```
