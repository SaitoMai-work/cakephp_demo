# PHPの公式イメージを使用
FROM php:7.4-apache

# 必要なパッケージのインストール
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libicu-dev \
    zip \
    unzip \
    git

# PHPの`intl`拡張モジュールをインストール
RUN docker-php-ext-install intl

# PHPのGDライブラリとPDOモジュールをインストール
RUN docker-php-ext-install gd pdo pdo_mysql

# Composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 作業ディレクトリの設定
WORKDIR /var/www/html/cakephp

# CakePHPのアプリケーションソースコードをコンテナにコピー
COPY cakephp/ /var/www/html/cakephp

# Apacheの設定を更新
RUN echo "<Directory /var/www/html/cakephp>\n\
    Options Indexes FollowSymLinks\n\
    AllowOverride All\n\
    Require all granted\n\
</Directory>" > /etc/apache2/sites-available/000-default.conf

# `mod_rewrite`の有効化
RUN a2enmod rewrite

# Apacheの権限設定
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

# ComposerでCakePHPの依存関係をインストール
RUN composer install --no-dev --optimize-autoloader
