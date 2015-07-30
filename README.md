![](https://raw.githubusercontent.com/b4b4r07/screenshots/master/enhancd/logo.gif)

:rocket: enhancd <sup>v2</sup> is ...

A next-generation `cd` command with an interactive filter :sparkles:

## :memo: Description

`cd` command is one of the frequently used commands. 

Nevertheless, it is not so easy to handle unfortunately. A directory path given as an argument to `cd` command must be a valid path that exists and is able to resolve. In other words, you cannot pass a partial path such as "dir" (you are in /home/lisa, dir is /home/lisa/work/dir) to `cd` command.

The new cd command called "enhancd" enhanced the flexibility and usability for a user. enhancd will memorize all directories visited by a user and use it for the pathname resolution. If the log of enhancd have more than one directory path with the same name, enhancd will pass the candidate directories list to the filter within the ENHANCD_FILTER variable in order to narrow it down to one.

Thanks to that mechanism, the user can intuitively and easily change the directory where you want to go to.

:warning: [`cdinterface`](https://github.com/b4b4r07/cdinterface) is deprecated and was merged into this enhancd. 

***DEMO:***

![](https://raw.githubusercontent.com/b4b4r07/screenshots/master/enhancd/demo.gif)

## :trollface: Features

- Go to the visited directory in the past
- Easy to filter, using your favorite filter
- Work on Bash, Zsh and Fish :fish:

## :heartbeat: Requirements

- An interactive filter
	- [percol](https://github.com/mooz/percol)
	- [peco](https://github.com/peco/peco)
	- [fzf](https://github.com/junegunn/fzf)
	- [gof](https://github.com/mattn/gof)
	- ...

	Choose any one from among these.

## :mag: Usage

Under Zsh or Bourne shells such as sh and bash, you would use `enhancd.sh`. Under [fish shell](http://fishshell.com), `enhancd.fish`.

```console
$ source ./enhancd.sh
```

Because enhancd functions must be executed in the context of the current shell, you should run something like above command.

The basic usage of the `cd` command that has been implemented by `enhancd` is the same as the normal builtin `cd` command.

```console
$ cd [-|.] <directroy>
```

If no arguments are given, enhancd `cd` command will display a list of the directory you've visited once, encourage you to filter the directory that you want to move.

```console
$ cd
  ...
  /home/lisa/src/github.com/b4b4r07/enhancd/zsh
  /home/lisa/src/github.com/b4b4r07/gotcha
  /home/lisa/src/github.com/b4b4r07/blog/public
  /home/lisa/src/github.com/b4b4r07/blog
  /home/lisa/src/github.com/b4b4r07/link_test
  /home/lisa/src/github.com/b4b4r07
  /home/lisa/Dropbox/etc/dotfiles
  /home/lisa/src/github.com/b4b4r07/enhancd
> /home/lisa
  247/247
> _
```

The ENHANCD_FILTER variable is specified as a list of one or more visual filter command such as [this](#requirements) separated by colon (:) characters.

It is likely the only environment variable you'll need to set when starting enhancd.

```console
$ ENHANCD_FILTER=peco; export ENHANCD_FILTER
```

Since the `$ENHANCD_FILTER` variable can be a list, enhancd will use `$ENHANCD_FILTER` to mean the first element unless otherwise specified.

```console
$ ENHANCD_FILTER=fzf:peco:gof
$ export ENHANCD_FILTER
```

### Options

- Hyphen (`-`)

	When enhancd takes a hyphen (`-`) string as an argument, it searchs from the last 10 directory items in the log. With it, you can search easily the directory you used last.
	
	```console
	$ cd -
	...
	  /home/lisa/Dropbox/etc/dotfiles
	  /home/lisa/Dropbox
	  /home/lisa/src/github.com
	  /home/lisa/src/github.com/b4b4r07/cli
	  /home
	  /home/lisa/src
	  /home/lisa/src/github.com/b4b4r07/enhancd
	  /home/lisa/src/github.com/b4b4r07/gotcha
	  /home/lisa/src/github.com/b4b4r07
	> /home/lisa/src/github.com/b4b4r07/portfolio
	  10/10
	> _	
	```
	
	Then, since the current directory will be delete from the candidate, you just press Enter key to return to the previous directory after type `cd -` ($PWD is /home/lisa, $OLDPWD is /home/lisa/src/github.com/b4b4r07/portfolio).

- Dot (`.`) :warning: *Experimental*

	When enhancd takes a dot (`.`) string as an argument, it behaves like a [zsh-bd](https://github.com/Tarrasch/zsh-bd) plugin. In short, you can jump back to a specific directory, without doing `cd ../../..`.
	
	For example, when you are in /home/lisa/src/github.com/b4b4r07/enhancd, type `cd .` in your terminal:
	
	```console
	$ cd .
	...
	  enhancd
	  b4b4r07
	  github.com
	  src
	  lisa
	> home
	  6/6
	> _
	```

## :package: Installation

![](https://raw.githubusercontent.com/b4b4r07/screenshots/master/enhancd/installation.png)

Give me a trial!

```console
$ curl -L git.io/enhancd | sh
```

Paste that at a Terminal prompt.

To specify installation location for enhancd:

```console
$ curl -L git.io/enhancd | PREFIX=/path/to/dir sh
```

PREFIX defaults to `~/.enhancd`.

### What's inside?

1. Grab enhancd.sh from github.com by using `git`, `curl` or `wget`
2. Add `source /path/to/enhancd.sh` to config file whose you use as the login shell

The above and below are almost the same.

```console
$ git clone https://github.com/b4b4r07/enhancd ~/.enhancd
$ echo "source ~/.enhancd/bash/enhancd.bash" >> ~/.bashrc
```

***NOTE:***

If you want to use older versions of enhancd (enhancd <sup>v1</sup>: [dca011aa34](https://github.com/b4b4r07/enhancd/tree/dca011aa34957bf88ea6edbdf7c84b8a5b0157b5)), set BRANCH as v1 and run this command:

```console
$ curl -L git.io/enhancd | BRANCH=v1 sh
```


### Uninstallation

Are you sure? To uninstall enhancd, paste the command below in a terminal prompt.

```console
$ rm -r ~/.enhancd
```

## :wrench: Configuration

### ENHANCD_DIR

The ENHANCD_DIR variable is a base directory path. It defaults to `~/.enhancd`.

### ENHANCD_FILTER

1. What is ENHANCD_FILTER?

	The ENHANCD_FILTER is an environment variable. It looks exactly like the PATH variable containing with many different filters such as [peco](https://github.com/peco/peco) concatenated using ':'.

2. How to set the ENHANCD_FILTER variable?      

	Setting the ENHANCD_FILTER variable is exactly like setting the PATH variable. For example:

	```console
	$ export ENHANCD_FILTER="/usr/local/bin/peco:fzf:non-existing-filter"
	```

	This above command will hold good till the session is closed. In order to make this change permanent, we need to put this command in the appropriate profile file. The ENHANCD_FILTER command in this example is set with 3 components: `/usr/local/bin/peco` followed by `fzf` and the `not-existing-filter`.

	enhancd narrows the ENHANCD_FILTER variable down to one. However, the command does not exist can not be the candidate.
	
	Let us try to test this ENHANCD_FILTER variable.

	```console
	$ cd
	```

	If cd command does not return error, the settings of ENHANCD_FILTER is success.
	
3. How to find the value of the ENHANCD_FILTER variable?

	```console
	$ echo $ENHANCD_FILTER
	/usr/local/bin/peco:fzf:non-existing-filter
	```

## :books: References

The "visual filter" is what is called "Interactive Grep Tool" according to [percol](https://github.com/mooz/percol) that is a pioneer in interactive selection to the traditional pipe concept on UNIX. 

- **percol** :point_right: [percol adds flavor of interactive selection to the traditional pipe concept on UNIX](https://github.com/mooz/percol)
- **peco** :point_right: [Simplistic interactive filtering tool](https://github.com/peco/peco)
- **hf** :point_right: [hf is a command line utility to quickly find files and execute a command](https://github.com/hugows/hf)
- **fzf** :point_right: [fzf is a blazing fast command-line fuzzy finder written in Go](https://github.com/junegunn/fzf)
- **gof** :point_right: [gof - Go Fuzzy](https://github.com/mattn/gof)
- **selecta** :point_right: [Selecta is a fuzzy text selector for files and anything else you need to select](https://github.com/garybernhardt/selecta/)
- **pick** :point_right: [Pick is "just like Selecta, but faster"](https://robots.thoughtbot.com/announcing-pick)
- **icepick** :point_right: [icepick is a reimplementation of Selecta in Rust](https://github.com/felipesere/icepick)
- **sentaku** :point_right: [Utility to make sentaku (selection, 選択(sentaku)) window with shell command](https://github.com/rcmdnk/sentaku)

## :ticket: License

[MIT](https://raw.githubusercontent.com/b4b4r07/dotfiles/master/doc/LICENSE-MIT.txt) :copyright: BABAROT (a.k.a. b4b4r07)
