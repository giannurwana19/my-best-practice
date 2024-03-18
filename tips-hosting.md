# TIPS HOSTING PHP DI CPANEL

### 1. Project PHP Native

- cukup pindahkan semua folder ke file manager cpanel
- sesuaikan setting database (username, password, nama database)

### 2. Project Laravel

Jika tidak menggunakan git dan ssh client, lakukan langkah-langkah berikut:

- pindahkan semua folder ke file manager cpanel
- copy file .htaccess yang di folder /htaccess-laravel yang sudah disediakan di folder ini
- pada file .env ganti `APP_URL` dengan url domain yang benar
- tambahkan di baris terakhir di file .env `ASSET_URL=/public` jika assets tidak terload


#### membuat symbolic link

1. Akses melalui terminal, ketikkan `php artisan storage:link`

1. dengan cara buat route untuk mengakses file ini

```php
$target  = '/home/gian19/project-laravel/storage/app/public';
$link    = '/home/gian19/public_html/storage';

// atau

$target = $_SERVER['DOCUMENT_ROOT']."/../laravel/storage";
$link = $_SERVER['DOCUMENT_ROOT']."/storage";
symlink($target, $link);
```

- ket: path diatas adalah contoh

2. akses melalui terminal (manual path) 

- Masuk ke folder laravel kamu Sob dengan ketik command : cd <nama_project>
- Kemudian masuk ke folder public dengan ketik command : cd public
- Buat symlink antara kedua folder tersebut dengan ketik command ln -s source destination

```
ln -s /home/jual/kalkun/laravel/storage/app/public storage
```

Atau bisa juga seperti ini:

Contoh: atau kalo misalkan Anda ingin membuat link dari root folder folder:

laravel/storage/app/public ke public_html/storage gunakan syntax berikut:

```
ln -s /home/usernamecpanel/laravel/storage/app/public /home/usernamecpanel/public_html/storage
```

\*ket: sesuaikan pathnya dengan url file manager dari cpanel

#### Mengatasi versi composer yang tidak support

```bash
composer update --ignore-platform-reqs
```

#### file .htaccess laravel
```htaccess title=".htaccess"
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews
    </IfModule>

    RewriteEngine On

    RewriteCond %{REQUEST_FILENAME} -d [OR]
    RewriteCond %{REQUEST_FILENAME} -f
    RewriteRule ^ ^$1 [N]

    RewriteCond %{REQUEST_URL} (\.\w+$) [NC]
    RewriteRule ^(.*)$ public/$1

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ server.php
</IfModule>

```

### 3. Project Codeigniter 3

- pindahkan semua folder ke file manager cpanel
- copy file .htaccess yang di folder /htaccess-codeigniter3 yang sudah disediakan di folder ini
- setting base_url di config.php menjadi alamat domain website

#### file htaccess codeigniter 3

```htaccess title=".htaccess"
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule .* index.php/$0 [PT,L]
```


### 4. Project Codeigniter 4

- pindahkan semua folder ke file manager cpanel
- copy file .htaccess yang di folder /htaccess-codeigniter4 yang sudah disediakan di folder ini
- setting base_url di config.php menjadi alamat domain website

#### file htaccess codeigniter 4

```htaccess title=".htaccess"
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteRule ^(.*)$ public/ [L]
</IfModule>

<FilesMatch "^\.">
  Require all denied
  Satisfy All
</FilesMatch>
```