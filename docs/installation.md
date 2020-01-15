# Installation

Azuriom can be installed in two different ways:

- Automatically through the installer _(recommended for most users)_. 
- Manually via [Composer](https://getcomposer.org/) _(recommended for experienced users or those wishing to contribute to Azuriom)_.

## Automatic Installation

1. Download the latest version of Azuriom on [our website](https://azuriom.com/download)

2. Extract the archive at the root of your website

3. Go to `your-website.com/install.php` and follow the steps of installation

## Manual Installation

1. Clone the GitHub repository (https://github.com/Azuriom/Azuriom.git) or [download a release](https://github.com/Azuriom/Azuriom/release).

2. Copy the `.env.example` file to `.env` and specify the database connection information.

3. Set write/read permissions to the `storage/` and `bootstrap/cache/`:
```
chmod -R 770 storage bootstrap/cache
```

4. Generate the secret key:
```
php artisan key:generate
```

5. Install the dependencies with Composer:
```
composer install
```

  *If you are using Azuriom in production, you can optimize the autoloader of Composer using this command:
```
composer install --optimize-autoloader --no-dev
```

6. Setup the database:
```
php artisan migrate --seed
```

7. Configure your web server to point to the `public/` folder _(Optional but highly recommended)_

8. Setup the scheduler _(Optional but recommended)_:

You need to configure your web server to run the `php artisan schedule:run` command every minute, for example by adding a Cron entry:
 ```
* * * * * cd /path-to-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```