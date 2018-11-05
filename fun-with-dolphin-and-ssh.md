# Fun with Dolphin and SSH

Dolphin works great when copying files over SFTP, but if something changes on the remote host things get tricky.

## Background story

The changes were:

1. Had to generate a new personal key for securing SSH communication with the nearby server 192.168.x.x.

All of these was done by the book:

```
$ ssh-keygen    # this generates *~/.ssh/id_rsa* and *id_rsa.pub*

...
$ ssh-copy-id -i ~/.ssh/id_rsa.pub user@192.168.x.x
Enter password for user: ***
```

Worked like a charm. The test:

    ssh -i ~/.ssh/mykey user@host

prove everything was working.

## Coming back to Dolphin

When trying to connect using the usual URL: `sftp://user@192.168.x.x` got these error:

    The host key for this server was not found, but another type of key exists. An attacker might change the default server key to confuse your client into thinking the key does not exist. Please contact your system administrator.

If we are certain that there is not an ongoing attack we should, first of all:

    $ ssh-keygen -R 192.168.x.x

and accept the new certificate answering **yes** on Dolphin to:

```
The authenticity of host 192.168.10.20 cannot be established.
The key fingerprint is: xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:7b
Are you sure you want to continue connecting?
```

This should solve the problem, but if not check the next section.

## What about if I have different key names?

Fear not. Everything boils down to the `OpenSSH SSH client configuration files` gettable by:

    $ info ssh_config

There you can find a lot of config options but I'm going for 2 of them that are quite handy:

1. Location of the key file
1. Port of connection to the SSH server

As stated on the man page: there are 3 sources of the config options: global, user and local environment.

In this case the one that is easier to use is the user source `~/.ssh/config`

    $ touch ~/.ssh/config

The option for the key file is `IdentityFile`:

    $ echo 'IdentityFile "/home/user/.ssh/rsakey_private.part"' >> ~/.ssh/config

The double quotes ensure no problem when spaces are found in the path of the file.

Next: the option for the port to which connect on the remote server. Though there is a catch, because not every server will use a different port for SSH so we need to specify that this *strange* port config is just for our *strange* server. This is done using the `Host` directive. Every time `Host` appears the following config lines will be applied to that particular server.

    $ echo 'Host 192.168.x.x' >> ~/.ssh/config
    $ echo 'Port 15000' >> ~/.ssh/config

## Conclusions

Thanks to sources that I used to fix this problems: 

* [Github Help](https://help.github.com/articles/testing-your-ssh-connection/)
* [How To Configure SSH Key-Based Authentication on a Linux Server](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)
* [SSH browsing in Dolphin using an ssh key file](https://yuenhoe.com/blog/2009/12/ssh-browsing-in-dolphin-using-an-ssh-key-file/)
* [Using ssh to browse remote file system in Dolphin & Konqueror on KDE](https://www.binarytides.com/ssh-dolphin-konqueror-kde/)


Have fun, that's it.
