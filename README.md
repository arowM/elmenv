# elmenv

Simple management of Elm versions, inspired by projects like
[elmenv](https://github.com/sstephenson/elmenv),
[nvm](https://github.com/creationix/nvm), and
[phpenv](https://github.com/phpenv/phpenv).

## How It Works

At a high level, elmenv intercepts Elm commands using shim
executables injected into your `PATH`, determines which Elm version
has been specified by your application, and passes your commands along
to the correct Elm installation.

### Understanding Shims

elmenv works by inserting a directory of _shims_ at the front of your
`PATH`:

    ~/.elmenv/shims:/usr/local/bin:/usr/bin:/bin

Through a process called _rehashing_, elmenv maintains shims in that
directory to match every Elm command across every installed version
of Elm—`elm-make`, `elm-reactor`, `elm-package`, `elm-reply`, `elm`,
and so on.

Shims are lightweight executables that simply pass your command along
to elmenv. So with elmenv installed, when you run, say, `elm`, your
operating system will do the following:

* Search your `PATH` for an executable file named `elm`
* Find the elmenv shim named `elm` at the beginning of your `PATH`
* Run the shim named `elm`, which in turn passes the command along to
  elmenv

### Choosing the Elm Version

When you execute a shim, elmenv determines which Elm version to use by
reading it from `.elm-version` file found by searching the directory of the
script you are executing and each of its parent directories until reaching
the root of your filesystem.

## Installation

This will get you going with the latest version of elmenv and make it
easy to fork and contribute any changes back upstream.

1. Check out elmenv into `~/.elmenv`.

    ~~~ sh
    $ git clone --recurse-submodules https://github.com/sonnym/elmenv.git ~/.elmenv
    ~~~

2. Add `~/.elmenv/bin` to your `$PATH` for access to the `elmenv`
   command-line utility.

    ~~~ sh
    $ echo 'export PATH="$HOME/.elmenv/bin:$PATH"' >> ~/.bash_profile
    ~~~

    **Ubuntu Desktop note**: Modify your `~/.bashrc` instead of `~/.bash_profile`.

    **Zsh note**: Modify your `~/.zshrc` file instead of `~/.bash_profile`.

3. Add `elmenv init` to your shell to enable shims and autocompletion.

    ~~~ sh
    $ echo 'eval "$(elmenv init -)"' >> ~/.bash_profile
    ~~~

    _Same as in previous step, use `~/.bashrc` on Ubuntu, or `~/.zshrc` for Zsh._

4. Restart your shell so that PATH changes take effect. (Opening a new
   terminal tab will usually do it.) Now check if elmenv was set up:

    ~~~ sh
    $ type elmenv
    #=> "elmenv is a function"
    ~~~

#### Upgrading

If you've installed elmenv manually using git, you can upgrade your
installation to the cutting-edge version at any time.

~~~ sh
$ cd ~/.elmenv
$ git pull
$ git submodule foreach $(git submodule update --init --recursive)
~~~