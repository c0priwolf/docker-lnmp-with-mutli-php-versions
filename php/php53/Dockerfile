FROM centos:6
RUN yum -y update && yum clean all
RUN yum -y install php-fpm php-mbstring php-mysql php-gd && yum clean all
RUN sed -e 's/127.0.0.1:9000/9000/' \
        -e '/allowed_clients/d' \
        -e '/catch_workers_output/s/^;//' \
        -e '/error_log/d' \
        -i /etc/php-fpm.d/www.conf
RUN mkdir -p /var/www/html
EXPOSE 9000
ENTRYPOINT /usr/sbin/php-fpm --nodaemonize
