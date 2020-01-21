---
title: Setup Jekyll in VisualStudio online
excerpt: Setup Jekyll in a visualstudio online environment to develop on github pages blog.
categories:
- Tips
header:
  overlay_image: /assets/images/posts/2020-01-16-windows10.jpg
---

This post is written on [visual studio online][1] and is focused on how to prepare the environment to run [Jekyll][6].

## Visual Studio Online

Not to be confused with the original visual studio online, which was first renamed to **Visual Studio Team Services** before finally becomming **Azure DEVOPS**. [Visual Studio Online][1] was first announced on [November 4][2] and it is still in preview mode. From the [announcement][2], [Nik Molnar][3] provides this description.

> Visual Studio Online provides managed, on-demand development environments that can be used for long-term projects, to quickly prototype a new feature, or for short-term tasks like reviewing pull requests. You can work with environments from anywhere using either Visual Studio Code, Visual Studio IDE (in private preview), or the included browser-based editor.

In other words, you get can create an instance on demand on Azure that has most of the popular tools installed and you can develop on it either by using the browser or by using [Visual Studio Code after][4] installing the [Visual Studio Online][5] extension.

In the screenshots below you can see me authoring this post while being able to have a preview like normal local development.

| Access from web browser | Access from Visual Studio Code |
| ----------------------- | ------------------------------ |
| ![Access from web browser](/assets/images/posts/2020-01-21-visual-studio-online-web.png "Access from Visual Studio Code") | ![Access from web browser](/assets/images/posts/2020-01-21-visual-studio-online-vscode.png "Access from Visual Studio Code") |

Two things to notice:
1. When on the browser the picture don't render in the preview. This is because the pictures are referenced by a local path, which [Jekyll][6] will convert to actual uris but the preview won't.
1. The configured them is used when using Visual Studio Code to access the environment.

The instance is a [Debian GNU/Linux][7] 9 (stretch). I believe the easiest way to describe the instance is to quote os information.

```text
vsonline:~/workspace$ cat /etc/*-release
PRETTY_NAME="Debian GNU/Linux 9 (stretch)"
NAME="Debian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
VERSION_CODENAME=stretch
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

## Upgrading Ruby for Jekyll with rbenv

There are a lot of packages installed but for Jekyll we need to focus on the Ruby ones.

```text
vsonline:~/workspace$ apt list ruby2*
Listing... Done
ruby2.3/oldstable 2.3.3-1+deb9u7 amd64
ruby2.3-dev/oldstable 2.3.3-1+deb9u7 amd64
ruby2.3-doc/oldstable 2.3.3-1+deb9u7 all
ruby2.3-tcltk/oldstable 2.3.3-1+deb9u7 amd64
ruby2.5/stable 2.5.5-3+deb10u1 amd64
ruby2.5-dev/stable 2.5.5-3+deb10u1 amd64
ruby2.5-doc/stable 2.5.5-3+deb10u1 all
```

[Jekyll][6] requires [Ruby][8] 2.4. I'm not experienced on Debian or Linux nor Ruby but to the best of my understanding, to install another version of [Ruby][8] we need to first install [rbenv][9] which becomes available by installing the `ruby-build` package.

From the terminal execute as root the following 

```
sudo apt install ruby-build -y
```

Once it finishes check the current version of [Ruby][8]

```
vsonline:~/workspace$ ruby -v
ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]
```

Although I don't undestand the way, it seems that the installation of the `ruby-build` package activates the [Ruby][8] 2.3 version.

```text
vsonline:~/workspace$ apt list ruby2*
Listing... Done
ruby2.3/oldstable,now 2.3.3-1+deb9u7 amd64 [installed,automatic]
ruby2.3-dev/oldstable 2.3.3-1+deb9u7 amd64
ruby2.3-doc/oldstable 2.3.3-1+deb9u7 all
ruby2.3-tcltk/oldstable 2.3.3-1+deb9u7 amd64
ruby2.5/stable 2.5.5-3+deb10u1 amd64
ruby2.5-dev/stable 2.5.5-3+deb10u1 amd64
ruby2.5-doc/stable 2.5.5-3+deb10u1 all
```

We will use the `rbenv` tool to install the `2.4` version. Notice that we are not installing as root. When I had done so, the `2.4` version was only available for root.

```text
vsonline:~/workspace$ rbenv install 2.4.0
Downloading ruby-2.4.0.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.4/ruby-2.4.0.tar.bz2
Installing ruby-2.4.0...
Installed ruby-2.4.0 to /home/vsonline/.rbenv/versions/2.4.0
```

When checking which versions are available, we see that `2.4.0` is available but not selected. The can be also verified by probing ruby for its executing version.

```text
vsonline:~/workspace$ rbenv versions
* system (set by /home/vsonline/.rbenv/version)
  2.4.0
