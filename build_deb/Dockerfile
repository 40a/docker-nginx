FROM debian:jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
RUN echo "deb-src http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list

WORKDIR /tmp


ENV NGINX_BASE_VERSION=1.11.6 \
    NGINX_VERSION=1.11.6-1~jessie \
    NPS_VERSION=1.11.33.4

RUN apt-get update && \
    apt-get install -y quilt wget libgd2-xpm-dev git libgeoip-dev libperl-dev libxslt1-dev lsb-release ca-certificates build-essential zlib1g-dev libpcre3 libpcre3-dev unzip debhelper libssl-dev libldap-dev && \
    apt-get source nginx=${NGINX_VERSION}


RUN wget https://github.com/pagespeed/ngx_pagespeed/archive/release-${NPS_VERSION}-beta.zip && \
    wget https://dl.google.com/dl/page-speed/psol/${NPS_VERSION}.tar.gz && \
    git clone https://github.com/kvspb/nginx-auth-ldap.git && \
    unzip release-${NPS_VERSION}-beta.zip && \
    tar -xzvf ${NPS_VERSION}.tar.gz && \
    cp -r psol ngx_pagespeed-release-${NPS_VERSION}-beta/


RUN sed -i 's/--with-ipv6/--with-ipv6 --add-module=\/tmp\/ngx_pagespeed-release-${NPS_VERSION}-beta --add-module=\/tmp\/nginx-auth-ldap/g' /tmp/nginx-${NGINX_BASE_VERSION}/debian/rules && \
    cd /tmp/nginx-${NGINX_BASE_VERSION} && dpkg-buildpackage -uc -b

VOLUME ["/packages"]
CMD cp /tmp/*.deb /packages


