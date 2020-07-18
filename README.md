## Local setup

Install docker and make sure docker-compose is working.

Make sure no local services are running on ports: 80, 6380 or 3309

```bash
cp .env.example .env
```

Add the following to */etc/hosts*:

```bash
127.0.0.1   classy.local
```

Build the containers:

```bash
docker-compose build
```

Start up the containers:

```bash
docker-compose up
```

Install composer packages:

```bash
docker exec classy-php-fpm composer install
```

Set up the DB:

```bash
docker exec -it classy-mysql mysql -u root -p
```

When prompted, enter the password: secret

```bash
create database classy;
exit;
```

Run Migrations:

```bash
docker exec classy-php-fpm php artisan migrate
```

If the oauth tables aren't created please run

```bash
docker exec classy-php-fpm php artisan passport:install
docker exec classy-php-fpm php artisan migrate
```

Generate oauth keys

```bash
docker exec classy-php-fpm php artisan passport:keys
```

Create password grant client

```bash
docker exec classy-php-fpm php artisan passport:client --password
```

Please note the client ID and secret



Go to http://classy.local
