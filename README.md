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

```bash
docker-compose build
docker-compose up
```

Set up the DB:

```bash
docker exec -it classy-mysql mysql -u root -p
```

When prompted, enter the password: secret
then:

```bash
create database classy;
exit;
```

Run Migrations:

```bash
docker exec classy-php-fpm php artisan migrate
```

Go to http://classy.local

