# Drupal 8 base stack
This repo uses [docker4drupal](https://github.com/wodby/docker4drupal) for a customizable Docker-based stack and
[Drupal Composer Project](https://github.com/drupal-composer/drupal-project) to install and manage Drupal.

## Spinning up a site
1. Clone the repo
2. Copy `.env.example` to `.env` and adjust the variables as needed. I suggest only changing the following:
```
PROJECT_NAME // the name of your project, it's used to name the containers
PROJECT_BASE_URL // this will be the URL you'll access your site at
DRUSH_OPTIONS_URI
# The following are for the DB
DB_NAME
DB_USER
DB_PASSWORD
DB_ROOT_PASSWORD
DB_HOST
DB_DRIVER
```
All the rest deal with `docker4drupal`'s [Wodby](https://cloud.wodby.com/stacks) container stacks

3. (OPTIONAL) You may want to mount the database containers in `docker-compose.yml`
4. Now spin up the containers with either `docker-compose up -d` ( I would recommend adding `--pull` the firts time) or `make up`
5. Login to the `PHP` container and run `composer install`. This will install Drupal 8 under `web`
```bash
docker exec -ti [PROJECT_NAME]_php /bin/bash
composer install
```
6. You should be able to visit your site at `http://PROJECT_BASE_URL`

Below are the `README` files for `docker4drupal` and `Composer Drupal Project`


# Docker-based Drupal stack

[![Build Status](https://travis-ci.org/wodby/docker4drupal.svg?branch=master)](https://travis-ci.org/wodby/docker4drupal)

## Introduction

Docker4Drupal is a set of docker images optimized for Drupal. Use `docker-compose.yml` file from the [latest stable release](https://github.com/wodby/docker4drupal/releases) to spin up local environment on Linux, Mac OS X and Windows.

* Read the docs on [**how to use**](https://wodby.com/docs/stacks/drupal/local#usage)
* Follow [@wodbycloud](https://twitter.com/wodbycloud) for future announcements
* Join [community slack](https://slack.wodby.com) to ask questions

## Stack

The Drupal stack consist of the following containers:

| Container       | Versions            | Service name    | Image                              | Default |
| --------------- | ------------------- | --------------- | ---------------------------------- | ------- |
| [Nginx]         | 1.15, 1.14          | `nginx`         | [wodby/nginx]                      | ✓       |
| [Apache]        | 2.4                 | `apache`        | [wodby/apache]                     |         |
| [Drupal]        | 8, 7                | `php`           | [wodby/drupal]                     | ✓       |
| [PHP]           | 7.3, 7.2, 7.1, 5.6* | `php`           | [wodby/drupal-php]                 |         |
| [MariaDB]       | 10.3, 10.2, 10.1    | `mariadb`       | [wodby/mariadb]                    | ✓       |
| [PostgreSQL]    | 11, 10, 9.x         | `postgres`      | [wodby/postgres]                   |         |
| [Redis]         | 5, 4                | `redis`         | [wodby/redis]                      |         |
| [Memcached]     | 1                   | `memcached`     | [wodby/memcached]                  |         |
| [Varnish]       | 6.0, 4.1            | `varnish`       | [wodby/varnish]                    |         |
| [Node.js]       | 10, 8, 6            | `node`          | [wodby/node]                       |         |
| [Drupal node]   | 1.0                 | `drupal-node`   | [wodby/drupal-node]                |         |
| [Solr]          | 7.x, 6.6, 5.5, 5.4  | `solr`          | [wodby/solr]                       |         |
| [Elasticsearch] | 6.x, 5.6, 5.5, 5.4  | `elasticsearch` | [wodby/elasticsearch]              |         |
| [Kibana]        | 6.x, 5.6, 5.5, 5.4  | `kibana`        | [wodby/kibana]                     |         |
| [OpenSMTPD]     | 6.0                 | `opensmtpd`     | [wodby/opensmtpd]                  |         |
| [Mailhog]       | latest              | `mailhog`       | [mailhog/mailhog]                  | ✓       |
| [AthenaPDF]     | 2.10.0              | `athenapdf`     | [arachnysdocker/athenapdf-service] |         |
| [Rsyslog]       | latest              | `rsyslog`       | [wodby/rsyslog]                    |         |
| [Blackfire]     | latest              | `blackfire`     | [blackfire/blackfire]              |         |
| [Webgrind]      | 1.5                 | `webgrind`      | [wodby/webgrind]                   |         |
| [Xhprof viewer] | latest              | `xhprof`        | [wodby/xhprof]                     |         |
| Adminer         | 4.6                 | `adminer`       | [wodby/adminer]                    |         |
| phpMyAdmin      | latest              | `pma`           | [phpmyadmin/phpmyadmin]            |         |
| Portainer       | latest              | `portainer`     | [portainer/portainer]              | ✓       |
| Traefik         | latest              | `traefik`       | [_/traefik]                        | ✓       |

Supported Drupal versions: 8 / 7

*❕PHP 5.6 [has reached end of life](http://php.net/supported-versions.php) and no longer supported by PHP team, we strongly advise you to migrate to the latest stable 7.x version.

## Documentation

Full documentation is available at https://wodby.com/docs/stacks/drupal/local.

============================

# Composer template for Drupal projects

[![Build Status](https://travis-ci.org/drupal-composer/drupal-project.svg?branch=8.x)](https://travis-ci.org/drupal-composer/drupal-project)

This project template provides a starter kit for managing your site
dependencies with [Composer](https://getcomposer.org/).

If you want to know how to use it as replacement for
[Drush Make](https://github.com/drush-ops/drush/blob/8.x/docs/make.md) visit
the [Documentation on drupal.org](https://www.drupal.org/node/2471553).

## Usage

First you need to [install composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions below refer to the [global composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar)
for your setup.

After that you can create the project:

```
composer create-project drupal-composer/drupal-project:8.x-dev some-dir --no-interaction
```

With `composer require ...` you can download new dependencies to your
installation.

```
cd some-dir
composer require drupal/devel:~1.0
```

The `composer create-project` command passes ownership of all files to the
project that is created. You should create a new git repository, and commit
all files not excluded by the .gitignore file.

## What does the template do?

When installing the given `composer.json` some tasks are taken care of:

* Drupal will be installed in the `web`-directory.
* Autoloader is implemented to use the generated composer autoloader in `vendor/autoload.php`,
  instead of the one provided by Drupal (`web/vendor/autoload.php`).
* Modules (packages of type `drupal-module`) will be placed in `web/modules/contrib/`
* Theme (packages of type `drupal-theme`) will be placed in `web/themes/contrib/`
* Profiles (packages of type `drupal-profile`) will be placed in `web/profiles/contrib/`
* Creates default writable versions of `settings.php` and `services.yml`.
* Creates `web/sites/default/files`-directory.
* Latest version of drush is installed locally for use at `vendor/bin/drush`.
* Latest version of DrupalConsole is installed locally for use at `vendor/bin/drupal`.
* Creates environment variables based on your .env file. See [.env.example](.env.example).

## Updating Drupal Core

This project will attempt to keep all of your Drupal Core files up-to-date; the
project [drupal-composer/drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold)
is used to ensure that your scaffold files are updated every time drupal/core is
updated. If you customize any of the "scaffolding" files (commonly .htaccess),
you may need to merge conflicts if any of your modified files are updated in a
new release of Drupal core.

Follow the steps below to update your core files.

1. Run `composer update drupal/core webflo/drupal-core-require-dev symfony/* --with-dependencies` to update Drupal Core and its dependencies.
1. Run `git diff` to determine if any of the scaffolding files have changed.
   Review the files for any changes and restore any customizations to
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `web` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish
   to perform these steps on a branch, and use `git merge` to combine the
   updated core files with your customized files. This facilitates the use
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple;
   keeping all of your modifications at the beginning or end of the file is a
   good strategy to keep merges easy.

## Generate composer.json from existing project

With using [the "Composer Generate" drush extension](https://www.drupal.org/project/composer_generate)
you can now generate a basic `composer.json` file from an existing project. Note
that the generated `composer.json` might differ from this project's file.


## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Should I commit the scaffolding files?

The [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@composer drupal:scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```
### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull
request is often a better solution), you can do so with the
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra
section of composer.json:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```
### How do I switch from packagist.drupal-composer.org to packages.drupal.org?

Follow the instructions in the [documentation on drupal.org](https://www.drupal.org/docs/develop/using-composer/using-packagesdrupalorg).

### How do I specify a PHP version ?

This project supports PHP 5.6 as minimum version (see [Drupal 8 PHP requirements](https://www.drupal.org/docs/8/system-requirements/drupal-8-php-requirements)), however it's possible that a `composer update` will upgrade some package that will then require PHP 7+.

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:
```json
"config": {
    "sort-packages": true,
    "platform": {
        "php": "5.6.40"
    }
},
```
