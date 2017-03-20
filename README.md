# modsecurity-heroku-php

Builds Apache with [ModSecurity](https://modsecurity.org/) for Heroku apps that use [Heroku buildpack: PHP](https://github.com/heroku/heroku-buildpack-php).

## Usage

To replace Apache with a build made with this tool, add its repository to your Heroku app's config:

```sh
$ heroku config:set HEROKU_PHP_PLATFORM_REPOSITORIES="https://modsecurity-heroku-php.s3.amazonaws.com/dist-cedar-14-develop/packages.json"
```

To build the Apache on your own, follow [the buildpack's instructions](https://github.com/heroku/heroku-buildpack-php/blob/master/support/build/README.md) for how to use the build formula provided by this tool.

### Logs

In order to feed ModSecurity's debug (audit likewise) logs into [Heroku's log stream](https://devcenter.heroku.com/articles/logging), you can use the `-l` option in your `Procfile`.

Procfile example:

```
web: vendor/bin/heroku-php-apache2 -C vhost.conf -l /tmp/heroku.modsecurity_debug.${PORT}.log
```

vhost.conf example:

```apache
DirectoryIndex index.php index.html index.htm

SecDebugLog /tmp/heroku.modsecurity_debug.${PORT}.log
SecDebugLogLevel 9
```
