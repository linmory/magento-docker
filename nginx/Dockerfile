FROM nginx:1.10-alpine

ARG PHP_UPSTREAM=php-fpm

ADD nginx.conf /etc/nginx/
ADD magento.conf /etc/nginx/conf.d/magento.conf

ARG PUID=1000
ARG PGID=1000
ARG MAGENTO_ROOT=/var/www/magento
ENV MAGENTO_ROOT ${MAGENTO_ROOT}

ARG DEBUG=false
ENV DEBUG ${DEBUG}

ARG MAGENTO_RUN_MODE=default
ENV MAGENTO_RUN_MODE ${MAGENTO_RUN_MODE}

ARG VIRTUAL_HOST=localhost
ENV VIRTUAL_HOST ${VIRTUAL_HOST}

ARG PHP_MEMORY_LIMIT=768M
ENV PHP_MEMORY_LIMIT ${PHP_MEMORY_LIMIT}

RUN set -x \
  && addgroup -g $PGID -S www-data \
  && adduser -u $PGID -D -S -G www-data www-data

RUN rm /etc/nginx/conf.d/default.conf \
    && echo "upstream fastcgi_backend { server ${PHP_UPSTREAM}:9000; }" > /etc/nginx/conf.d/upstream.conf

COPY bin/* /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/entrypoint"]

CMD ["nginx"]

EXPOSE 80 443
