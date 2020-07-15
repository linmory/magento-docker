Magento Docker
====================

## Installation

Let's see how easy it is to install `NGINX`, `PHP`, `Composer`, `MySQL` and `Redis`. Then run `Magento`.

1. Get Magedock inside your Magento project: 

```shell script
git submodule add https://github.com/ojhaujjwal/magedock.git
```
2. Copy local.env.sample to local.env and modify it to your needs.
3. Enter the magedock folder and run only these Containers: 

`docker-compose up -d nginx mysql redis`

4. `docker-compose exec workspace magento-installer`
5. Open your browser and visit the localhost: `http://localhost`

```shell script
cd /var/www/html/<magento install directory>
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chown -R :www-data .
chmod u+x bin/magento
```

```shell script
bin/magento setup:install \
--base-url=http://localhost \
--db-host=mysql \
--db-name=magento \
--db-user=root \
--db-password=magento2 \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=Asia/Shanghai \
--use-rewrites=1
```

## Configure

### php.ini
```ini
memory_limit = 2G
max_execution_time = 1800
zlib.output_compression = On

opcache.save_comments=1
```

### 修改配置
```shell script
docker-compose up -d --no-deps --build nginx
```

### Configure Redis default caching
```shell script
bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=redis  --cache-backend-redis-db=0

bin/magento setup:config:set --page-cache=redis --page-cache-redis-server=redis --page-cache-redis-db=1

bin/magento setup:config:set --session-save=redis --session-save-redis-host=redis --session-save-redis-log-level=3 --session-save-redis-db=2

```
