FROM amazonlinux:with-sources

ARG DOMAIN
ARG BASE

EXPOSE 80 443 22 567

RUN amazon-linux-extras install -y epel
RUN yum install -y yum-utils http://rpms.remirepo.net/enterprise/remi-release-7.rpm && yum-config-manager --enable remi-php73
RUN yum update -y
RUN yum install -y --enablerepo=remi-php73 --disableplugin=priorities httpd \
                        mod_ssl \
                        mod_security \
                        mod_http2 \
                        make \
                        gcc \
                        gcc-c++ \
                        git \
                        wget \
                        python2-certbot-apache \
                        php \
                        php-common \
                        php-bcmath \
                        php-devel \
                        php-gd \
                        php-intl \
                        php-mbstring \
                        php-mcrypt \
                        php-pdo \
                        php-mysql \
                        php-opcache \
                        php-poxi \
                        php-soap \
                        php-xml \
                        php-zip \
                        php-memcached
RUN rm -rf /var/cache/yum
RUN usermod -a -G apache $(whoami)
RUN mkdir -p /var/www /var/www/hosts /var/www/hosts/${DOMAIN} /var/www/hosts/${DOMAINd}/web /var/www/hosts/${DOMAIN}/logs /var/www/hosts/${DOMAIN}/.ssh
RUN chmod 2775 /var/www && find /var/www -type d -exec chmod 2775 {} \;
RUN find /var/www -type f -exec chmod 0664 {} \;
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/localhost.key -out /etc/pki/tls/certs/localhost.crt -subj '/CN=localhost'

COPY ./httpd/conf.modules.d /etc/httpd/conf.modules.d
COPY ./httpd/conf.d /etc/httpd/conf.d
COPY ./httpd/conf/httpd.conf /etc/httpd/conf/
COPY ./vhosts.conf /etc/httpd/conf.d/

RUN systemctl enable httpd
CMD httpd -DFOREGROUND