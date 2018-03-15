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
    
## Selecting the packages that we want

Our intention is to create a repo with the packages that we use a lot and its dependencies. So:
 how do you find out which packages you'll need? Answer: by looking at your composer.json.
 
Lets say we want to install Laravel 5. We'll normally go 
    
    $ composer create-project --prefer-dist laravel/laravel blog
    
    


## Repository configuration file


Let’s do it for a  that we are a company with a few developers and all our projects only need two dependencies: the Symfony HttpFoundation component and Twig. We want to have a custom repository with all versions (excluding development versions) of those two packages so we don’t have to rely on GitHub:
