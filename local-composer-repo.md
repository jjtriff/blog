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
- `require` a list of required packages, directly copied from the global `composer.json`
- `require-all` download all packages, not only tagged ones. In my case I don't want everything from 
the repo so it stays `false`
- `require-dependencies` when `true`, Satis automatically resolves and adds all dependencies, so Composer won’t need to use Packagist.
- `require-dev-dependencies` when `true`, Satis will fetch dependencies needed for developing on top
of packages, so is good for offline use
- `archive` `zip` files will be stored in the `dist` directory and we won’t download `dev` packages

## Building (downloading) the repo

Then, we tell Satis to build the repository:

    $ ./bin/satis build satis.json new-repo-folder/
       
I would recommend raising php default memory limit of 128 mb to much bigger than that to avoid running
into out of memory issues. Is easy to do from the shell:

    $ php -dmemory_limit=1G ./bin/satis build satis.json new-repo-folder/ 
    
This is going to take a while, specially if you have a *sh\*\*ty* connection like mine. 
So go grab a coffee ;)
    
After this, we have a new directory called `new-repo-folder`, which contains two files: 
`packages.json` and `index.html`. The former is the repository configuration file, 
the one that will be read by Composer to determine what packages the repository offers. 
The `index.html` file is just a static html file with information about the repository. 
It also contains the `dist` directory with all packages 
so they won’t have to be downloaded from GitHub anymore.

And finally, we publish our repository using the PHP built-in server
(for real repositories it is recommended to use Apache or nginx):

    $ php -S localhost:8001 -t new-repo-folder/

Now, if we go to `http://localhost:8001/`, we should see a webpage that states that our server is there,
updated and with certain packages and versions.

At the top of the page should be the `repositories` entry that you'll need to add to your Composer global
config.

## Using the repo in Composer

Once we have the repository up and running, the last step is to tell Composer that we want to use it. 
This can be done by adding the following lines to the `composer.json` file of your project or in 
`~/.config/composer/config.json` for a global use of this.

    {
        "repositories" : [
            {"type" : "composer", "url" : "http://localhost:8001/"}
        ],
        ...
    }
    
Then, when executing composer install, our custom repository should be used.
