---
layout: post
title: How to Install Zsh and Oh My Zsh on Windows 10
categories: [windows, zsh, shell, syntax, highlighting, ohmyzsh, hyper, terminal]
tags: [windows, zsh, shell, syntax, highlighting, ohmyzsh, hyper, terminal]
fullview: false
comments: true
description: How to Install Zsh and Oh My Zsh on Windows 10. How to make work with command prompt on Windows 10 better with Windows Subsystem for Linux and Zsh. Install zsh syntax highlighting plugin.
---

In this article I will tell how to make work with command prompt on Windows 10 better with `Windows Subsystem for Linux`, `Hyper` terminal, `oh-my-zsh` framework for `zsh` and `zsh-syntax-highlighting` plugin.

For this article you need to have Windows 10 x64 Version 1607 Build 14393 or higher and installed [Node.js](https://nodejs.org/en/).

### Install Windows Subsystem for Linux

First, you need to install Windows Subsystem for Linux. Go to `Settings -> Update and Security -> For developers` and change `Sideload apps` setting to `Developer mode`

![DEVELOPER_MODE](https://i.imgur.com/pf6vpxN.png)

Next open command prompt and go to `OptionalFeatures.exe` and enable `Windows Subsystem for Linux (Beta)` then reboot your PC. After rebooting you need to open command prompt and use `bash` command. Then begin automatic downloading and installation of Linux Subsystem.

In the next time when you need to use bash shell open command prompt and use `bash` command.

In more detail, this part is described in [Install Guide on the Microsoft official website](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)

### Install Hyper Terminal

Go to official [hyper terminal website](https://hyper.is) and download latest version of terminal for Windows.

### Install cURL and Git

Go to bash terminal installed above and use following commands:

- Install cURL:

{% highlight text %}
sudo apt-get install curl
{% endhighlight %}

- Install Git:

{% highlight text %}
sudo apt-get install git
{% endhighlight %}

### Install Zsh

To install Zsh use following command:

{% highlight text %}
sudo apt-get install zsh
{% endhighlight %}

### Install Oh My Zsh

Official GitHub repository of oh-my-zsh project [https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

To install oh-my-zsh go to bash terminal and use following command:

{% highlight text %}
curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | bash
{% endhighlight %}

and wait until the installation is completed.

### Configure and Run Oh My Zsh

Now each time when you need to use bash shell and zsh you need go to command prompt, use `bash` command and then use `zsh` command.

Let's simplify it.

Open bash terminal and use `nano ~/.bashrc` to open `.bashrc` config file.

[What is the purpose of .bashrc and how does it work?](http://unix.stackexchange.com/questions/129143/what-is-the-purpose-of-bashrc-and-how-does-it-work)

In start of that file add following command:

{% highlight text %}
bash -c zsh
{% endhighlight %}

> Note: You should not edit anything under %localappdata%\lxss (Directory of Windows Subsystem for Linux in Windows Filesystem) from a Windows editor at all. In this case use only nano or vim editor.

My `.bashrc` file on [gist](https://gist.github.com/evdokimovm/67e4fcd938af98528aa108574626e522#file-bashrc-L5)

Note that in this time if you will try to use "default" command `chsh -s /bin/zsh` in bash shell terminal it will not run zsh as default bash shell environment.

Look at this comment of Ben Hillis [@benhillis](https://github.com/benhillis) [https://github.com/Microsoft/BashOnWindows/issues/1724#issuecomment-282420193](https://github.com/Microsoft/BashOnWindows/issues/1724#issuecomment-282420193)

Now each time when you will use `bash` in command prompt zsh terminal will start automatically instead of bash shell.

### Configure and Run Hyper Terminal

After you installed Hyper Terminal open `%USERPROFILE%/.hyper.js` config file and replace line:

{% highlight text %}
shell: '',
{% endhighlight %}

with

{% highlight text %}
shell: 'C:\\Windows\\System32\\bash.exe',
{% endhighlight %}

Now each time when you will open hyper terminal it's will be use `zsh` as default shell environment.

Without installed themes for hyper this will looks like this:

![DEFAULT_HYPER](https://i.imgur.com/MLFyvYR.png)

Themes and other plugins for Hyper Terminal you can find here [https://github.com/bnb/awesome-hyper](https://github.com/bnb/awesome-hyper).

I use [hyper-material-theme](https://github.com/equinusocio/hyper-material-theme) theme by Mattia Astorino [@equinusocio](https://github.com/equinusocio).

![MATERIAL_HYPER](https://i.imgur.com/pD0cPJn.png)

### Change Oh My Zsh Theme

List of Oh My Zsh themes you can find here [https://github.com/robbyrussell/oh-my-zsh/wiki/Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

By default oh-my-zsh use `robbyrussell` (like on screen above). If you want to change theme open `~/.zshrc` config file by use following command in terminal:

{% highlight text %}
nano ~/.zshrc
{% endhighlight %}

and change default "robbyrussell" value of `ZSH_THEME` variable to another theme name that you need to enable.

> Known Issue: Hot key `Ctrl + X` in Hyper Terminal Does not Work. Instead of this use `Shift + Ctrl + X`. Link to Related GitHub Issue [https://github.com/zeit/hyper/issues/1069](https://github.com/zeit/hyper/issues/1069)

> Known Issues for Windows Subsystem for Linux here [https://github.com/Microsoft/BashOnWindows](https://github.com/Microsoft/BashOnWindows)

### Install zsh-syntax-highlighting plugin

Official GitHub repository of project [https://github.com/zsh-users/zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

This plugin provides syntax highlighting in terminal like in `Fish` shell.

Open terminal and download `zsh-syntax-highlighting` plugin to `.oh-my-zsh` plugins directory using git:

{% highlight text %}
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
{% endhighlight %}

If `$ZSH_CUSTOM` variable exist and contains value (full path to `/.oh-my-zsh/custom` in this case) use its value, otherwise use `~/.oh-my-zsh/custom`.

About `:-` (colon dash) you can read in this topic on stackoverflow: [Usage of :- (colon dash) in bash](https://stackoverflow.com/questions/10390406/usage-of-colon-dash-in-bash).

Next to activate this plugin open `~/.zshrc` config file using `nano ~/.zshrc` and find the following text:

{% highlight text %}
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)
{% endhighlight %}

Here you need to add `zsh-syntax-highlighting` to `plugins` list. For that just replace 

{% highlight text %}
plugins=(git)
{% endhighlight %}

with

{% highlight text %}
plugins=(git zsh-syntax-highlighting)
{% endhighlight %}

and save changes.

Finally you need to take changes into account. For that use follow command:

{% highlight text %}
source ~/.zshrc
{% endhighlight %}

And now after restart hyper terminal you can see result. Screen:

![ZSH_SHELL_HIGHLIGHTING](https://i.imgur.com/U0wzrbk.png)
