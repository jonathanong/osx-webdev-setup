# The Perfect Web Development Setup for OS X / macOS

This is what I perceive as the perfect web development OS X setup for JavaScript engineers.

## Goals

- Never use `sudo`. Once you run a command with `sudo`, future commands are probably gonna be fucked up as well.
- Automatically start all services. Don't bother keeping track a bunch of terminals running processes.
- Don't shoot yourself in the foot.

## Environment Profile

The new default shell in macOS is zsh.
Create and update your profile at `~/.zshrc`.

## XCode Command Line Tools

First, you need to install XCode's command line tools.
This installs a lot of tools like `git` which aren't needed for plebeians.

```zsh
xcode-select --install
```

## Install Homebrew

[Homebrew](https://brew.sh/) is a macOS package manager.
It makes setting up all your services very easy.

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

## Install everything

Install everything with Homebrew.
Here are some packages you might be interested in right now:

```zsh
# for editing files
brew install vim
# always keep your git up to date by installing it with brew
brew install git
# download stuff
brew install curl
# for compiling
brew install gcc
```

### Updating

To update your packages,
simply run:

```zsh
brew update
brew upgrade
brew cleanup
```

## Install node.js with nvm

One thing many users do is install node.js globally.
This is easy to get started or fine for servers,
but it makes developing a pain.
If you have to ever run `npm` with `sudo`,
you're doing it wrong!

[nvm](https://github.com/nvm-sh/nvm) is what I think is the best node version manager.
It can be installed with homebrew!
Yes, you use a package manager to install a version manager to install another package manager.
It's stupid, but they all have their strengths.

```zsh
brew install nvm
```

Then, follow the installation instructions:

```zsh
% brew info nvm
```

Then, set a default version of node.js. To use LTS:

```zsh
nvm alias default lts/*
```

### Default node.js version via `.nvmrc`

Create a `~/.nvmrc` file with a version of node you'd like to use or, for LTS versions, `lts/*`:

```zsh
echo "lts/*" > ~/.nvmrc
```

Then install that version of node:

```zsh
nvm install
```

Then in your `~/.zshrc`, add the following line to always use the version of node the current working directory expects:

```zsh
nvm use
```

Now, `nvm` will find the nearest `.nvmrc` file and use that version of node whenever the terminal starts.

### Using npm

With `nvm`, you should never be calling any `npm` functions with `sudo`.
`nvm` adds binaries installed globally to your `$PATH`, so if you install `npm i -g create-react-app`, `create-react-app` will automatically be added to your `$PATH`.

To upgrade `npm`, run `npm -i -g npm`. 

To have [node binaries install faster](https://www.npmjs.com/package/node-gyp), add the following to your `~/.nvmrc`:

```zsh
export NPM_CONFIG_JOBS=max
```

If you hate the npm progress bar like, add this to your `zshrc`:

```zsh
export NPM_CONFIG_PROGRESS=false
```

## thefuck?

[thefuck](https://github.com/nvbn/thefuck) is a nifty tool that allows you to fix your previous CLI typos by just typing `fuck`.
It perhaps has the greatest UX of all products, ever.

Installing it is easy:

```zsh
brew install thefuck
```

Then alias it as `fuck` (or whatever you want) manually by adding this line to your `~/.bash_profile`:

```env
alias fuck='$(thefuck $(fc -ln -1))'
```

## globstars

OS X, by default, does not support globstars like `**/*.js`.
It may work for certain packages who support it,
but not by default.
To add support for it, install the latest version of bash with `brew install bash`,
then set the default shell in terminal to `/usr/local/bin/bash`.

Then add the following line to your `~/.bash_profile`:

```zsh
echo "shopt -s globstar" >> ~/.bash_profile
```

or just:

```
shopt -s globstar
```

## vim

By default, `vim` installed via `brew` sucks.
Create a `~/.vimrc` with the following:

```
:set nocompatible
syntax on
```

## git

`git` by default doesn't have autocompletion on OS X.
Super easy to [install it](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion):

```zsh
brew install bash-completion
```

Then add this line to your `~/.bash_profile`:

```zsh
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi
```

Make sure your git pushes only the current branch.
Run the following:

```zsh
git config --global push.default simple
```

To have git user the OS X Keychain, run this command:

```zsh
git config --global credential.helper osxkeychain
```

## Setting up databases

Homebrew makes setting up databases super easy.
First step - install it with Homebrew:

```zsh
brew install redis
```

Then you'll see information on your terminal like the following:

```
To have launchd start redis now and restart at login:
  brew services start redis
```

To read this information again, just type `brew info redis`.
Run the command and, Voila!
`redis-server` will always be running!
You won't have to open a bunch of terminals to keep it running!

Rinse and repeat for all your databases.

## brew options

A lot of packages on Homebrew have terrible defaults.
I haven't bothered making a PR to update these defaults,
mostly because I don't have a reason to change the defaults other than, "why not?"

For example, type the following:

```zsh
brew options ffmpeg
```

You're probably overloaded with options.
Fun isn't it?
Supposedly, once you install a package with homebrew using specific options,
future updates will use the same options.
I haven't found that to be the case - I have to reinstall `ffmpeg` many times - but I'm not going to try reproducing it.

Have fun reading all the option info and typing commands like:

```zsh
brew install ffmpeg --with-faac --with-libssh --with-libvorbis --with-libvpx --with-openssl --with-opus --with-theora --with-webp --with-x265
```

Not only will this install all the dependencies like `webp`,
it will make sure you can pretty much throw anything at `ffmpeg`.

You'll probably have to do the same with `imagemagick` and/or `graphicsmagick`.

## Ruby

Lots of programs use ruby, so be sure to install it!

```zsh
brew install ruby
```

## Java

Unfortunately, a lot of programs still require Java.
Install Java by googling "Java OS X".
https://support.apple.com/kb/DL1572?locale=en_US

## DNS Server

Set Google as your computer's DNS server and default search domain,
which will almost always be better than your ISP's default settings.
https://developers.google.com/speed/public-dns/docs/using?hl=en#mac_os

## Other Tools

- [iStat Menus](http://bjango.com/mac/istatmenus/) - help me figure out if something's taking too much CPU, RAM, or network
- [Atom](https://atom.io/) - the best text editor :D
- [Sublime Text](http://www.sublimetext.com/) - the second best text editor
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - for VMs, which can't be install using Homebrew
- [SF Fonts](https://developer.apple.com/fonts/)
