# Terminal

## Set up Unix like terminal

- On Linux you can use default terminal

- On MacOS, it comes with a default terminal, although we prefer to use third-part terminals. For instance:
  - [iTerm2](https://iterm2.com/)
  - [Kitty](https://sw.kovidgoyal.net/kitty/)
  - [Warp](https://www.warp.dev/)
  - [Hyper.is](https://hyper.is/)

- Windows is kinda tricky, to have Unix like command you use have to install [WLS](https://learn.microsoft.com/en-us/windows/wsl/install). You can also use new [Terminal](https://github.com/microsoft/terminal) which is more powerful easier to use

## Command-line Plugins

Most likely, you might be using `zsh`, `tmux`, or `vim`/`nvim` as part of your workflow. They are powerful on their own, but lack some of the love and care of GUI tools we take for granted today. If you feel this might be the case for you, you can try extending these tools with plugins. Their configs usually reside in the `~/.config` folder, which you can version control with your dotfiles.

## Setup zsh and plugins

ZSH plugins give extra features such as rich history search, better autocompletion, snippets, etc. to better facilitate developer workflows. A lot of plugins also help make your terminal look better.

Before adding a plugin, you need to choose a plugin manager. You can find an appropriate plugin framework as well as plugins at [https://github.com/unixorn/awesome-zsh-plugins](https://github.com/unixorn/awesome-zsh-plugins). The most popular plugin manager is [oh-my-zsh](https://ohmyz.sh/), while one of the fastest ones is [zinit](https://github.com/zdharma-continuum/zinit).

Our team prefer to use Z shell (Zsh) to execute commands, with [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh) as plugins manager

Useful plugins/script you should install/enable to save your time.

- git - save your time with git aliases
- z - jump around you visited directory quickly
- dotenv - auto load your .env file
- zsh-completions
- zsh-autosuggestions
- zsh-syntax-highlighting

## Setup tmux and plugins

Although our team often uses tabs on GUI terminals, there is always the option to use `tmux` to help manage multiple terminal sessions.

### TMUX Plugins

If you use `tmux` to manage multiple terminal sessions, you might be interested in using [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm). You can find a list of tmux plugins compatible with the plugin manager at [https://github.com/orgs/tmux-plugins/repositories?type=all](https://github.com/orgs/tmux-plugins/repositories?type=all). For instance, If you often find yourself re-scaffolding your tmux environments, we highly recommend using [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect).
