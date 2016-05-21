# The Perfect Web Development Setup for OS X

This is what I perceive as the perfect web development OS X setup.
It makes web development a breeze.
I've tried to use Ubuntu, but unfortunately it's just not as user friendly as OS X.
I wish to make one for Ubuntu eventually once I figure it out.

Note: this is all from memory and will be revised once I get a new Mac or reformat this one.
Clarifications and PRs are welcomed.

## Goals

- Never use `sudo`. Once you run a command with `sudo`, future commands are probably gonna be fucked up as well.
- Automatically start all services. Don't bother keeping track a bunch of terminals running processes.
- Don't shoot yourself in the foot.

## Environment Profile

My current profile is `~/.bash_profile`.
I don't know why OS X uses `~/.bash_profile`,
but it may be different for you.
Some lines you should add are:

- `ulimit -n 10240` - bumps the maximum number of file descriptors you can have open on your computer.
  There's no purpose for the default limit, especially on SSDs.
- `export JOBS=max` - tells `npm` to compile and install all your native addons in parallel and not sequentially.
  This greatly increases installation times.

## XCode Command Line Tools

First, you need to install XCode's command line tools.
This installs a lot of tools like `git` which aren't needed for plebeians.

```bash
xcode-select --install
```

If you're actually going to use XCode, just install it from the App Store and do the whole shebang.

## Install Homebrew

[Homebrew](http://brew.sh/) is OS X's package manager.
It makes setting up all your services very easy.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Install everything

Install everything with Homebrew.
Here are some packages you might be interested in right now:

```bash
# for editing files
brew install vim
# always keep your git up to date by installing it with brew
brew install git
# download stuff
brew install curl
# docker on your mac
brew install boot2docker
# for your pr0n
brew install youtube-dl
```

### Updating

To update your packages,
simply run:

```bash
brew update
brew upgrade
```

Once in a while, I run `brew prune` and `brew doctor` to keep my computer in check.

## Install node.js with nvm

One thing many users do is install node.js globally.
This is easy to get started or fine for servers,
but it makes developing a pain.
If you have to ever run `npm` with `sudo`,
you're doing it wrong!

[nvm](https://github.com/creationix/nvm) is what I think is the best node version manager.
It can be installed with homebrew!
Yes, you use a package manager to install a version manager to install another package manager.
It's stupid, but they all have their strengths.

```bash
brew install nvm
```

Once you've installed nvm,
install the version of node.js you use:

```bash
nvm install 4
```

To make sure each terminal uses the version of node you want,
add this to your `~/.bash_profile` or whichever environment you use:

```env
# load nvm whenever a terminal starts
source ~/.nvm/nvm.sh
# load the version of nvm you want
nvm use 4
```

Now every time you open a window,
it will say which version of node you are using.
This might be annoying,
but it's better than not knowing what version you're using!

> NOTE: `nvm` slows down creating new terminals, so you may just want to use `brew install node` if you only need one version of node.

### Default node.js version

Create a `~/.nvmrc` file with just the version you'd like.

```bash
echo "6" > ~/.nvmrc
```

Then in your `bash_profile`, add the following line:

```bash
nvm use
```

Now, `nvm` will find the nearest `.nvmrc` file and use that version of node whenever the terminal starts.

### Using npm

Don't ever use `sudo`!
You don't need to `sudo npm install -g grunt-cli` or anything anymore.
Just run `npm install -g eslint` and `eslint` will always be in your path.
`nvm` adds these to your `$PATH`.

Don't ever upgrade `npm`!
At least not until node.js and io.js merge.
If anyone ever tells you to `npm update -g npm` or `npm install -g npm`,
tell them to shuttup.

Remember to add `export JOBS=max` in your `~/.bash_profile` so your `npm install`s are faster!

## thefuck?

[thefuck](https://github.com/nvbn/thefuck) is a nifty tool that allows you to fix your previous CLI typos by just typing `fuck`.
It perhaps has the greatest UX of all products, ever.

Installing it is easy:

```bash
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
To add support for it, install the latest version of bash with `brew install bash`
and follow the instructions at http://mistermorris.com/blog/get-yourself-globstar-bash-4-for-your-mac-terminal/.

Then add the following line to your `~/.bash_profile`:

```bash
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

```bash
brew install bash-completion
```

Then add this line to your `~/.bash_profile`:

```bash
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi
```

Make sure your git pushes only the current branch.
Run the following:

```bash
git config --global push.default simple
```

To have git user the OS X Keychain, run this command:

```bash
git config --global credential.helper osxkeychain
```

## Setting up databases

Homebrew makes setting up databases super easy.
First step - install it with Homebrew:

```bash
brew install redis
```

Then you'll see information on your terminal like the following:

```
To reload redis after an upgrade:
    launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
Or, if you don't want/need launchctl, you can just run:
    redis-server /usr/local/etc/redis.conf
```

To read this information again, just type `brew info redis`.
All I do is copy and paste the first 2 commands listed.
Voila!
`redis-server` will always be running!
You won't have to open a bunch of terminals to keep it running!

Rinse and repeat for all your databases.

## brew options

A lot of packages on Homebrew have terrible defaults.
I haven't bothered making a PR to update these defaults,
mostly because I don't have a reason to change the defaults other than, "why not?"

For example, type the following:

```bash
brew options ffmpeg
```

You're probably overloaded with options.
Fun isn't it?
Supposedly, once you install a package with homebrew using specific options,
future updates will use the same options.
I haven't found that to be the case - I have to reinstall `ffmpeg` many times - but I'm not going to try reproducing it.

Have fun reading all the option info and typing commands like:

```bash
brew install ffmpeg --with-faac --with-libssh --with-libvorbis --with-libvpx --with-openssl --with-opus --with-theora --with-webp --with-x265
```

Not only will this install all the dependencies like `webp`,
it will make sure you can pretty much throw anything at `ffmpeg`.

You'll probably have to do the same with `imagemagick` and/or `graphicsmagick`.

## Ruby

Lots of programs use ruby, so be sure to install it!

```bash
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
