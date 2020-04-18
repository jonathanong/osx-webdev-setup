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

The `.nvmrc` route of selecting the version of node to use is helpful when you are working on multiple projects with different versions of node. Create a `~/.nvmrc` file with a version of node you'd like to use or, for LTS versions, `lts/*`:

```zsh
echo "lts/*" > ~/.nvmrc
```

Then install that version of node:

```zsh
nvm install
```

Note that you should run `nvm install` once in a while to get an updated version of node.

Then in your `~/.zshrc`, add the following line to always use the version of node the current working directory expects:

```zsh
nvm use
# `nvm install`, which is slower, but will always install the latest version of node
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

Then follow the instructions:

```zsh
brew info thefuck
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
- [SF Fonts](https://developer.apple.com/fonts/)
- [VS Code](https://code.visualstudio.com/)
- [GitHub Desktop](https://desktop.github.com/)
- [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)
