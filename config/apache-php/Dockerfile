#
# Apache PHP7.4 Dockerfile in CentOS 7 Core image
#
# Start & build:
# docker-compose up -d --build
#
# Stop & clean:
# docker-compose stop && docker-compose rm -f && docker-compose down -v
#

FROM centos:7

LABEL Daniel Rodríguez <dirs21@gmail.com>

RUN yum -y update
RUN yum -y install wget httpd httpd-tools curl unzip

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
 && rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN yum --enablerepo=remi-php74 -y install php php-bcmath php-cli php-common php-gd php-intl php-ldap php-mbstring \
    php-mysqlnd php-pear php-soap php-xml php-xmlrpc php-zip php-apc php-opcache php-redis php-curl && yum clean all

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/httpd/conf/httpd.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/httpd/conf/httpd.conf
RUN sed -E -i -e 's/DirectoryIndex (.*)$/DirectoryIndex index.php \1/g' /etc/httpd/conf/httpd.conf

RUN chmod -R 777 /var/lib/php/session

RUN usermod -u 1000 apache
# RUN usermod -G 1000 apache

RUN chown -R apache:apache /var/www/html
RUN find /var/www/html -type d -exec chmod 775 {} \;

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

EXPOSE 80

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
