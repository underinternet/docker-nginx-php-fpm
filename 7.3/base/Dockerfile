FROM ubuntu:20.04

MAINTAINER Roberto Berto

ENV LC_ALL=C.UTF-8

RUN apt-get update && \
    apt-get -y --no-install-recommends --no-install-suggests install software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    add-apt-repository ppa:ondrej/pkg-gearman && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get remove php7.0 && \
    apt-get install -y --no-install-recommends --no-install-suggests nginx php7.3 php7.3-fpm php7.3-cli php7.3-common ca-certificates gettext \
    php7.3-mongodb php7.3-curl php7.3-intl php7.3-soap php7.3-xml php7.3-mcrypt php7.3-bcmath \
    php7.3-mysql php7.3-amqp php7.3-gearman php7.3-mbstring php7.3-ldap php7.3-zip php7.3-gd php7.3-xdebug php7.3-imagick && \    
    rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stderr /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log \
	&& ln -sf /dev/stderr /var/log/php7.3-fpm.log \
	&& ln -sf /dev/stderr /var/log/php-fpm.log

RUN rm -f /etc/nginx/sites-enabled/*

COPY nginx.conf.tpl /nginx.conf.tpl
COPY nginx_ssl.conf.tpl /nginx_ssl.conf.tpl
COPY php-fpm.conf.tpl /php-fpm.conf.tpl
COPY defaults.ini /etc/php/7.3/cli/conf.d/defaults.ini
COPY defaults.ini /etc/php/7.3/fpm/conf.d/defaults.ini

RUN mkdir -p /run/php && touch /run/php/php7.3-fpm.sock && touch /run/php/php7.3-fpm.pid

COPY entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

EXPOSE 80

CMD ["/entrypoint.sh"]
