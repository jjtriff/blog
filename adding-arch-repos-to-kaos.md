# Adding Arch linux repos to Kaos Linux

This is mainly a reminder on how to do things, because it all boils down to knowing how to configure Pacman package manager.

## pacman.conf

This is the config file for pacman. The interesting part here is the defined Repos. As it says right there:

    # REPOSITORIES
    #   - can be defined here or included from another file
    #   - pacman will search repositories in the order defined here
    #   - local/custom mirrors can be added here or in separate files
    #   - repositories listed first will take precedence when packages
    #     have identical names, regardless of version number
    #   - URLs will have $repo replaced by the name of the current repo
    #   - URLs will have $arch replaced by the name of the architecture
    #
    # Repository entries are of the format:
    #       [repo-name]
    #       Server = ServerName
    #       Include = IncludePath
    #
    # The header [repo-name] is crucial - it must be present and
    # uncommented to enable the repo.

So we have to create a [repo] section to use. Within this section there are 2 variables we can use: `$repo` and `$arch`.

1. `$repo` will be converted into whatever we name the repo and will be used to search for a particular file inside the final URL that matches `$repo`.db
1. `$arch` will be converted into the architecture you are running in my case x86_64

Inside a [repo] section it must be a Server URL setting that will be used to conform the addresses of packages.

It'll look like this

        [community]
        Server = https://mirror.math.princeton.edu/pub/archlinux/$repo/os/$arch/

Pacman will go out there and try to update the [community] repo downloading the file https://mirror.math.princeton.edu/pub/archlinux/community/os/x86_64/community.db.
 *How do I know that?* Well, running `pacman` in debug mode gives a lot of info, including what its trying to get and what it didn't find:

    # pacman --debug -Syy
    debug: pacman v5.0.2 - libalpm v10.0.2
    debug: config: attempting to read file /etc/pacman.conf
    debug: config: new section 'options'
    debug: config: HoldPkg: pacman
    debug: config: HoldPkg: glibc
    debug: config: arch: x86_64
    debug: config: verbosepkglists
    debug: config: chomp
    debug: config: SigLevel: Never
    debug: config: LocalFileSigLevel: Never
    debug: config: new section 'community'
    debug: config: finished parsing /etc/pacman.conf
    debug: setup_libalpm called
    debug: option 'logfile' = /var/log/pacman.log
    debug: option 'gpgdir' = /etc/pacman.d/gnupg/
    debug: option 'hookdir' = /etc/pacman.d/hooks/
    debug: option 'cachedir' = /var/cache/pacman/pkg/
    debug: registering sync database 'community'
    debug: database path for tree community set to /var/lib/pacman/sync/community.db
    debug: setting usage of 15 for community repository
    debug: adding new server URL to database 'community': https://mirror.math.princeton.edu/pub/archlinux/community/os/x86_64
    :: Synchronizing package databases...
    debug: url: https://mirror.math.princeton.edu/pub/archlinux/community/os/x86_64/community.db
    debug: maxsize: 26214400
    debug: opened tempfile for download: /var/lib/pacman/sync/community.db.part (wb)
    downloading community.db...
    debug: curl returned error 0 from transfer
    debug: response code: 200
    debug: unregistering database 'local'
    debug: unregistering database 'community'

## Keeping it DRY *(Don't Repeat Yourself)*

If you have to include a lot of repos you should put the well formed URL with $repo an $arch vars in a separate file and include it for as many repos you want. For instance, lets create a file `/etc/pacman.d/urllist`:

        Server = https://mirror.math.princeton.edu/pub/archlinux/$repo/os/$arch/

Then inside the pacman.conf we will do:

        [community]
        Include = /etc/pacman.d/urllist

        [core]
        Include = /etc/pacman.d/urllist

That will do the trick. After all this don't forget to:

    # pacman -Syy

to update your repos info and be able to use vanilla tools like `Octopi`


