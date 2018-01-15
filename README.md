# docker-lnmp-with-mutli-php-versions

[(中文版说明)](https://or2.in/2018/01/13/docker-compose-lnmp-multi-php-version/)

Because of my work needs, I need to maintain somw old system which use php5.3, so I need a more rapid deployment system tools.
And that's what I going to share with you.

Create the LNMP with Docker (Use the php5.3, 5.6, 7.2 to deployment is available)


```
                 +- - - - - - - - - - - - - - - - - - - - - - - -+
                 ' docker:                                       '
                 '                                               '
+------+  80     ' +-------+    9000    +-----------+            '
|      | ------> ' |       |   <------> | PHP-FPM53 | <------+   '
|      |         ' |       |            +-----------+        |   '
|      |         ' |       |    9000    +-----------+        |   '
|      |         ' |       |   <------> | PHP-FPM56 | <------+   '
|      |         ' | Nginx |            +-----------+        |   '
| User |         ' |       |    9000    +-----------+        |   '
|      |         ' |       |   <------> | PHP-FPM72 | <------+   '
|      |         ' |       |            +-----------+        |   '
|      |         ' |       |                                 |   '
|      |         ' +-------+                                 |   '
|      |  3306   ' +-------+    3306                         |   '
|      | ------> ' | Mysql |   <-----------------------------+   '
+------+         ' +-------+                                     '
                 '                                               '
                 +- - - - - - - - - - - - - - - - - - - - - - - -+
```

## Feature
1. Multiple PHP version can be one-time deployment.
2. All Configuration/Log files are stored in the host.
3. MySQL Data are stored in the host.

## Requirement

- Git
- Docker ([Install](https://docs.docker.com/engine/installation/))
- Docker-Compose([Install](https://docs.docker.com/compose/install/))

## Deploy

1. Clone Project:
```bash
$ git clone https://github.com/copriwolf/docker-lnmp-with-mutli-php-versions.git
```

2. Go into the project & Start docker compose:
```bash
$ cd docker-lnmp-with-mutli-php-versions
$ docker-compose up
```

3. Check the URL `localhost` in you browser, and you will catch the phpinfo with 7.2:

> the php7.2 site files is in `./site/site1`

![](./src/docker-compose-lnmp-multi-php-version-screenshot.png)

## Preview other php version website?
1. You can add these `hosts` in your system.
> hosts file location: Linux & Mac in `/etc/hosts`, Windows in `C:\Windows\System32\drivers\etc` (default)

```bash
127.0.0.1 www.site1.com
127.0.0.1 www.site2.com
127.0.0.1 www.site3.com
```

2. Then go to your browser and type `www.site2.com` or `www.site3.com`, you will catch the php5.6 & php5.3.

## Create a php53/php56/php72 website?
In fact I am using the Nginx conf file to control the version of PHP.
You can check the `.conf/nginx/conf.d/site1.conf`, and found I fill the `fpm72:9000` as the fastcgi_pass.
So you can use `fpm53:9000`/`fpm56:9000`/`fpm72:9000` to create a php53/php56/php72 website if you want.

Here is a example for creating a php53 site:

1. Copy a conf file from site3.conf
> If you want to create a `php56`/`php72` site? Refer to `site2.conf`/`site1.conf`

```bash
$ cp ./conf/nginx/conf.d/site3.conf ./conf/nginx/conf.d/youDomainName.conf
```

2. Edit the field `server_name`(Your WebSite Domain) & `root`(Your WebSite Root Directory) in conf file:
![](./src/docker-compose-lnmp-multi-php-version-2.png)

3. Create the Site Web Root Directory
```bash
$ mkdir ./site/youDomainName
```

4. Put the web files into the Directory
5. Restart the docker
```bash
$ docker-compose restart
```

## Acknowledgement
- [yeszao/dnmp](https://github.com/yeszao/dnmp)
- [micooz/docker-lnmp](https://github.com/micooz/docker-lnmp)
