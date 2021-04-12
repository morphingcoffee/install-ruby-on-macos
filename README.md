# Install Ruby on macOS

`install-ruby` is a script that reliably configures your Mac so you can install
Ruby gems (like Bundler, Jekyll, Rails), and switch between multiple versions of Ruby.

It can be run multiple times on the same machine safely. It installs, upgrades,
or skips packages based on what is already installed on the machine.

## Important
If you came here from my answer on Stack Overflow, you most likely want my [laptop](https://github.com/monfresh/laptop) script instead, unless you know for sure that all you need is Ruby.

**After running either this script or the laptop script, make sure to restart your terminal for the changes to take effect.**

## More goodies

- Join the 1200+ people on my list who are becoming confident coders through my [free weekly coding guides](https://www.moncefbelyamani.com/newsletter) and exclusive tutorials and courses.
- Check out my customizable [laptop](https://github.com/monfresh/laptop) script that installs more essential development tools, as well as the Jekyll and Rails gems. I recommend the `laptop` script for most people.

## What's supported

Supported chips:

- Apple Silicon M1
- Intel

Supported operating systems:

* Big Sur
* Catalina
* Mojave

Unsupported operating systems. Give it a shot, but I can't guarantee it will work.

* macOS High Sierra (10.13.x)
* macOS Sierra (10.12.x)
* OS X El Capitan (10.11.x)
* OS X Yosemite (10.10.x)
* OS X Mavericks (10.9.x)

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

Install
-------

**IMPORTANT! CHECK ALL OF THE ITEMS BELOW BEFORE AND AFTER RUNNING THE SCRIPT!** 

### Check prerequisites
Make sure your computer meets all [prerequisites](https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/#prerequisites) first.

### If you are on an M1 Mac, do not use Rosetta
Homebrew works natively on M1 Macs. Make sure to open the default Terminal application, or iTerm, or whatever app you use. Make sure it is not in Rosetta mode. Read my guide on [installing Ruby on Apple Silicon](https://www.moncefbelyamani.com/how-to-install-homebrew-and-ruby-on-a-mac-with-the-m1-apple-silicon-chip/) for more details.

### Quit and relaunch Terminal after running my script
I mention this several times in this README, as well as when the script finishes successfully, but I'll say it again. For the changes to take effect, you have to "refresh" your terminal. The best way is to quit and relaunch it.

### Now on to the installation

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

Note that the script might ask you to enter your macOS password at various
points. This is the same password that you use to log in to your Mac. The
prompt comes from Homebrew, because it needs permissions to write to the
`/usr/local` (or `/opt/homebrew` on M1 Macs) directory.

**Once the script is done, quit and relaunch Terminal.**

[Spotlight]: https://support.apple.com/en-us/HT204014

Debugging
---------

Your last run of the script will be saved to a file called `laptop.log` in your home
folder. Read through it to see if you can debug the issue yourself, with the help of the [Troubleshooting Errors](https://github.com/monfresh/install-ruby-on-macos/wiki/Troubleshooting-Errors) Wiki article. If not,
copy the entire contents of `laptop.log` into a
[new GitHub Issue](https://github.com/monfresh/install-ruby-on-macos/issues/new) (or attach the whole log file to the issue) for me and I'll be glad to help you.

How to tell if the script worked
--------------------------------

If the last thing the script displayed was "All done!", then everything the script was meant to do worked. **Now make sure you quit and restart your terminal.**

To verify that the Ruby environment is properly configured, run one or more of these
commands:

```shell
ruby -v
```

This should show `ruby 2.7.2` or `ruby 3.0.0`. **If not, try quitting and relaunching Terminal.** Then try switching manually to 2.7.2:

```shell
chruby 2.7.2
```

and check the version to double check:

```shell
ruby -v
``` 

Then check where Ruby is installed:

```shell
which ruby
```

This should point to the `.rubies` directory in your home folder. For example:

```
/Users/monfresh/.rubies/ruby-2.7.2/bin/ruby
```

## How to switch between Ruby versions and install different versions

By default, the script installs Ruby 2.7.2. If you run it again, it will also install Ruby 3.0.0. For now, if you're not an experienced Rubyist, I recommend using Ruby 2.7.2 because some gems like Jekyll aren't yet fully compatible with Ruby 3.0.0. 

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
