ARG PHP_VERSION
FROM davidalger/php:${PHP_VERSION}-fpm

# Install and enable Source Gaurdian loader
RUN PHP_VERSION=$(php -v | head -n1 | cut -d' ' -f2 | cut -d. -f1-2) \
    && mkdir -p /tmp/sourceguardian \
    && cd /tmp/sourceguardian \
    && curl -Os https://www.sourceguardian.com/loaders/download/loaders.linux-x86_64.tar.gz \
    && tar xzf loaders.linux-x86_64.tar.gz \
    && cp ixed.${PHP_VERSION}.lin "$(php -i | grep '^extension_dir =' | cut -d' ' -f3)/sourceguardian.so" \
    && echo "extension=sourceguardian.so" > /etc/php.d/15-sourceguardian.ini \
    && rm -rf /tmp/sourceguardian

# Install and enable IonCube loader
RUN PHP_VERSION=$(php -v | head -n1 | cut -d' ' -f2 | cut -d. -f1-2) \
    && mkdir -p /tmp/ioncube \
    && cd /tmp/ioncube \
    && curl -Os https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
    && tar xzf ioncube_loaders_lin_x86-64.tar.gz \
    && curl -Os https://www.ioncube.com/php-7.4.0-beta-loaders/ioncube_loaders_lin_x86-64_7.4_BETA2.tar.gz \
    && tar xzf ioncube_loaders_lin_x86-64.tar.gz \
    && tar xzf ioncube_loaders_lin_x86-64_7.4_BETA2.tar.gz -C ioncube \
    && cp "$(find . -regex "./ioncube/ioncube_loader_lin_${PHP_VERSION}[0-9\.\_]*\(beta[0-9]*\|\)\.so")" \
        "$(php -i | grep '^extension_dir =' | cut -d' ' -f3)/ioncube_loader.so" \
    && echo "zend_extension=ioncube_loader.so" > /etc/php.d/01-ioncube-loader.ini \
    && rm -rf /tmp/ioncube
