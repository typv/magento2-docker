# Magento2 Docker

# Getting Started

## Tech stacks

- PHP >= 8.3
- MariaDB >= 11.4
- Redis >= 8.1
- OpenSearch

## Installing preparation

1. Default Application $BASEPATH : `/home/app.user/magento2-docker`

2. Copy the .env file from .env.example under $BASEPATH, fill your config in .env file instead of example config

# Build the app

## Start containers

```bash
  bin/start
```

## Dependencies Installation

```bash
  bin/composer install
  bin/composer dump-autoload
```

## Magento Configuration

### Init Config

```bash
    bin/magento setup:install \
    --base-url=http://localhost:8111/ \
    --db-host=db \
    --db-name=magento \
    --db-user=magento \
    --db-password=magento \
    --search-engine=opensearch \
    --opensearch-host=opensearch \
    --opensearch-port=9200 \
    --opensearch-index-prefix=magento \
    --backend-frontname=admin \
    --admin-firstname=Admin \
    --admin-lastname=Admin \
    --admin-email=admin@example.com \
    --admin-user=admin \
    --admin-password=Admin123! \
    --language=en_US \
    --currency=USD \
    --timezone=Asia/Ho_Chi_Minh \
    --use-rewrites=1
    "
```

### Add Configurations for Each Method

> ⚠️ **Note:**
> Each configuration method below corresponds to a specific APP_ENV setting. Choosing the right method ensures system stability and security across different environments.

#### Local

```bash
    bin/magento config:set web/secure/base_url http://localhost:8111/
    bin/magento config:set web/unsecure/base_url http://localhost:8111/
```

#### Dev (Reverse Proxy)

```bash
    bin/magento config:set web/unsecure/base_url https://your-domain/
    bin/magento config:set web/unsecure/base_link_url https://your-domain/
    bin/magento config:set web/secure/base_url https://your-domain/
    bin/magento config:set web/secure/base_link_url https://your-domain/
    bin/magento config:set web/secure/use_in_frontend 0
    bin/magento config:set web/secure/use_in_adminhtml 1
    bin/magento config:set web/secure/offloader_header X-Forwarded-Proto
    bin/magento config:set web/url/redirect_to_base 0
```

### Clear cache

```bash
    bin/magento cache:flush
```

### Restart App

```bash
    bin/restart
```

### Access Urls:

#### Local

- Client: http://localhost:8111
- Admin: http://localhost:8111/admin (admin / Admin123!)

#### Dev Env (Reverse Proxy)

- Client: https://your-domain
- Admin: https://your-domain/admin (admin / Admin123!)

# Other

## Custom CLI Commands

```bash
- bin/bash: Drop into the bash prompt of your Docker container. The phpfpm container should be mainly used to access the filesystem within Docker.
- bin/cache-clean: Access the cache-clean CLI. Note the watcher is automatically started at startup in bin/start. Ex. bin/cache-clean config full_page
- bin/cli: Run any CLI command without going into the bash prompt. Ex. bin/cli ls
- bin/clinotty: Run any CLI command with no TTY. Ex. bin/clinotty chmod u+x bin/magento
- bin/cliq: The same as bin/cli, but pipes all output to /dev/null. Useful for a quiet CLI, or implementing long-running processes.
- bin/composer: Run the composer binary. Ex. bin/composer install
- bin/copyfromcontainer: Copy folders or files from container to host. Ex. bin/copyfromcontainer vendor
- bin/copytocontainer: Copy folders or files from host to container. Ex. bin/copytocontainer --all
- bin/dev-urn-catalog-generate: Generate URNs for PHPStorm and remap paths to local host. Restart PHPStorm after running this command.
- bin/devconsole: Alias for bin/n98-magerun2 dev:console
- bin/devtools-cli-check: Check & install the CLI devtools if missing from system.
- bin/download: Download specific Magento version from Composer to /var/www/html directory within the container. Ex. bin/download 2.4.2 community
- bin/fixowns: This will fix filesystem ownerships within the container.
- bin/fixperms: This will fix filesystem permissions within the container.
- bin/grunt: Run the grunt binary. Ex. bin/grunt exec
- bin/magento: Run the Magento CLI. Ex: bin/magento cache:flush
- bin/mysql: Run the MySQL CLI with database config from env/db.env. Ex. bin/mysql -e "EXPLAIN core_config_data" orbin/mysql < backups/magento.sql
- bin/mysqldump: Backup the Magento database. Ex. bin/mysqldump > backups/magento.sql
- bin/n98-magerun2: Access the n98-magerun2 CLI. Ex: bin/n98-magerun2 dev:console
- bin/node: Run the node binary. Ex. bin/node --version
- bin/npm: Run the npm binary. Ex. bin/npm install
- bin/pwa-studio: (BETA) Start the PWA Studio server. Note that Chrome will throw SSL cert errors and not allow you to view the site, but Firefox will.
- bin/redis: Run a command from the redis container. Ex. bin/redis redis-cli monitor
- bin/remove: Remove all containers.
- bin/removeall: Remove all containers, networks, volumes, and images.
- bin/removevolumes: Remove all volumes.
- bin/restart: Stop and then start all containers.
- bin/root: Run any CLI command as root without going into the bash prompt. Ex bin/root apt-get install nano
- bin/rootnotty: Run any CLI command as root with no TTY. Ex bin/rootnotty chown -R app:app /var/www/html
- bin/setup: Run the Magento setup process to install Magento from the source code, with optional domain name. Defaults to magento.test. Ex. bin/setup magento.test
- bin/setup-composer-auth: Setup authentication credentials for Composer.
- bin/setup-grunt: Install and configure Grunt JavaScript task runner to compile .less files
- bin/setup-pwa-studio: (BETA) Install PWA Studio (requires NodeJS and Yarn to be installed on the host machine). Pass in your base site domain, otherwise the default master-7rqtwti-mfwmkrjfqvbjk.us-4.magentosite.cloud will be used. Ex: bin/setup-pwa-studio magento.test
- bin/setup-ssl: Generate an SSL certificate for one or more domains. Ex. bin/setup-ssl magento.test foo.test
- bin/setup-ssl-ca: Generate a certificate authority and copy it to the host.
- bin/start: Start all containers, good practice to use this instead of docker-compose up -d, as it may contain additional helpers.
- bin/status: Check the container status.
- bin/stop: Stop all containers.
- bin/update: Update your project to the most recent version of docker-magento.
- bin/xdebug: Disable or enable Xdebug. Accepts params disable (default) or enable. Ex. bin/xdebug enable
```

## Reference Document

- https://hub.docker.com/r/markoshust/magento-php