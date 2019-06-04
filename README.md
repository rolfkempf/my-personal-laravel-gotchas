# My Personal Laravel Gotchas

## Telescope

### phpunit

I had to disable telescope in order to run phpunit tests without errors.

Add this to your ```/phpunit.xml```: ```<env name="TELESCOPE_ENABLED" value="false"/>``` inside of the php-node.

``` xml
<php>
    <server name="APP_ENV" value="testing"/>
    <server name="BCRYPT_ROUNDS" value="4"/>
    <server name="CACHE_DRIVER" value="array"/>
    <server name="MAIL_DRIVER" value="array"/>
    <server name="QUEUE_CONNECTION" value="sync"/>
    <server name="SESSION_DRIVER" value="array"/>
    <env name="TELESCOPE_ENABLED" value="false"/>
</php>
```
If this is still not working try the following appraoch: https://dyrynda.com.au/blog/using-laravel-telescope-in-specific-environments

### Performance

Add some config values to your .env file to avoid performance problems.


```
TELESCOPE_ENABLED=true
TELESCOPE_CACHE_WATCHER=false
TELESCOPE_COMMAND_WATCHER=false
TELESCOPE_DUMP_WATCHER=false
TELESCOPE_EVENT_WATCHER=false
TELESCOPE_EXCEPTION_WATCHER=false
TELESCOPE_JOB_WATCHER=false
TELESCOPE_MAIL_WATCHER=false
TELESCOPE_MODEL_WATCHER=false
TELESCOPE_NOTIFICATION_WATCHER=false
TELESCOPE_REDIS_WATCHER=false
TELESCOPE_GATE_WATCHER=false
TELESCOPE_SCHEDULE_WATCHER=false
```


### Use SQLite

If you want to use SQLite as database for telescope add this database-connection to ```/config/database.php```

``` php
'telescope' => [
    'driver' => 'sqlite',
    'database' => database_path('telescope.sqlite'),
    'prefix' => '',
],
```

To use this connection add this to ```/config/telescope.php```

``` php
'storage' => [
    'database' => [
        'connection' => 'telescope',
    ],
],
```


## Development

### Avoid Delete Cache Shotgun Method

Don´t cache your config in the local environment. This usually happens when devs clear caches with a shotgun method: google "laravel clear caches", visit https://tecadmin.net/clear-cache-laravel-5/ (for example) and execute all artisan commands along with ```composer dump-autoload```

The culprit here is ```php artisan config:cache``` which does as it says... cache the config. Use ```php artisan config:clear``` instead.

 ### How to clear caches

remove the following files:

- bootstrap/cache/routes.php
- bootstrap/cache/config.php
- run ```php artisan cache:clear && php artisan view:clear```

### Run Composer when switching branches

When working in a team with multiple git-branches it can help to run ```composer install``` to get all new dependencies. 

```composer dump-autoload``` regenerates the compiled classlist.
