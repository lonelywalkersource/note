## 编译安装PHP 步骤

#### 1. 安装必要的依赖
```shell
    sudo yum install -y epel-release
    sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
    sudo yum-config-manager --enable remi-php80
    sudo yum install -y gcc \
    gcc-c++ \
    make \
    autoconf \
    libtool \
    re2c \
    bison \
    libxml2-devel \
    openssl-devel \
    libcurl-devel \
    libjpeg-turbo-devel \
    libpng-devel \
    libXpm-devel \
    freetype-devel \
    gmp-devel \
    libicu-devel \
    libxslt-devel \
    libzip-devel \
    sqlite-devel \
    oniguruma-devel \
    postgresql-devel \
    libssh2-devel \
    liburing-devel \
    libzstd-devel \
    c-ares-devel \
    php-devel
```

#### 2. 下载源码并解压
```shell
    cd /usr/local/src/
    sudo wget https://www.php.net/distributions/php-8.4.4.tar.gz
    tar -zxvf php-8.4.4.tar.gz
    cd php-8.4.4
```

#### 3.  配置 PHP
```shell
    ./configure \
    --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php/etc \
    --enable-fpm \
    --with-fpm-user=www \
    --with-fpm-group=www \
    --enable-mbstring \
    --with-zip \
    --enable-bcmath \
    --enable-pcntl \
    --enable-sockets \
    --enable-gd \
    --with-jpeg \
    --with-freetype \
    --with-zlib \
    --with-openssl \
    --with-curl \
    --with-pdo-mysql \
    --with-pdo-pgsql \
    --with-pgsql \
    --with-gmp \
    --with-xsl \
    --enable-soap \
    --enable-intl \
    --with-pear \
    --enable-opcache \
    --with-libxml
```

#### 4.编译安装
```shell
    make -j$(nproc)
    sudo make install
```

#### 5. 复制 PHP.ini 到指定目录
```shell
    sudo cp php.ini-rpoduction /usr/local/php/etc/php.ini
```

#### 6. 创建软链接
```shell
    sudo ln -s /usr/local/php/bin/php /usr/local/bin/php
```

#### 7. 复制 php-fpm.conf 到指定目录
```shell
    sudo cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
    sudo cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```

#### 8. 配置 service 文件
```shell
    sudo tee /etc/systemd/system/php-fpm.service <<-'EOF'
[Unit]
Description=The PHP FastCGI Process Manager
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/php/sbin/php-fpm --nodaemonize --fpm-config /usr/local/php/etc/php-fpm.conf
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
EOF
```

#### 9. 修改 service 文件权限并重载 systemd 配置
```shell
    sudo chmod 644 /etc/systemd/system/php-fpm.service
    sudo systemctl daemon-reload
```

#### 10. 启动 PHP-FPM 服务 并设置开机启动
```shell
    sudo systemctl start php-fpm
    sudo systemctl enable php-fpm
    sudo systemctl status php-fpm
```
