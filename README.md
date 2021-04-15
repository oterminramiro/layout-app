## Dockerized Laravel + Nuxt.JS project.

## Stack includes
* Laravel (clean 8.4.0 version)
* Nuxt.JS (clean 2.14.5 version)
* PostgreSQL 11.3
* Nginx
* Redis 3

## Prerequisites
- Docker-compose


#### installation
If you do not have available the make utility, or you just want to install the project manually, you can go through the installation process running the following commands:

**Build and up docker containers (It may take up to 10 minutes)**
```
docker-compose up -d --build
```

**Install composer dependencies:**
```
docker-compose exec php composer install
```

**Copy environment files**
```
cp .env.api api/.env
cp .env.client client/.env
```

**Set up laravel permissions**
```
sudo chmod -R 777 api/bootstrap/cache
sudo chmod -R 777 api/storage
```

**Restart the client container**
```
docker-compose restart client
```

## Basic usage
Your base url is ```http://localhost:8080```. All requests to Laravel API must be sent using to the url starting with `/api` prefix. Nginx server will proxy all requests with ```/api``` prefix to the node static server which serves the Nuxt. 

There is also available [http://localhost:8081](http://localhost:8081) url which is handled by Laravel and should be used for testing purposes only.


## Environment
To up all containers, run the command:
```
# Full command
docker-compose up -d
```

To shut down all containers, run the command:
```
# Full command
docker-compose down
```

## Nuxt
Your application is available at the [http://localhost:8080](http://localhost:8080) url.

Take a look at `client/.env` file. There are two variables:
```
API_URL=http://nginx:80
API_URL_BROWSER=http://localhost:8080
```
`API_URL` is the url where Nuxt sends requests during SSR process and is equal to the Nginx url inside the docker network. Take a look at the [image above](#basic-usage).
`API_URL_BROWSER` is the base application url for browsers.

To make them work, ensure you have the import dotenv statement at the very top in the nuxt.config.js file 
```
require('dotenv').config()

export default {    
  // Nuxt configuration
}
```


#### Dependencies
If you update or install node dependencies, you should restart the Nuxt process, which is executed automatically by the client container:
```
# Full command
docker-compose restart client
```

## Laravel
Laravel API is available at the [http://localhost:8080/api](http://localhost:8080/api) url.   

There is also available [http://localhost:8081](http://localhost:8081) url which is handled by Laravel and should be used for testing purposes only.

#### Artisan
Artisan commands can be used like this
```
docker-compose exec php php artisan migrate
```
