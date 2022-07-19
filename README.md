# List repository dapat dilihat pada table berikut:
| Repository | Url |
| ------ | ------ |
| laman cms backend | [cms-article-backend](https://github.com/ivalrivall/cms-article-backend) |
| laman cms ui | [cms-article-ui2](https://github.com/ivalrivall/cms-article-ui2) |
| laman portal | [laman-portal2](https://github.com/ivalrivall/laman-portal2) |
| jawaban written test | [google-docs](https://docs.google.com/document/d/1GN5DCdxWsCNqHJZ8pAgGt2pwDA9ig5NFKSGijVvZG2A/edit?usp=sharing) |

## requirement
- nodejs (lts / gallium)
- npm (v8.11.0)
- php (v7.4.24)
- composer (v2.3.9)
- postgresql/mysql database
- laradock untuk development

## config nginx laradock
```bash
## tambah file cmsbackend.conf lalu paste config dibawah ini:
server {

    listen 80;
    listen [::]:80;

    # For https
    # listen 443 ssl;
    # listen [::]:443 ssl ipv6only=on;
    # ssl_certificate /etc/nginx/ssl/default.crt;
    # ssl_certificate_key /etc/nginx/ssl/default.key;

    server_name cmsbackend.test;
    root /var/www/cms/cms-article-backend/public;
    index index.php index.html index.htm;

    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_pass php-upstream;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fixes timeouts
        fastcgi_read_timeout 600;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }

    error_log /var/log/nginx/laravel_error.log;
    access_log /var/log/nginx/laravel_access.log;
}
```

## setup laradock
- clone laradock pada lokasi seperti url berikut https://laradock.io/getting-started/#B
- masuk ke repo laradock yang telah di clone dan checkout ke branch v12.1
- run "docker compose up -d nginx postgresql workspace" pada terminal
- (optional #1) run "docker compose exec workspace bash"
- (optional #2) run "chmod -R 777 nama folder repo untuk "cms backend"

## ubah config host
- ubah /etc/hosts dengan nano command
- lalu tambahkan cmsbackend.test di samping 127.0.0.1. contoh:
```bash
127.0.0.1   cmdbackend.test
```

## setup mailtrap.io
- login ke mailtrap.io dan tambahkan inbox
- click judul inbox yang sudah dibuat, cari confignya pada tab SMTP Settings
- pilih laravel 7+ pada value integration dropdown
- copy setup .env yang didapat dari mailtrap.io  dan paste pada .env pada repo cms-article-backend
- berikut adalah contoh setup yang didapat dari mailtrap.io
```bash
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=xxxx
MAIL_PASSWORD=xxxxxxx
MAIL_ENCRYPTION=tls
```

## setup code untuk membuat

## petunjuk instalasi laman cms backend
- buat database sql dengan nama cms
- jalankan command ini pada terminal
```bash
cp .env.example .env
composer install
php artisan key:generate
php artisan migrate
php artisan migrate --path=/database/migrations/main
php artisan migrate --path=/database/migrations/alter
php artisan passport:install
```
- lakukan perintah berikut pada .env 
- ubah PERSONAL_CLIENT_ID dengan value "Client ID" dari "Personal access client"
- ubah PERSONAL_CLIENT_SECRET dengan value "Client secret" dari "Personal access client"
- ubah PASSWORD_CLIENT_ID dengan value "Client ID" dari "Password grant client"
- ubah PASSWORD_CLIENT_SECRET dengan value "Client secret" dari "Password grant client"
- ubah APP_ENV menjadi production
- ubah APP_DEBUG menjadi false
```bash
php artisan db:seed
php artisan storage:link
php artisan serve
```
- jalankan unit testing
```bash
php artisan test --testsuite=Feature --stop-on-failure
```

## petunjuk instalasi laman cms ui
- jalankan command ini pada terminal
```bash
cp .env.example .env
yarn install
yarn serve
```
- buka localhost:80xx di browser (port bisa saja 8081/8082/8083, dan seterusnya)

## petunjuk instalasi laman portal ui
- jalankan command ini pada terminal
```bash
yarn install
yarn dev
```
- buka localhost:30xx di browser (port bisa saja 3001/3002/3003, dan seterusnya)
