# Creating a local Composer repo with Satis

This article is based on http://blog.servergrove.com/2015/04/29/satis-building-composer-repository/
which gave me a great guidance, and some modifications made to the customs explained there.

The point is to create a local Composer repo that allows you to keep developing (creating projects, 
installing dependencies) without having to be online all the time.

## Who needs this?

People, like me, who spent must of their live offline, because they have poor internet, and
they basically don't trust the cloud, 'cause the cloud is never there.

## Installation

We can install Satis using Composer:

    $ composer create-project composer/satis --stability=dev
    
## Selecting the packages that we want (require)

Our intention is to create a repo with the packages that we use a lot and its dependencies. So:
 how do you find out which packages you'll need? Answer: by looking at your composer.json.
 
Lets say we want to install Laravel 5. We'll normally go 
    
    $ composer create-project --prefer-dist laravel/laravel blog
    
This says that we need package `laravel/laravel`. If we install that package globally we'll end up with
a `composer.json` file inside our global Composer config dir (mine is `~/.config/composer`) that
expresses neatly the packages we require:

        {
            "require": {
                "laravel/installer": "^2.0",
                "laravel/laravel": "^5.6",
                "fabpot/goutte": "^3.2",
                "guzzlehttp/guzzle": "^6.3",
                "laravel/valet": "^2.0"
            }
        }


## Repository configuration file

Now we know what we need we can go for the `satis.json` file, which is the place to config things.
This file will be inside the `satis` folder where you issue the `composer create-project composer/satis`
command.

Take a look at mine:

        {
            "name": "jtriff repository",
            "homepage" : "http://localhost:8001",
            "repositories": [
                        {
                    "type": "composer",
                    "url": "https://packagist.jp"
                }
            ],
            "require": {
                "laravel/installer": "^2.0",
                "laravel/laravel": "^5.6",
                "fabpot/goutte": "^3.2",
                "guzzlehttp/guzzle": "^6.3",
                "laravel/valet": "^2.0"
            },
            "require-all": false,
            "require-dependencies": true,
            "require-dev-dependencies" : true,
            "archive": {
                "directory": "dist",
                "format": "zip",
                "skip-dev": true
            }
        }
        
### Explanation

Fields `name` and `homepage` are required. The other ones:

- `repositories` a list of the repositories that you'll use to get packages
- `require-all` download all packages, not only tagged ones. In my case I don't want everything from 
the repo so it stays `false`
- `require-dependencies` when `true`, Satis automatically resolves and adds all dependencies, so Composer won’t need to use Packagist.
- `require-dev-dependencies` when `true`, Satis will fetch dependencies needed for developing on top
of packages, so is good for offline use
- `archive` `zip` files will be stored in the `dist` directory and we won’t download `dev` packages

