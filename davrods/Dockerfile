FROM centos:7

RUN yum update -y && \
    yum install -y epel-release && \
    yum install -y wget curl apache2 gnupg python python-pip python-jsonschema python-requests

RUN rpm --import https://packages.irods.org/irods-signing-key.asc && \
    wget -qO - https://packages.irods.org/renci-irods.yum.repo | tee /etc/yum.repos.d/renci-irods.yum.repo

RUN wget -O /davrods.rpm https://github.com/UtrechtUniversity/davrods/releases/download/4.2.10_1.5.0/davrods-4.2.10_1.5.0-1.rpm && \
    yum install -y --nogpgcheck localinstall davrods.rpm

RUN sed -i "s/^#//g" /etc/httpd/conf.d/davrods-vhost.conf
RUN sed -i "s/ServerName.*/ServerName irods-davrods.example.org/" /etc/httpd/conf.d/davrods-vhost.conf
RUN sed -i "s/#DavrodsServer.*/DavrodsServer icat.example.org 1247/" /etc/httpd/conf.d/davrods-vhost.conf

#RUN a2enmod dav && a2enmod davrods && a2ensite davrods-vhost && a2dissite 000-default && service apache2 restart

RUN curl -fSL https://github.com/jwilder/dockerize/releases/download/v0.6.1/dockerize-linux-amd64-v0.6.1.tar.gz -o dockerize-linux-amd64-v0.6.1.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-v0.6.1.tar.gz
CMD dockerize -wait tcp://icat.example.org:1247 -timeout 250s service apache2 restart && tail -f /dev/null
