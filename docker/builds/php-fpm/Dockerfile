FROM php:7.1-fpm

ENV user dev
ARG PUID=1000
ARG PGID=1000
RUN groupadd -g $PGID $user && \
    useradd -u $PUID -g $user -m $user

ADD php.ini /usr/local/etc/php/conf.d/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && apt-get install zip unzip -y

# gd
RUN buildRequirements="libpng12-dev libjpeg-dev libgd-dev libfreetype6-dev" \
	&& apt-get update && apt-get install -y ${buildRequirements} \
	&& docker-php-ext-configure gd --with-jpeg-dir=/usr/lib/ --with-png-dir=/usr/lib/ --with-freetype-dir=/usr/lib/ \
	&& docker-php-ext-install gd \
	&& apt-get purge -y ${buildRequirements} \
	&& rm -rf /var/lib/apt/lists/*

# zip
RUN apt-get update \
    && apt-get install -y zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip

RUN docker-php-ext-install pdo pdo_mysql

# mcrypt
RUN apt-get update \
	&& apt-get install -y libmcrypt-dev \
	&& docker-php-ext-install mcrypt

# openssl
RUN apt-get update \
	&& apt-get install -y openssl

# xml
RUN apt-get update \
	&& apt-get install -y \
	libxml2-dev \
	&& docker-php-ext-install \
		dom \
		xmlrpc

# strings
RUN docker-php-ext-install \
	gettext \
	mbstring

RUN yes | pecl install xdebug
ADD xdebug.ini /usr/local/etc/php/conf.d/

RUN curl -sL https://deb.nodesource.com/setup_9.x | bash -
RUN apt-get install nodejs build-essential -y

RUN apt-get update && apt-get install git g++ -y

# Clean
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/*

# Install phpunit globally
RUN curl -O -L https://phar.phpunit.de/phpunit.phar
RUN chmod +x phpunit.phar
RUN mv phpunit.phar /usr/local/bin/phpunit

USER $user
