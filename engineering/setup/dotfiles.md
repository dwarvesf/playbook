## Dotfiles

In UNIX, the files start with a dot “.” are hidden. If you list files in the directory, they don’t show up and keep them safe from the end users. Because of that reason, the developer usually uses it to store configurations of their tools.

Those files are so-called dotfiles. 'Dotfile' become a generalized term for a UNIX configuration file, typically prefixed with a dot (e.g., .vimrc) and found in your home directory. Most UNIX programs, including Vim, will load a dotfile during launch.

We recommend using dotfiles to customize your tools and environment to suit your preferences, reduce typing, and get work done. Check them into a git repository for safe-keeping and open-source for the benefit of others.

There are a few ways you can create dotfiles:
- **Bare git repository**: A way to manage dotfiles with git and a few command aliases.
  - reference: https://www.atlassian.com/git/tutorials/dotfiles
- **Tools and frameworks**: A specialized tool used to manage local dotfiles
  - e.g: https://github.com/kazhala/dotbare
  - e.g: https://github.com/bashdot/bashdot
- **Custom scripts**: Using custom bash scripts to bootstrap dotfiles from a repo to the machine
  - e.g: https://github.com/dwarvesf/dotfiles

### Share configuration with dotfiles

If you are new for dotfiles, you can have a quick look at https://dotfiles.github.io. They organize most popular dotfiles for you to look at or start with. You can browse some repo to learn from the community, discover new tools for your toolbox and new tricks for the ones you already use.

### Automate your development environment

https://github.com/dwarvesf/dotfiles

We use MacOS and few popular Linux distros as a environment for development. We depend on compilers, databases, programming languages, package management systems, installers, and other critical programs for our daily activities.

We have our [dotfiles](https://github.com/dwarvesf/dotfiles), a script to share our configuration and variable. It helps to set up a laptop with the programs required for web and mobile development. It should take less than 15 minutes to install.

Using an automated setup helps us to stay up-to-date with new operating system and program versions. Also, because the setup is standardized, new team members are able to quickly join a project without wasting time re-configuring their machine.

Using the [dotfiles](https://github.com/dwarvesf/dotfiles) make pair programming with teammates easier and make each other more productive.

## Command-line Plugins

Most likely, you might be using `zsh`, `tmux`, or `vim`/`nvim` as part of your workflow. They are powerful on their own, but lack some of the love and care of GUI tools we take for granted today. If you feel this might be the case for you, you can try extending these tools with plugins. Their configs usually reside in the `~/.config` folder, which you can version control with your dotfiles.

### ZSH Plugins

ZSH plugins give extra features such as rich history search, better autocompletion, snippets, etc. to better facilitate developer workflows. A lot of plugins also help make your terminal look better.

Before adding a plugin, you need to choose a plugin manager. You can find an appropriate plugin framework as well as plugins at [https://github.com/unixorn/awesome-zsh-plugins](https://github.com/unixorn/awesome-zsh-plugins). The most popular plugin manager is [oh-my-zsh](https://ohmyz.sh/), while one of the fastest ones is [zinit](https://github.com/zdharma-continuum/zinit).

### TMUX Plugins

If you use `tmux` to manage multiple terminal sessions, you might be interested in using [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm). You can find a list of tmux plugins compatible with the plugin manager at [https://github.com/orgs/tmux-plugins/repositories?type=all](https://github.com/orgs/tmux-plugins/repositories?type=all). For instance, If you often find yourself re-scaffolding your tmux environments, we highly recommend using [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect).

### VIM/NVIM Plugins

[https://vimawesome.com/](https://vimawesome.com/)
[https://github.com/rockerBOO/awesome-neovim](https://github.com/rockerBOO/awesome-neovim)

Both Vim and Neovim have really extensive plugins and comprehensive guides to customing the editor to fit your needs. If you are looking for a VSCode or IDE-like experience, we highly recommend using neovim with the [coc.nvim](https://github.com/neoclide/coc.nvim) plugin.

## Applications

In our workplace, we often use [VSCode](https://code.visualstudio.com/) with other [editors][editor.md] to help bootstrap our development environment. It's a convenient text-editor with a ton of extensions, enough to replace modern IDE workloads. This is possible with the advent of [language servers](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) to help create rich IDE-like experiences for developers.

VSCode has a feature called [Settings Sync](https://code.visualstudio.com/docs/editor/settings-sync) that uses your Microsoft or GitHub account to sync your `settings.json`, `keybindings.json`, `extensions.json`, etc. to keep your settings and configuration in sync.

### VSCode Extensions

There are a ton of extensions available in the [VSCode Marketplace](https://marketplace.visualstudio.com/vscode). What you use and style with is up to you. We often program with languages such as [TypeScript](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-next), [Golang](https://code.visualstudio.com/docs/languages/go), and [Elixir](https://marketplace.visualstudio.com/items?itemName=JakeBecker.elixir-ls) and use their respective plugins for our projects.
