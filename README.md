# Install Ruby on macOS

`install-ruby` is a script that reliably configures your Mac so you can install
Ruby gems (like Bundler, Jekyll, Rails), and switch between multiple versions of
Ruby.

It can be run multiple times on the same machine safely. It installs, upgrades,
or skips packages based on what is already installed on the machine.

Why
---
Installing Ruby and/or gems is a common source of confusion and frustration.
Search for `You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory`
or "[command not found](https://www.moncefbelyamani.com/troubleshooting-command-not-found-in-the-terminal/)"
in your favorite search engine, and you will see pages and pages of results.

To make matters worse, the vast majority of suggestions are bad advice and
incomplete. The reason for the error message above is because people are trying
to install gems using the version of Ruby that comes pre-installed by Apple.
That error message is there for a reason: you should not modify macOS system
files. A common suggestion is to bypass that security protection by using
`sudo`, which is [not safe](https://www.moncefbelyamani.com/why-you-should-never-use-sudo-to-install-ruby-gems/) and can cause issues down the line that are hard to
undo.

The recommended way of using Ruby on a Mac is to install a newer (the
macOS version is often outdated and is only updated during a major release),
separate version in a different folder than the one that comes by default on
macOS. The best and most flexible way to do that is with a Ruby manager. The
most popular ones are: RVM, rbenv, and chruby, and asdf. I have chosen `chruby` in this script. See below for my reasons. There are different ways to
install these tools, and they all require additional configuration in your [shell startup file](https://www.moncefbelyamani.com/which-shell-am-i-using-how-can-i-switch/), such as `.bash_profile` or `.zshrc`.

When attempting to install and configure a Ruby manager manually, it's easy to
miss or fumble a step due to human error or incomplete or outdated instructions. Since all of the steps are automatable, the best and most reliable way to set up Ruby on a Mac is to run a script like the one I've written. I test it regularly on my spare laptop where I delete the hard drive and install fresh versions of macOS. If you've already attempted to set up a development environment on your Mac, and you run into issues with my script, please read through the [Troubleshooting Errors](https://github.com/monfresh/install-ruby-on-macos/wiki/Troubleshooting-Errors) article. If that doesn't help, feel free to open an issue, and I will do my best to help you.

Read more in my [definitive guide to installing Ruby gems on a Mac](https://www.moncefbelyamani.com/the-definitive-guide-to-installing-ruby-gems-on-a-mac/).

Note that this script installs the bare minimum for a working Ruby development
environment. I also have another script called [laptop](https://github.com/monfresh/laptop)
that installs other essentials for Rails apps, Jekyll sites, or other web development, and it's customizable. I recommend the
`laptop` script for most people.

For more coding guides, scripts, and screencasts, subscribe to my [free weekly newsletter](https://www.moncefbelyamani.com/newsletter).

## Requirements

**I have not yet tested Macs with the M1 chip. The script should work under Rosetta 2. If you give it a try, please let me know if it worked.**

Supported operating systems:

* Big Sur (11.1)
* macOS Catalina (10.15)
* macOS Mojave (10.14)

Unsupported operating systems. Give it a shot, but I can't guarantee it will work.

* macOS High Sierra (10.13)
* macOS Sierra (10.12)
* OS X El Capitan (10.11)
* OS X Yosemite (10.10)
* OS X Mavericks (10.9)

Supported shells:

- bash
- fish
- zsh

What it sets up
---------------

* [Bundler] for managing Ruby gems
* [chruby] for managing [Ruby] versions (recommended over RVM and rbenv)
* [Homebrew] for managing operating system libraries (which also installs the prerequisite Apple command line tools)
* [ruby-install] for installing different versions of Ruby

[Bundler]: http://bundler.io/
[chruby]: https://github.com/postmodern/chruby
[Homebrew]: http://brew.sh/
[Ruby]: https://www.ruby-lang.org/en/
[ruby-install]: https://github.com/postmodern/ruby-install

## Why chruby and not RVM or rbenv?

It is the smallest, most reliable, and easiest to understand. I like that it does not do some of the things that other tools do:

* Does not hook `cd`.
* Does not install executable shims.
* Does not require Rubies to be installed into your home directory.
* Does not automatically switch Rubies by default.
* Does not require write-access to the Ruby directory in order to install gems.

Other folks who prefer `chruby`:

* <https://kgrz.io/programmers-guide-to-choosing-ruby-version-manager.html>
* <https://stevemarshall.com/journal/why-i-use-chruby/>
* <https://linhmtran168.github.io/blog/2014/02/27/moving-from-rbenv-to-chruby/>

Install
-------

**IMPORTANT:** Before you run the script, make sure you have the latest Apple software updates installed. Check by going to System Preferences, then Software Update. If you're on Catalina, this does not mean upgrading to Big Sur, just the latest Catalina updates.

**Also, please make sure to read everything on this page for caveats, troubleshooting tips, and to make sure the script worked.**

Begin by opening the `Terminal` or `iTerm` application on your Mac. The easiest
way to open an application in macOS is to search for it via [Spotlight]. The
default keyboard shortcut for invoking Spotlight is `command-Space`. Once
Spotlight is up, start typing the first few letters of the app you are looking
for, and once it appears, press `return` to launch it.

In your Terminal window, run the following commands:

```sh
cd ~
curl --remote-name https://raw.githubusercontent.com/monfresh/install-ruby-on-macos/master/install-ruby
/usr/bin/env bash install-ruby 2>&1 | tee ~/laptop.log
```

The [script](./install-ruby) itself is available in this repo for you to review
if you want to see what it does and how it works.

Note that the script will ask you to enter your macOS password at various
points. This is the same password that you use to log in to your Mac. The
prompt comes from Homebrew, because it needs permissions to write to the
`/usr/local` directory.

If you
have `rbenv` or `RVM` installed, the script will ask you to uninstall them first
and run the script again. Look at the script output for uninstallation
instructions.

**Once the script is done, quit and relaunch Terminal.**

[Spotlight]: https://support.apple.com/en-us/HT204014

Debugging
---------

Your last run of the script will be saved to a file called `laptop.log` in your home
folder. Read through it to see if you can debug the issue yourself, with the help of the [Troubleshooting Errors](https://github.com/monfresh/install-ruby-on-macos/wiki/Troubleshooting-Errors) Wiki article. If not,
copy the entire contents of `laptop.log` into a
[new GitHub Issue](https://github.com/monfresh/install-ruby-on-macos/issues/new) (or attach the whole log file to the issue) for me and I'll be glad to help you. If the script doesn't work for you, it's most likely a bug in my script, which I'd love to fix, so please don't hesitate to report any issues.

How to tell if the script worked
--------------------------------

If the last thing the script displayed was "All done!", then everything the script was meant to do worked.

To verify that the Ruby environment is properly configured, run one or more of these
commands:

```shell
ruby -v
```

This should show `ruby 3.0.0p0` or later. If not, try quitting and relaunching Terminal.

```shell
which ruby
```

This should point to the `.rubies` directory in your home folder. For example:

```
/Users/monfresh/.rubies/ruby-3.0.0/bin/ruby
```

## How to switch between Ruby versions and install different versions

By default, the script installs the latest version of Ruby. 

**On December 25, 2020, Ruby 3.0 was released, and since this script always installs the latest version by default, you will have Ruby 3.0 (in addition to any other versions you might have installed). I have not yet tested popular gems like Rails or Jekyll with Ruby 3.0, so I'm not sure if everything works. If something seems broken, please let me know, and in the meantime, you can install 2.7.2 if you don't already have it.**

To install an older version,
run `ruby-install` followed by `ruby-` and the desired version. For example:

```shell
ruby-install ruby-2.7.2
```

To switch to this newly-installed version, run `chruby` followed by the version. For example:

```shell
chruby 2.7.2
```

**If this doesn't work, try quitting and restarting your terminal.**

Another way to automatically switch between versions is to add a `.ruby-version` file in your Ruby project with the version number prefixed with `ruby-`, such as `ruby-2.7.2`. To test that this works:

1. `cd` into a folder outside of your project
2. Run `chruby 3.0.0` (or some other version that is not the one specified in your `.ruby-version`)
3. Verify that you are using 3.0.0 with `ruby -v`
4. `cd` into your project
5. Verify that you are using the specified version with `ruby -v`

Note that gems only get installed in a specific version of Ruby. If you installed jekyll in 3.0.0,
and then you install 2.7.2 for example, you'll have to install jekyll again in 2.7.2.

## How to install gems such as Rails or Jekyll

Once you run the script, and have verified that it worked, then you can safely install gems using the `gem install` command. For example:

```shell
gem install jekyll
```

You should then be able to use the gems right away:

```shell
jekyll -v
```
