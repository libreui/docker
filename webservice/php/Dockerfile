FROM php:7.3-fpm
 
# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
 
# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        --no-install-recommends libfreetype6-dev libjpeg62-turbo-dev libpng-dev curl libzip-dev \
        && rm -r /var/lib/apt/lists/* \
        && docker-php-ext-configure gd \
        && docker-php-ext-install -j$(nproc) gd bcmath opcache pdo_mysql gettext sockets zip mysqli dba pcntl calendar shmop exif sysvmsg sysvsem sysvshm 
 
# 安装 PECL 拓展，安装Redis，mongodb，amqp
# RUN pecl install redis \
#     && docker-php-ext-enable redis
	
# RUN pecl install mongodb \
#    && docker-php-ext-enable mongodb
 
# 若在此处安装amqp失败（因为linux系统版本问题查不到包会报错：E: Unable to locate package librabbitmq-dev），可先将下面注释。
# 等安装好后 进入容器执行：
# 1.执行：apt-get update 
# 2.再搜索一下： apt-cache search librabbitmq-dev
# 3.如果能查询到再执行 apt-get install librabbitmq-dev -y
# 4.同理，pecl查询下版本：pecl search amqp 
# 5.执行：pecl install amqp 1.11.0 -y
# 6.执行：docker-php-ext-enable amqp
 
# RUN apt-get install librabbitmq-dev -y \
# 	&& pecl install amqp 1.11.0 -y \
# 	&& docker-php-ext-enable amqp
 
# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH
 
WORKDIR /data

