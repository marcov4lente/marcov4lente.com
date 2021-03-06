---
layout: article
title:  "Installing a Letsencrypt.org SSL certificate on an Ubuntu 16.04 server"
date:   2017-04-01 10:06:54 +0000
description: It's currently staggeringly easy to intercept HTTP (non encrypted) data, especially whilst connected to public WiFI networks. Any website that has a login form and/or deals with user-submitted information should use HTTPS to protect it's user's privacy and data.
categories: Linux
permalink: articles/installing-a-letsencrypt-ssl-certificate-on-an-Ubuntu-16-04-xenial-server.html
---
It's currently staggeringly easy to intercept HTTP (non encrypted) data, especially whilst connected to public WiFI networks. Any website that has a login form and/or deals with user-submitted information should use HTTPS to protect it's user's privacy and data.

This is where [Letsencrypt](https://letsencrypt.org/about/) comes to the rescue! [Letsencrypt](https://letsencrypt.org/about/) is a free and open SSL Certificate authority that's backed by some big names in the internet industry. Installing and using a [Letsencrypt](https://letsencrypt.org/about/) certificate is simple and takes literally minutes to set up.

## Install Certbot
Add the Certbot repositories and install it.
```
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot
```


## Install the SSL certificate
Using the Webroot option, specify the server root and the domain name of the website.
```
$ sudo certbot certonly --webroot -w /var/www/marcovalente.io/ -d marcovalente.io
```

Remember to respond to the interactive command prompts to complete the installation.

If this process fails with a "...Failed authorization procedure ... urn:acme:error:unauthorized :: The client lacks sufficient authorization..." kind of error, enable access to the .well-known directory that Certbot will create from the NginX config.

```
    location ~ \.well-known {
        try_files $uri $uri/ =404;
    }
```

Once done, a success message, similar to the one below, should pop up.
```
...
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/example.com/fullchain.pem. Your
...
```


## Configure Nginx
Inside the main server block: configure the SSH certificate.
```
    # listen 80;
    # listen [::]:80;

    listen 443 ssl;
    listen [::]:443;

    ssl_certificate /etc/letsencrypt/live/marcovalente.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/marcovalente.io/privkey.pem;
```

Outside the main server: block add an additional server block to redirect HTTP traffic to HTTPS.
```
    server {
        listen 80;
        server_name example.com www.example.com;
        return 301 https://$host$request_uri;
    }
```

Test for NginX configuration file syntax errors.
```
$ sudo nginx -t
```

Restart NginX.
```
$ sudo service nginx restart
```

## Automatic certificate renewal
Automating the certificate renewal can be done via the system's crontab.
```
$ sudo crontab -e
```

Add the following to the crontab.
```
30 2 * * 1 /usr/local/sbin/certbot-auto renew >> /var/log/le-renew.log
35 2 * * 1 /etc/init.d/nginx reload
```



