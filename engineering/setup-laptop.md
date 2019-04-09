# Dotfiles

In UNIX, the files start with a dot “.” are hidden. If you list files in the directory, they don’t show up and keep them safe from the end users. Because of that reason, the developer usually uses it to store configurations of their tools. 

Those files are so-called dotfiles. 'Dotfile' become a generalized term for a UNIX configuration file, typically prefixed with a dot (e.g., .vimrc) and found in your home directory. Most UNIX programs, including Vim, will load a dotfile during launch.

We recommend using dotfiles to customize your tools and environment to suit your preferences, reduce typing, and get work done. Check them into a git repository for safe-keeping and open-source for the benefit of others.

## Share configuration with dotfiles

If you are new for dotfiles, you can have a quick look at https://dotfiles.github.io. They organize most popular dotfiles for you to look at or start with. You can browse some repo to learn from the community, discover new tools for your toolbox and new tricks for the ones you already use.

## Automate your development environment

https://github.com/dwarvesf/dotfiles

We use MacOS and few popular Linux distros as a environment for development. We depend on compilers, databases, programming languages, package management systems, installers, and other critical programs for our daily activities.

We have our [dotfiles](https://github.com/dwarvesf/dotfiles), a script to share our configuration and variable. It helps to set up a laptop with the programs required for web and mobile development. It should take less than 15 minutes to install.

Using an automated setup helps us to stay up-to-date with new operating system and program versions. Also, because the setup is standardized, new team members are able to quickly join a project without wasting time re-configuring their machine. 

Using the [dotfiles](https://github.com/dwarvesf/dotfiles) make pair programming with teammates easier and make each other more productive.