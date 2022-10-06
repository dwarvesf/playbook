# Code editors

Right now our team is scattered between VSCode and Emacs (Spacemacs).

> The VSCode gang enjoy pretty syntax themes, excellent auto-completion for most languages, fast integrated terminal and awesome Git integration.

> The Spacemacs gang live far from the human realm, with all 10 fingers moving constantly to ramp through everything they open, writing Elisp to change editor's behavior on-the-fly, switching projects or playing next Spotify song is just a keymap away. They ain't fear nothing.

![image](https://cdn-images-1.medium.com/max/1600/1*yrFfS0uIjjwKLkB9KyU3Tg.jpeg)

### Using the right tool for the job

_A well personalized (and configured) editor can significantly boost your productivity, pick one and make it yours._

Everyone has their own set of favorite tools, if you're unsure which tool to pick - just use the one that works for the task you're up to. In the end it all comes down to the result you deliver, not what you used to achieve it.

Some personal preferences (for starters) that might helps:

- For project-based development: VSCode
- For single file editing (.eg config files): VSCode/Vim

If you're in doubt, go with [VSCode](https://code.visualstudio.com/).

### Should I learn Vim?

Yes, seriously. Maybe not now, maybe you will end up hating it and move back to VSCode (or turned into a [Spacemacs](http://spacemacs.org/) fanboy), but its a [big](https://pascalprecht.github.io/posts/why-i-use-vim/) [fat](https://www.quora.com/What-are-the-advantages-of-Vim-over-other-text-editors) [_YES_](https://www.quora.com/How-useful-is-learning-VI-VIM-for-a-new-programmer).

## Mouse vs keyboard

Learn your editor's keybindings - after you have gotten used to your editor, try to learn (and remember) useful shortcuts/keybindings such as switching between tabs, open project tree, open terminal, .etc. It feels easy enough to just use mouse/trackpad for those task, but once you got used to keybindings you will get things done much faster than hovering a mouse, in almost all circumstances.

If you using Mac and wanted powerful keybinding across system checkout [Karabiner-Elements](https://karabiner-elements.pqrs.org/). Our engineers are using it to utilize the Caps lock the result is less keystroke and get more shortcuts

## Useful plugins for your editors

### VScode

In our workplace, we often use [VSCode](https://code.visualstudio.com/) with other [editors][editor.md] to help bootstrap our development environment. It's a convenient text-editor with a ton of extensions, enough to replace modern IDE workloads. This is possible with the advent of [language servers](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide) to help create rich IDE-like experiences for developers.

VSCode has a feature called [Settings Sync](https://code.visualstudio.com/docs/editor/settings-sync) that uses your Microsoft or GitHub account to sync your `settings.json`, `keybindings.json`, `extensions.json`, etc. to keep your settings and configuration in sync.

There are a ton of extensions available in the [VSCode Marketplace](https://marketplace.visualstudio.com/vscode). What you use and style with is up to you. We often program with languages such as [TypeScript](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-next), [Golang](https://code.visualstudio.com/docs/languages/go), and [Elixir](https://marketplace.visualstudio.com/items?itemName=JakeBecker.elixir-ls) and use their respective plugins for our projects.

Some of our other recommended extensions are:

- [GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)
- [Go](https://marketplace.visualstudio.com/items?itemName=golang.Go)
- [Eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Jest Runner](https://marketplace.visualstudio.com/items?itemName=firsttris.vscode-jest-runner)
- [Tailwind IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
- [ES7+ React/Redux/React-Native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
- [Makefile Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.makefile-tools)
- [Indent Rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)

### Vim/NeoVim

#### VIM/NVIM Plugins

[https://vimawesome.com/](https://vimawesome.com/)
[https://github.com/rockerBOO/awesome-neovim](https://github.com/rockerBOO/awesome-neovim)

Both Vim and Neovim have really extensive plugins and comprehensive guides to customing the editor to fit your needs. If you are looking for a VSCode or IDE-like experience, we highly recommend using neovim with the [coc.nvim](https://github.com/neoclide/coc.nvim) plugin.

### Emacs
