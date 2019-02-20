# Docker Configuration for Lumen Development

## Overview
Docker container with apache 2 server configured with xdebug to provide a quick start and consistent environment for Lumen
framework development. Files created for the Lumen application will by synced with the */app* folder.

## Running the Containers
Start containers with:
```
docker-compose up -d
```

Stop containers with:
```
docker-compose stop
```
  
To start and rebuild after adjusting configuration:
```
docker-compose build
```

## Create the Lumen Application
Open shell to docker container
```
docker exec -it lumen /bin/bash
```
  
Create the application
```
composer create-project --prefer-dist laravel/lumen my-app
```

## Setup the database connection params

Update the *.env* file in application root. Example:
```
APP_NAME=Lumen
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost
APP_TIMEZONE=UTC

LOG_CHANNEL=stack
LOG_SLACK_WEBHOOK_URL=

DB_CONNECTION=pgsql
DB_HOST=lumen-postgres
DB_PORT=5432
DB_DATABASE=recipe_api
DB_USERNAME=recipe_api_user
DB_PASSWORD=RecipeApiPass123

CACHE_DRIVER=file
QUEUE_CONNECTION=sync
```
  
In *<app-root>/app.php* uncomment:
```
$app->withFacades();
```

## Adjust the Configuration
Find your ip address using *ifconfig* or *ipconfig*  

Replace the ip address in *docker-php-ext-xdebug.ini* with your ip address
```
xdebug.remote_host=<your-ip-address-here>
``` 
  
Changes to the ini file require a stop/start to be reflected.
Changes to in the */app* directory sync automatically.  

#### Apache2 Configurations
Changes specific to the application for the apache2 web server can be made in the .htaccess file generated in the */public* folder.
Changes to the overall server configuration can be made in the *apache2.conf* file.

## VS Code Debug Settings

Using the PHP Debug extension:
https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug  

```
{
    "name": "Listen for XDebug",
    "type": "php",
    "request": "launch",
    "port": 9000,    
    "pathMappings": {
        "/var/www/html": "${workspaceRoot}/app"    
    }
}
```