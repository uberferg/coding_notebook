# Learn Python the Hard Way Exercise Repo

This is a repository to keep track of notes and code as I work
through Learn Python the Hard Way by Zed Shaw.

## Python Installation/Setup
### ArchLinux
* Python 2 and 3 should both be installed by default. Default is Python3
  but this book uses Python 2, so we will use virtualenv to sandbox
  our exercises as follows:
  * `$ pacman -S virtualenv`
  * `$ cd my_lpthw_directory`
  * `$ virtualenv --python=python2 my_py2_venv
  * `$ source my_py2_venv/bin/activate
* This last line activates the virtualenv, switching the default
  python executable to python 2 (probably 2.7). Rerun this line
  each time you go back to working on this project.
* You may encounter problems creating the virtualenv. Do not use
  sudo to override these problems. Check your fstab and make sure you
  are mounting your working drive with correct umask permissions.
