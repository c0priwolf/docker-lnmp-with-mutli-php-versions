# docker-lnmp-with-mutli-php-versions

Create the LNMP with Docker (Use the php5.3, 5.6, 7.2 to deployment is available)

> Because of my work needs, I need to maintain somw old system which use php5.3, so I need a more rapid deployment system tools.
> And that's what I going to share with you.


```

                 +- - - - - - - - - - - - - - - - - - - - - - - -+
                 ' host:                                         '
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

the php7.2 site files is in `./site/site1`

![](./src/SCREENSHOT.png)

4. If you want to preview other php version info in the site. You can add the `hosts` in your system.
> hosts file location: Linux & Mac in `/etc/hosts`, Windows in `C:\Windows\System32\drivers\etc` (default)

```bash
127.0.0.1 www.site1.com
127.0.0.1 www.site2.com
127.0.0.1 www.site3.com
```

5. Then go to your browser and type `www.site2.com` or `www.site3.com`, you will catch the php5.6 & php5.3.

## Create a php53 website?

1. Copy a conf file from site3.conf
> why site3.conf? Because site3.conf use the `fpm53:9000` as fastcgi_pass.
> If you want to create a `php56`/`php72` site? Refer to `site2.conf`/`site1.conf`

```bash
cp ./conf/nginx/conf.d/site3.conf ./conf/nginx/conf.d/youDomainName.conf
```

2. Create the Site Web Root Directory

```bash
mkdir ./site/youDomainName
```

3. Put the web files into the Directory
4. Restart the docker

```bash
docker-compose restart
```

## Acknowledgement
- [yeszao/dnmp](https://github.com/yeszao/dnmp)
- [micooz/docker-lnmp](https://github.com/micooz/docker-lnmp)
