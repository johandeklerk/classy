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

Create a test user:

```bash
docker exec classy-php-fpm php artisan make:test-user
```

Test oauth:

replace [client_id] and [client_secret] with relevant values

```bash
curl -d "grant_type=password&client_id=[client_id]&client_secret=[client_secret]&username=test@test.com&password=secret&scope=*" -X POST http://classy.local/oauth/token
```

It should return a Bearer token, copy the access_token value

You can also use Postman for a pretty UI. Just do a normal form-data POST request.

## API

Using the bearer token you can now make API requests to authenticated routes.

For non-auth

Go to http://classy.local
