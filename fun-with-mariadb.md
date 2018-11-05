# Fun with MariaDB

Had a thing with this free distro of MySQL that had to uninstall it. The thing is that the `data` dir, which houses all the databases, had grown beyond my spectations: around 9.8 GB.

That was a problem in my 15 G system partition, that led me into problems with system updates, and other stuff. **That growth is another thing that I have to look into**.

So I had to remove everything and re-install it, because I use it for work all the time.

Removing was fairly simple:

    # pacman -Rns -dd mariadb

`-dd` option to skip the dependencies check.

Re-installing was a different deal.

## Re-installing MariaDB

To be honest all of this could have been avoided in vanilla distros like `Ubuntu`. Everything there is so relaxed that must of the time just works but... it ends up lagging the whole PC, *I don't know why*, but that feels just like `Windows`, with the sweetener of a [K Desktop Environment](http://kde.org).

Uhhhhhh, I love KDE :-)

Well, hands into business:

    # pacman -S mariadb
    
That will install `MariaDB` but is not enough. There are some things we need to configure. All of this things are detailed in the `2.10 Postinstallation Setup and Testing` of the MySQL 5.7 Reference Manual. These are:

1. Initializing the data directory
1. Start the server
1. Securing the Initial MySQL Accounts, *by this I mean setting a password for `root` user*.

## Initializing the data directory and suddenly everything else

There are 2 ways to do these and depend on your *MyMaria* version. I'm just guessing but I think MariaDB will always be an outdated MySQL version. So in this case we'll have to take the old way which is `2.10.1.2 Initializing the Data Directory Manually Using mysql_install_db`

Long story short:

    # mysql_install_db --user=mysql \
            --basedir=/usr \
            --datadir=/var/lib/mysql/data
            
Some things to notice here on the params:

- `basedir` is set to `/usr/` because `mysql_install_db` will be looking for *`basedir`/bin/mysql*, and other binaries that are in `/usr/bin`.
- `datadir` is set to that path which houses the data files at least in these Linux distro that I'm using now [KaOS Linux](http://kaosx.us)
- `user` is set to `mysql` so everything will remain under the mysql user once is created

The output will be something like these (I'm cutting off non relevant parts to keep it short, but try to get the idea):

```
Installing MariaDB/MySQL system tables in '/var/lib/mysql/data' ...
2018-11-04 15:12:30 140662934899904 [Note] /usr/bin/mysqld (mysqld 10.1.36-MariaDB) starting as process 4311 ...
...
...
2018-11-04 15:12:30 140662799276800 [Warning] Failed to load slave replication state from table mysql.gtid_slave_pos: 1146: Table 'mysql.gtid_slave_pos' doesn't exist
OK
Filling help tables...
....
OK
Creating OpenGIS required SP-s...
....
OK
...

PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:

'/usr/bin/mysqladmin' -u root password 'new-password'
'/usr/bin/mysqladmin' -u root -h asuncion password 'new-password'

Alternatively you can run:
'/usr/bin/mysql_secure_installation'

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

...
You can start the MariaDB daemon with:
cd '/usr/' ; /usr/bin/mysqld_safe --datadir='/var/lib/mysql/data'


The latest information about MariaDB is available at http://mariadb.org/.
...
```

So in that output we have everything we need to complete the rest of the setup:

    # sudo -u mysql mysqld &
    # mysqladmin -u root password 'new-password'

First command will fireup the server and put the process in background. It is saying to sudo to run as the `mysql`  user.

The second command sets the password for the root user on the new server. And that is by far the most easy way to do these.

## Resetting password for the forgotten root account

This steps are described more extensibly in the MySQL manual: `B.5.3.2.2 Resetting the Root Password: Unix and Unix-Like Systems` but here is the sum up:

1. Shutdown the mysqld server
1. Create a .sql script to reset the password
1. Start the mysqld instructing to execute the password-resetting-script
1. Test everything works

The hard part here is the script and starting the server again.

The script depends on what version of MySQL you are using, as these is MariaDB lets go one more time with the outdated instructions:

    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('MyNewPass');

Save that into `~/init.sql`, for example. Now use it:

    $ sudo -u mysql mysqld --init-file=~/init.sql &

Give it a try to log into MySQL using `mysql` client:

    $ mysql -u root -p
    Enter password: MyNewPass


## Conclusion

That's it. Enjoy having fun with MariaDB.

