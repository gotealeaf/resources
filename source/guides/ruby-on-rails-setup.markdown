---
layout: page
title: "ruby-on-rails-setup"
date: 2014-02-06 01:59
comments: true
sharing: true
footer: true
---

(Updated Feb 4 2014)

# Step 1. Upgrade Your System to OS X Mavericks

If you have a not-so-ancient Mac model (generally, 2008 or newer), you should [upgrade your system to OS X Mavericks](https://www.apple.com/osx/how-to-upgrade/). It comes with the latest application features, energy efficiency and performance optimization from Apple, and it's completely free. Do this first before the rest of the steps in this guide.

# Step 2. Install XCode Command Line Developer Tools

This used to involve a much lengthy process, but thanks to a newly released package by Apple, all you have to do is to run the following command in your terminal. 

```bash
xcode-select --install
```

# Step 3. Install Homebrew

Type the following command in your terminal:

```bash
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

# Step 4. Install Ruby with rbenv

Your Mac already ships with Ruby (that's how we can use it to install homebew in the last step). However, it's still a good idea to use a Ruby Version Manager like `rbenv` to install Ruby because 1) the system Ruby is likely outdated and 2) you may need to work on multiple projects on different versions of Ruby. A Ruby Version Manager makes organizing and switching between Rubies a breeze. 

First we can use homebrew to install rbenv:

```bash
brew update
brew install rbenv ruby-build rbenv-gem-rehash
```

`rbenv` by itself only manages switching ruby versions. `ruby-build` and `rbenv-gem-rehash` are both `rbenv` extensions. `ruby-build` allows you to install rubies with `rbenv` and `rbenv-gem-rehash` automatically installs new shims for you whenever a new Ruby gem is installed so you don't have to rehash yourself. 

Open up your the `~/.bashrc' file, and put this line on the bottom:

```
eval "$(rbenv init -)" 
```

Now you are ready to install Ruby with `rbenv`. At the time of this writing, the latest stable Ruby version is 2.1.0, so let's install that.

```bash
rbenv install 2.1.0-p0
```

# Step 5. Install git and set up Github account

Git is a powerful version control system and is very popular in the Ruby community. If you followed this guide, you should already have git installed as part of the XCode Command Line Developer Tools. You may also want to install git separately with homebrew to make it easier to upgrade to latest releases.

```bash
brew install git
```

Now tell git your name and email that it will use for your future commits.

```bash
git config --global user.name "Your Name Here"
git config --global user.email "your_email@example.com"
```

Github is the leading platform for source code hosting and collaboration. If you don't have an account yet, go ahead and sign up for one at https://github.com. Make sure you sign up with the same email address from the step above.

For easier authentication with Github when you push or pull code, follow [this guide](https://help.github.com/articles/generating-ssh-keys) to set up ssh keys for your Mac.

# Step 6. Set up Sublime Text 2 as code editor

If you already have an editor of choice, such as Vim or Emacs, you are all set and can skip this step! If you are not familiar with code editors, Sublime Text 2 is an excellent choice and you can download it [here](http://www.sublimetext.com/2). 

After you install it, run the following command so you can easily launch the editor from your terminal directly:

```bash
ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl
```
The convention for Ruby programs is to use two spaces as indentation, so follow `Sublime Text 2 => Preferences => Settings - User` and add these lines.

```
{
"tab_size": 2,
"translate_tabs_to_spaces": true
}
```

# Step 7. Create a New Rails Application

If you don't have a directory to hold all your development projects yet, you can create that directory like below: 

```bash
mkdir ~/projects
```

Now you can create a Rails project in that directory:

```bash
cd ~/projects
gem install rails
rails new name_of_your_new_app
```
Wait until the the last step finishes, and you just created your first Rails project! You can verify that you set up Rails by first starting the server

```bash
cd ~/projects/name_of_your_new_app
rails s
```
Now open up your browser and type in the address bar `http://localhost:3000` and if you see a welcome page, your app is running locally. 


# Optional Step 1. Use iTerm 2, zsh and oh-my-zsh to set up an awesome looking and easily customizable terminal

Download and Install [iTerm 2](http://www.iterm2.com/#/section/home). It comes with more features and easier to customize than the built in Terminal. 

Optionally, you can further customize iTerm 2 to your liking! Here are some of my preferences.

- Under "Genereal", check "Copy to clipboard on selection". 
- Under "Profile" => "Colors", click on "Load Presets", then choose "Dark Background"; 
- Under "Profile" => "Text", change the font to one that you enjoy looking at. My favorite is 20pt [Anonymous Pro](http://www.marksimonson.com/fonts/view/anonymous-pro) with Anti-aliased. You have to download it first and install it into your Mac's font book before you can use it. 
- Under "Keys", define a hotkey to hide/show the terminal window. This is much faster than having to Command+Tab through opened windows and find iTerm 2. 

Zsh is an alternative shell to the default bash shell that comes with Mac. It adds nice features such as smart tab completions, but what really sets it apart is its scriptability. Together with oh-my-zsh, an open source zsh configuration management framework, it becomes really easy to customize both the look and functionality of your terminal. 

Your Mac should already come with zsh. To use zsh, go to iTerm 2 => Preferences => Profiles => General and in the "Command" section, select "Command", and type `/bin/zsh` in the box after it.

Now you can install oh-my-zsh with

```bash
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

Now you can customize the `~/.zshrc` file. 

- If you have your settings in `~/.bash_profile`, you may want to copy them over to `/.zshrc`. 
- Find the `plugins=(git)` line and add [more plugins](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins). Here is the plugins I am using:

```
plugins=(git bundler brew osx vagrant wd gem)
```

The bunlder plugin is especially useful when you work on Rails applications as it saves you from typing those "bundle exec"'s in front of commands like `rails`, `rake`, `rspec`, etc! 

If you do not like the default theme, you can pick from one of the many themes that come with oh-my-zsh. You can see the list of themes [here](https://github.com/robbyrussell/oh-my-zsh/wiki/themes) and [here](http://zshthem.es/all/). 

If you feel really adventurous, you can even build your own theme! Take a look at [how themes are implemented](https://github.com/robbyrussell/oh-my-zsh/tree/master/themes), and copy/tweak/build one exactly to your taste!

# Optional step 2. Install Postgresql as a production quality database

By default, Rails uses sqlite3 as the default development database. It's a nice database but probably not one that you want to use in production. Postgresql is a solid, production quality relational database and works well with Rails. It's generally a good idea to set up your local database to match the database on the production environment. 

The easiest way to use Postgresql on Mac is to download and install the [Postgres.app](http://postgresapp.com/)

With Postgresql running, add `gem 'pg'` to the Gemfile in your rails project and run `bundle install` to install the Postgresql Ruby driver.
