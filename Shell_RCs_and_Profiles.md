# Shell RC and Profile Files
- Conventions Used
    - R+X = Read FILE if it exists, execute commands within file
    - >> = Fallback to next file, OR programmed to go next file in addition to previous file
    - >  = Fallback to next file found, should stop at first found

## /bin/bash
- https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Bash-Startup-Files

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
    
## /bin/sh linked to /bin/bash
- Invoking bash as `sh`
- Enters POSIX mode AFTER R+X startup files

### Interactive + Login, OR /bin/sh --login
- R+X /etc/profile >> ~/.profile
- /bin/sh --noprofile disables reading profile files

### Interactive + Non-Login
- if shell env variable ENV has filename defined in value R+X filename, `if [ -n "$ENV" ]; then . "$ENV"; fi`

### Non-Interactive
- No startup files are read
