#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM linmory/magento-php-fpm:7.2

#
#--------------------------------------------------------------------------
# Optional Software's Installation
#--------------------------------------------------------------------------
#
# Optional Software's will only be installed if you set them to `true`
# in the `docker-compose.yml` before the build.
#
#   - INSTALL_XDEBUG=            false
#

###########################################################################
# xDebug:
###########################################################################

ARG INSTALL_XDEBUG=false

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Install the xdebug extention
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
;fi

# Copy xdebug configuration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN sed -i "s/xdebug.remote_autostart=0/xdebug.remote_autostart=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.remote_enable=0/xdebug.remote_enable=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.cli_color=0/xdebug.cli_color=1/" /usr/local/etc/php/conf.d/xdebug.ini


#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

ADD ./magento.ini /usr/local/etc/php/conf.d
ADD ./magento.pool.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

ARG MAGENTO_ROOT=/var/www/magento
WORKDIR ${MAGENTO_ROOT}

COPY bin/* /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/entrypoint"]

CMD ["php-fpm"]

EXPOSE 9000
