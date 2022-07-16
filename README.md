# List repository dapat dilihat pada table berikut:
| Repository | Url |
| ------ | ------ |
| laman cms backend | [cms-article-backend](https://github.com/ivalrivall/cms-article-backend) |
| laman cms ui | [cms-article-ui](https://github.com/ivalrivall/cms-article-ui) |
| laman portal | [laman-portal](https://github.com/ivalrivall/laman-portal) |
| jawaban written test | [google-docs](https://docs.google.com/document/d/1GN5DCdxWsCNqHJZ8pAgGt2pwDA9ig5NFKSGijVvZG2A/edit?usp=sharing) |

## requirement
- nodejs (v14.17.3)
- npm (v6.4.13)
- php (v7.4.24)
- composer (v2.0.11)
- postgresql/mysql database

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
npm install
npm run dev
```
- buka localhost:3000 di browser

## petunjuk instalasi laman portal ui
- jalankan command ini pada terminal
```bash
npm install
npm run serve
```
- buka localhost:8080 di browser