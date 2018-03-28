# Adding Arch linux repos to Kaos Linux

This is mainly a reminder on how to do things, because it all boils down to knowing how to configure Pacman package manager.

# pacman.conf

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
    