```
```text
vsonline:~/workspace$ ruby -v
ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]  
```

Apparently the installation of `rbenv` is not "complete" and we need to do the following before using it:

- Add the `rbenv` shims path to the `PATH` environmental variable. 
- Initialaze `rbenv` with the `rbenv init` command.

We can add these steps in the `~/.bash_profile` to make sure it always executes when starting `bash`. The following block appends or creates the `~/.bash_profile`.

```text
vsonline:~/workspace$ echo 'export PATH="$HOME/.rbenv/shims:$PATH"' >> ~/.bash_profile
vsonline:~/workspace$ echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
```

To confirm execute 

```text
vsonline:~/workspace$ cat ~/.bash_profile
export PATH="$HOME/.rbenv/shims:$PATH"
if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi
```

The commands have not been executed yet. To do so we can start a new instance of `bash` or execute the `~/.bash_profile` using the `source` command.

```text
vsonline:~/workspace$ source ~/.bash_profile
```

To confirm execute 

```text
vsonline:~/workspace$ echo $PATH
/home/vsonline/.rbenv/shims:/home/vsonline/.rbenv/shims:/home/vsonline/.rbenv/shims:/home/vsonline/.dotnet:/home/vsonline/.dotnet:/home/vsonline/.vscode-remote/bin/26076a4de974ead31f97692a0d32f90d735645c0/bin:/home/vsonline/.dotnet:/home/vsonline/.dotnet:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/opt/oryx:/opt/nodejs/lts/bin:/opt/python/latest/bin:/opt/yarn/stable/bin:/home/vsonline/.local/bin:/home/vsonline/.npm-global/bin:/opt/oryx:/opt/nodejs/lts/bin:/opt/python/latest/bin:/opt/yarn/stable/bin:/home/vsonline/.local/bin:/home/vsonline/.npm-global/bin:/opt/oryx:/opt/nodejs/lts/bin:/opt/python/latest/bin:/opt/yarn/stable/bin:/home/vsonline/.local/bin:/home/vsonline/.npm-global/bin:/opt/oryx:/opt/nodejs/lts/bin:/opt/python/latest/bin:/opt/yarn/stable/bin:/home/vsonline/.local/bin:/home/vsonline/.npm-global/bin
```

From [Up and Running with rbenv][10] by [Ian Oxley][11]
> The job of the shims is to grab the directory for your desired version of Ruby and stick it at the beginning of your $PATH and then execute the corresponding binaries. rbenv requires you to install each version of Ruby in the ~/. rbenv/versions directory

We can use the `rbenv which` command to confirm which version is used. For example before changing the version

```text
vsonline:~/workspace$ rbenv which ruby
/usr/bin/ruby
vsonline:~/workspace$ rbenv which gem
/usr/bin/gem
vsonline:~/workspace$ ruby -v
ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]
vsonline:~/workspace$ gem -v
2.5.2.1
```

To change the version of [ruby][8] we need to execute one of the following:
- `rbenv global 2.4.0`
- `rbenv local 2.4.0`
- `rbenv shell 2.4.0`

There is more information about `rbenv` on it's [github repository][9]. Because we need to execute some more installation and configuration, I prefer to set globally the version to `2.4.0`. Execute the following and then confirm that we are using the right version.

```text
vsonline:~/workspace$ rbenv global 2.4.0
vsonline:~/workspace$ rbenv which ruby
/home/vsonline/.rbenv/versions/2.4.0/bin/ruby
vsonline:~/workspace$ ruby -v
ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-linux]
vsonline:~/workspace$ gem -v
2.6.8
```

Before installing Jekyll we need to initialize the gem paths by creating the `GEM_HOME` environment variable and adding it's `bin` to the `PATH` variable.

```text
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
```

Then execute the `~/.bashrc` or restart `bash`.

```text
vsonline:~/workspace$ source ~/.bashrc
```

## Installing Jekyll and Bundler

If we would try to install the [Jekyll][6] and [Bundler][12] gems we would receive the following error:

```text
ERROR:  Error installing jekyll:
        jekyll requires RubyGems version >= 2.7.0. Try 'gem update --system' to update RubyGems itself.
```

So we need to fist update `gem` and then install the [Jekyll][6] and [Bundler][12] gems.

```text
vsonline:~/workspace$ gem update --system
```
```text
vsonline:~/workspace$ gem install jekyll bundler
```











[1]: https://online.visualstudio.com/environments/
[2]: https://devblogs.microsoft.com/visualstudio/announcing-visual-studio-online-public-preview/?WT.mc_id=-blog-scottha
[3]: https://devblogs.microsoft.com/visualstudio/author/nimolnarmicrosoft-com/
[4]: https://code.visualstudio.com/?wt.mc_id=DX_841432
[5]: https://marketplace.visualstudio.com/items?itemName=ms-vsonline.vsonline
[6]: https://jekyllrb.com/
[7]: https://www.debian.org/
[8]: https://www.ruby-lang.org/en/
[9]: https://github.com/rbenv/rbenv
[10]: https://www.sitepoint.com/up-and-running-with-rbenv/
[11]: https://www.sitepoint.com/author/ioxley/
[12]: https://bundler.io/