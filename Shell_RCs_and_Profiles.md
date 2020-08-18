# Shell RC and Profile Files
- Conventions Used
    - R+X = Read FILE if it exists, execute commands within file
    - \>\> = Fallback to next file, OR programmed to go next file in addition to previous file
    - \>  = Fallback to next file found, should stop at first found

---

# BASH
## /bin/bash
- https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Bash-Startup-Files
- Unequal effective vs real UID/GID, bash behaves very differently, likely ignoring startup files and env vars

### Interactive + Login, OR /bin/bash --login
- R+X /etc/profile >> ~/.bash_profile > ~/.bash_login > ~/.profile
- /bin/bash --noprofile disables reading profile files
- `exit` /bin/bash built-in command, R+X ~/.bash_logout
- ~/.bash_profile often sources ~/.bashrc `if [ -f ~/.bashrc ]; then . ~/.bashrc; fi`

### Interactive + Non-Login
- R+X ~/.bashrc
- /bin/bash --noroc disables reading rc files
- /bin/bash --rcfile FILE R+X only specified rc file

### Non-Interactive
- if shell env variable BASH_ENV has filename defined in value R+X filename, `if [ -n "$BASH_ENV" ]; then . "$BASH_ENV"; fi`
    - Does not use PATH variable to locate BASH_ENV=filename
    
### What is interactive?
- Can use a special character test, or check if PS1 env var is set
```
case "$-" in
*i*)	echo This shell is interactive ;;
*)	echo This shell is not interactive ;;
esac

# OR
if [ -z "$PS1" ]; then
        echo This shell is not interactive
else
        echo This shell is interactive
fi
```
    
## /bin/sh linked to /bin/bash
- Invoking bash as `sh`
- Enters POSIX mode AFTER R+X startup files

### Interactive + Login, OR /bin/sh --login
- R+X /etc/profile >> ~/.profile
- /bin/sh --noprofile disables reading profile files

### Interactive + Non-Login
- if shell env variable ENV has filename defined in value R+X filename, `if [ -n "$ENV" ]; then . "$ENV"; fi`
    - Does not use PATH variable to locate ENV=filename

### Non-Interactive
- No startup files are read

## /bin/bash POSIX Mode
- if shell env variable ENV has filename defined in value R+X filename, `if [ -n "$ENV" ]; then . "$ENV"; fi`
    - Does not use PATH variable to locate ENV=filename
- No other startup files are read

## /bin/bash invoked by rshd or sshd
- R+X ~/.bashrc
- --norc and --rcfile may also be used, but rshd/sshd rarely use/allow these variables to execute shell

## /bin/sh linked to /bin/bash, invoked by rshd or sshd
- No startup files are read

---

# ZSH
## /usr/bin/zsh
- http://zsh.sourceforge.net/Intro/intro_3.html
- http://zsh.sourceforge.net/Doc/Release/The-Z-Shell-Manual.html#The-Z-Shell-Manual
- Defaults to $HOME if $ZDOTDIR is not defined

### Config files for ZSH
- Each config file has a counterpart /etc/{config_file}, the /etc file is always R+X before the $ZDOTDIR file
- $ZDOTDIR/.zshenv
- $ZDOTDIR/.zprofile
- $ZDOTDIR/.zshrc
- $ZDOTDIR/.zlogin
- $ZDOTDIR/.zlogout

### Config Usage
- `.zshenv` always invoked, unless `-f` passed to `zsh`, sets command search path and env vars, should not produce output or depend on tty
- `.zshrc` is sourced in interactive shells. It should contain commands to set up aliases, functions, options, key bindings, etc.
- `.zlogin` is sourced in login shells. It should contain commands that should be executed only in login shells.
    - Is not the place for alias definitions, options, environment variable settings, etc..
    - As a general rule, it should not change the shell environment at all. Rather, it should be used to set the terminal type and run a series of external commands (fortune, msgs, etc). 
- `.zlogout` is sourced when login shells exit.
- `.zprofile` is similar to `.zlogin`, except that it is sourced before `.zshrc`.
    - Is meant as an alternative to `.zlogin` for ksh fans; the two are not intended to be used together.
