ARG PHP_VERSION
FROM davidalger/php:${PHP_VERSION}

RUN set -eux \
    && PHP_VERSION=$(php -v | head -n1 | cut -d' ' -f2 | awk -F '.' '{print $1$2}') \
    && if (( ${PHP_VERSION} >= 73 )); \
        then yum install -y php${PHP_VERSION}-fpm; \
        else yum install -y php${PHP_VERSION}u-fpm; \
    fi \
    && yum clean all \
    && rm -rf /var/cache/yum

RUN set -eux; \
	cd /etc; \
	{ \
		echo '[global]'; \
		echo 'error_log = /proc/self/fd/2'; \
		echo; \
		echo '[www]'; \
		echo '; if we send this to /proc/self/fd/1, it never appears'; \
		echo 'access.log = /proc/self/fd/2'; \
		echo; \
		echo 'clear_env = no'; \
		echo; \
		echo '; Ensure worker stdout and stderr are sent to the main error log.'; \
		echo 'catch_workers_output = yes'; \
	} | tee php-fpm.d/docker.conf; \
	{ \
		echo '[global]'; \
		echo 'daemonize = no'; \
		echo; \
		echo '[www]'; \
		echo 'listen = 9000'; \
	} | tee php-fpm.d/zz-docker.conf; \
    perl -pi -e 's/^(pid|error_log|daemonize)/;$1/g' /etc/php-fpm.conf; \
    perl -pi -e 's/^(listen)/;$1/g' /etc/php-fpm.d/www.conf; \
    perl -pi -e 's/^(php_admin_(value|flag))/;$1/g' /etc/php-fpm.d/www.conf

COPY docker-entrypoint /usr/local/bin
ENTRYPOINT ["docker-entrypoint"]

# Override stop signal to stop process gracefully
# https://github.com/php/php-src/blob/17baa87faddc2550def3ae7314236826bc1b1398/sapi/fpm/php-fpm.8.in#L163
STOPSIGNAL SIGQUIT

EXPOSE 9000
CMD ["php-fpm"]
