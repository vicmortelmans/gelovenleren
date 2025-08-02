---
title: "Hosting websites on a virtual private server to save costs"
date: 2022-12-21T08:52:11+01:00
categories: 
  - "blog"
tags:
  #- "theologie"
  #- "vrijheid"
  #- "politiek"
  #- "liturgie"
  #- "traditie"
  #- "biecht"
  #- "ethiek"
  #- "aanbidding"
  #- "sacramenten"
  #- "paus"
  #- "vlaanderen"
  #- "opvoeding"
  #- "verbeelding"
  #- "gebed"
  #- "boeken"
  #- "films"
  #- "bijbel"
  #- "woke"
  #- "antwerpen"
  - "publicaties"
  #- "kerstmis"
  #- "pasen"
  #- "kerkleer"
  #- "onderwijs"
  #- "islam"
  #- "leven"
  #- "synode"
---

*(dit artikel verschijnt per uitzondering in het Engels)*

[Versio](https://www.versio.nl/), my web host since more than 10 years, has decided to "redefine the subscription plans". Up till now I could use a basic plan costing less than EUR 80 per year to host my 14 small hobby websites, a mix of wordpress sites, static sites and some specific tools like phplist (mailing lists, like mailchimp, but free) and tiny tiny rss (managing rss feeds, like feedly, but free). "Upgrading" to the [new plans](https://www.versio.nl/webhosting) (which offer less and cost more) would cost me more than EUR 500 per year, a **cost increase of 500%**. Am I allowed to call that greedy? 

Obviously they loose me as a customer, but I have to think of a plan B. Migrating websites is not easy. Many people will take the hit and settle with the cost increase rather than investing much time in a migration. I have been busy with it full time for the last three days. In this article I will describe how I set it up. It's not easy and you need quite some linux skills. 

My first idea was to ge really cheap and host the websites on a **raspberry pi** that I have in my home. Technically it is possible, but it requires dynamic DNS, because a home network has no fixed IP address. Setting up DynDNS isn't that hard, but there's a flaw in the concept that causes locally hosted sites to be inaccessible from within the home network. I did some research and to solve this, my router would need a "loopback NAT' function, but this is not available on the equipment that is used by my provider (Scarlet, same goes for Proximus and Telenet and probably all other providers in Belgium). 

So I chose to rent a **virtual private server** (VPS) from Vimexx. I picked the [cheapest option](https://www.vimexx.be/budget-vps), featuring 1 CPU core, 2 GB RAM and 50GB storage and an Ubuntu linux distro, which will cost me EUR 60 per year. It's a bare linux system, so I have to do a lot more of configuration to set it up as a webserver for my sites.

I roughly based myself on this tutorial:  [How to host a WordPress website on a Raspberry Pi with Raspbian Buster Lite and Nginx](https://www.techcoil.com/blog/how-to-host-a-wordpress-website-on-a-raspberry-pi-with-raspbian-buster-lite-and-nginx/).

- [Step 1: setting up a user account for safely accessing the server using ssh](#step-1-setting-up-a-user-account-for-safely-accessing-the-server-using-ssh)
- [Step 2: installing the packages that compose the "LEMP stack"](#step-2-installing-the-packages-that-compose-the-lemp-stack)
- [Step 3: restore the files and---if applicable---the database, using a backup file](#step-3-restore-the-files-and---if-applicable---the-database-using-a-backup-file)
- [Step 4: setup the web server for http](#step-4-setup-the-web-server-for-http)
- [Step 5: configure DNS](#step-5-configure-dns)
- [Step 6: generate a certificate for your domain](#step-6-generate-a-certificate-for-your-domain)
- [Step 7: setup the web server for https](#step-7-setup-the-web-server-for-https)
- [Troubleshooting](#troubleshooting)

## Step 1: setting up a user account for safely accessing the server using ssh

1. use ssh to login to the root account using the password that is provided by email after activating the server \
`ssh root@37.97.145.69`
2. create a user "vps" and add it to sudo group and set default shell \
`useradd -m vps` \
`passwd vps` \
`usermod --shell /bin/bash vps` \
`usermod -a -G sudo vps` \
(note that the sudo group won't apply until next login with vps)
3. disable ssh login for root  \
update the file `/etc/ssh/sshd_config` \
change: `PermitRootLogin no`
4. setup login using key for vps (to be done from my local pc, using the IP address of the server) \
`ssh-copy-id vps@37.97.145.69`
5. disable password login for vps (check if login using key works first!!) \
update the file `/etc/ssh/sshd_config` \
change: `PasswordAuthentication no`

## Step 2: installing the packages that compose the "LEMP stack"

**L:** Linux, the OS

**E**: Nginx, the web server

**M**: MariaDB, the database used by Wordpress and other applications

**P**: PHP, the dynamic webpage rendering language

1. `sudo apt install nginx mariadb-server mariadb-client php-fpm php-mysql php-json php-curl php-dom php-imagick php-mbstring php-zip php-gd php-intl certbot` \
Remark: don't install the `php` package, it will automatically install the Apache web server, conflicting with Nginx!
2. add a php upstream section to `/etc/nginx/nginx.conf` \
This allows the Nginx web server to address the php service for executing dynamic files. 
```
    ##
    # Virtual Host Configs
    ##

    upstream local_php {
            server unix:/run/php/php7.4-fpm.sock;
    }
```
3. create a file `dhparam.pem`, this has got something to do with SSL certificates, don't ask me exactly what \
`sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048`

Now the server is ready to start migrating a website. Let's assume that the website has www.example.com (and example.com) as an address.

## Step 3: restore the files and---if applicable---the database, using a backup file

My backup file (`example.com.tar.gz`) is made with softaculous (this is the application manager that was used for managing my account on Versio). 

1. `cd /var/www`
2. `sudo mkdir example.com`
3. `cd example.com`
4. `sudo tar xvzf example.com.tar.gz`
5. `sudo chown -R www-data:www-data *`

If the site uses the database, find among the extracted php files the credentials for the database. For a Wordpress site, they're in `wp-config.php`. You need the database name (e.g. vicmouo145_wp7), user name (typically identical to the database name) and password (e.g. abc123). The backup file contains apart from the files for the website also one special file `softsql.sql` that contains the data from the database. 

6. `sudo mariadb`
    1. `create database vicmouo145_wp7;`
    2. `create user vicmouo145_wp7@localhost identified by 'abc123';`
    3. `grant all on vicmouo145_wp7.* to vicmouo145_wp7@localhost;`
    4. `use vicmouo145_wp7;`
    5. `source softsql.sql`

## Step 4: setup the web server for http

Later on, https access will be configured as well, but http has to be activated first, before creating the certificate required for https.

1. create a file `/etc/nginx/sites-enabled/example.com.conf`
```
        server {
                listen 80;
                server_name example.com www.example.com;
                root /var/www/example.com;
                index index.php;

                location = /favicon.ico {
                        log_not_found off;
                        access_log off;
                }

                location = /robots.txt {
                        allow all;
                        log_not_found off;
                        access_log off;
                }

                location ~ /.well-known {
                        allow all;
                }

                location / {
                        try_files $uri $uri/ /index.php?$args;
                }

                location ~ \.php$ {
                        include fastcgi.conf;
                        fastcgi_intercept_errors on;
                        fastcgi_pass local_php;
                        fastcgi_buffers 16 16k;
                        fastcgi_buffer_size 32k;
                }

                location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                        expires max;
                        log_not_found off;
                }
        }
```
2. restart Nginx to make this effective \
`sudo systemctl restart nginx.service` \
I experienced that Nginx is very picky on the syntax of the configuration files. For indentation you should only use tabs, no spaces, and empty lines should be really empty, no spaces allowed! Error messages aren't very clear, so getting these files straight took me quite some time!!

## Step 5: configure DNS

This step is not performed on the server. There's a company that is selling you the domain name, this can be the hosting company (Vimexx), but I obtain my domain names from [bNamed](https://www.bnamed.net/en/index.jsp). There you typically have a login to manage your domain. You have to add an A-record containing the IP address of your server to link your domain to your server.

It can take a while before this link is effective. Do a `"ping www.example.com"` on your PC to check if it returns the IP address of your new server. If it does, you can check the website in your browser. It will probably complain about your site not being safe. That's because it has no https access, so we'll see to that now.

## Step 6: generate a certificate for your domain

1. `sudo certbot certonly -a webroot --webroot-path=/var/www/example.com -d example.com -d www.example.com` \
If successful, it says the certificate will expire in three months and to non-interactively renew all of your certificates, you can run  "certbot renew", but it looks like there's a [timer active for that already](https://community.letsencrypt.org/t/cerbot-cron-job/23895/5). Not sure myself if this will work…

## Step 7: setup the web server for https

The proposed setup will automatically redirect http addresses to https.

1. update `/etc/nginx/sites-enabled/example.com.conf`
```
        server {
                listen 80;
                server_name example.com www.example.com;
                return 301 https://$host$request_uri;
        }

        server {
                ssl on;
                ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
                ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
                ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
                ssl_prefer_server_ciphers on;
                ssl_dhparam /etc/ssl/certs/dhparam.pem;
                ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
                ssl_session_timeout 1d;
                ssl_session_cache shared:SSL:50m;
                ssl_stapling on;
                ssl_stapling_verify on;
                add_header Strict-Transport-Security max-age=15768000;

                listen   443;

                server_name example.com www.example.com;
                root /var/www/example.com;
                index index.php;

                location ~ /.well-known {
                        allow all;
                }

                location = /favicon.ico {
                        log_not_found off;
                        access_log off;
                }

                location = /robots.txt {
                        allow all;
                        log_not_found off;
                        access_log off;
                }

                location / {
                        try_files $uri $uri/ /index.php?$args;
                }

                location ~ \.php$ {
                        include fastcgi.conf;
                        fastcgi_intercept_errors on;
                        fastcgi_pass local_php;
                        fastcgi_buffers 16 16k;
                        fastcgi_buffer_size 32k;
                }

                location ~ \.php$ {
                        include fastcgi.conf;
                        fastcgi_intercept_errors on;
                        fastcgi_pass local_php;
                        fastcgi_buffers 16 16k;
                        fastcgi_buffer_size 32k;
                }

                location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                        expires max;
                        log_not_found off;
                }
        }
```
2. restart Nginx again to make this effective \
`sudo systemctl restart nginx.service`

Here's the config file for a static website (= not using PHP):

```
        server {
                listen 80;
                server_name example.com www.example.com;
                return 301 https://$host$request_uri;
        }

        server {
                ssl on;
                ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
                ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
                ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
                ssl_prefer_server_ciphers on;
                ssl_dhparam /etc/ssl/certs/dhparam.pem;
                ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
                ssl_session_timeout 1d;
                ssl_session_cache shared:SSL:50m;
                ssl_stapling on;
                ssl_stapling_verify on;
                add_header Strict-Transport-Security max-age=15768000;

                listen   443;

                server_name example.com www.example.com;
                root /var/www/example.com;
                index index.html;

                location ~ /.well-known {
                        allow all;
                }

                location = /favicon.ico {
                        log_not_found off;
                        access_log off;
                }

                location = /robots.txt {
                        allow all;
                        log_not_found off;
                        access_log off;
                }

                location / {
                        try_files $uri $uri/ /index.html;
                }

                location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                        expires max;
                        log_not_found off;
                }
        }
```

## Troubleshooting

So far I had only one problem. Suddenly all my dynamic sites (= the ones using PHP) would return error 504 and 502. This was because PHP stopped working (it's a service running in the background). 

To get more info on the root cause, I added logging for slow request in `/etc/php/7.4/fpm/pool.d/www.conf`:

`slowlog = /var/log/php_slow.log`

`request_slowlog_timeout = 10s`

The report in the log revealed that requests by my phplist website were slow. So I assume they get the blame of crashing my php-fpm, but I cannot solve that myself. 

After applying following settings to control how to handle slow requests (by trial and error, not sure which change exactly did the trick, or even which are actually changed from default), things appear to be more stable:

In `/etc/php/7.4/fpm/pool.d/www.conf`:

`pm = ondemand`

`pm.max_requests = 10`

`pm.max_children = 15`

`pm.process_idle_timeout = 10s`

`request_terminate_timeout = 60s`

`request_terminate_timeout_track_finished = yes` 

In `/etc/php/7.4/fpm/php.ini`:

`max_execution_time = 30` 

`default_socket_timeout = 60` 

In `/etc/nginx/nginx.conf`:

`keepalive_timeout 55;`

`fastcgi_read_timeout 60;`

`fastcgi_read_timeout 60;`


