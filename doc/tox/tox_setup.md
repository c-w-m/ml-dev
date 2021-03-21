# [tox](https://tox.readthedocs.io/en/latest/) Setup

The following steps were used to setup an Ubuntu 16.04 development system.  

## PyPy and Python Versions

### pypy
__Don’t__ use the python-scrapy package provided by Ubuntu, they are typically 
too old and slow to catch up with the latest Scrapy.

To install Scrapy on Ubuntu (or Ubuntu-based) systems, you need to install 
these dependencies:
```shell
sudo apt-get install python3 python3-dev python3-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev
```

Installing pypy package on Ubuntu 16.04 (Xenial Xerus) is as easy as running 
the following command on terminal:
```shell
$ sudo add-apt-repository ppa:pypy/ppa
$ sudo apt-get update
$ sudo apt-get install pypy
$ pypy
Python 2.7.18 (7.3.3+dfsg-1~ppa1~ubuntu16.04, Nov 21 2020, 20:30:08)
[PyPy 7.3.3 with GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>> 

$ sudo add-apt-repository ppa:pypy/ppa
$ sudo apt update
$ sudo apt install pypy3
$ pypy3
Python 3.6.12 (7.3.3+dfsg-2~ppa1~ubuntu16.04, Dec 20 2020, 00:30:46)
[PyPy 7.3.3 with GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

### Python
Ubuntu 16.04 should come with Python 2.7 and 3.5 already installed.  PyCharm 
has already dropped Python 3.5 so what follows jumps to >= 3.6.

Here are the commands 
```shell
$ sudo apt install python3.6
$ sudo apt install python3.6-dev
$ sudo apt install python3.6-venv
$ python3.6 --version            # 3.6.12
```
The same steps are used to install 3.7, 3.8, and 3.9 by simply changing the 
version number.
```shell
$ sudo apt install python3.7       | python3.8         | python3.9
$ sudo apt install python3.7-dev   | python3.8-dev     | python3.9-dev
$ sudo apt install python3.7-venv  | python3.8-venv    | python3.9-venv
$ python3.7 --version  # 3.7.9     | 3.8.7             | 3.9.1
```

We can now reset the alias (symbolic link) `python3` to point to the new  
Python 3.6.12 installation.
```shell
$ sudo ln -s /usr/bin/python3.6 /usr/local/bin/python3
````

## Install tox
The following strategy is to have a `.venv` virtual environment that only 
has `tox` installed.  This is used to run `tox` (i.e., as defined in `tox.ini`) 
which will then create all the development and test environments (e.g., dev 
in py3.6 and test in py2.7, py3.6, ...).

Install the virtual environment (Python 3.6)
```shell
$ cd <root of project directory>
$ python3 -m venv .venv
```

Activate the virtual environment
```shell
$ source .venv/bin/activate
```

Update pip and install tox
```shell
$ cd <root of project directory>
(.venv) $ pip install --upgrade pip
(.venv) $ pip install tox
```

### Optional - use the `setpath.sh` script to find the `tox` virtual environment
By doing this the `python` command will now be using the python3.6 you installed 
in `.venv`. 

```shell
(.venv) $ deactivate         # get out of the virtual environment
$ cd <root of project directory>
$ cp -i docs/issue878/setpath.sh setpath.sh
$ source ./setpath.sh        # set terminal path and echo to verify paths

Display executable path and version
~~~~~ pip ~~~~~~~
.venv/bin/pip
pip 20.3.3 from /.venv/lib/python3.
6/site-packages/pip (python 3.6)
~~~~~ python ~~~~
.venv/bin/python
Python 3.6.12
~~~~~ tox ~~~~~~~
.venv/bin/tox
3.21.0 imported from /.venv/lib/python3.

```
## Run `tox`
The initial version of [`tox.ini`](../../tox.ini) for this project is 
formulated to mimic the manual steps outlined in the [contributing]() 
document.  The major benefit is that tox more easily automates the setup 
for all virtual environments needed for development and testing across 
multiple python versions.

Here is the abridged terminal output from setting up the development 
environment:
```shell
(.venv) $ cd <root of project directory>
(.venv) $ tox -e dev
```
`tox.ini` only specified pytest and pycodestyle, all the other packages 
were based on what is defined in the [`setup.py`](../../setup.py) file.

## Cleanup After Running `tox`
Rerunning tox becomes much faster because of the cache created by the first 
run.  The only way to delete this cache is to manually delete all the 
artifact directories and files.  This is a deterministic process simplified 
by running the `clean_after_tox.sh` script from the root directory.

```shell
(.venv) $ cd <root of project directory>
(.venv) $ cp -i docs/tox/clean_after_tox.sh clean_after_tox.sh
(.venv) $ sh ./clean_after_tox.sh
```

# Reference Commands

## Submodules
### git clone --recurse-submodules
Clone super-project and submodules:
```shell
$ git clone --recurse-submodules
```
### git submodule add
Adding submodules to a super-project:
```shell
$ git submodule add https://github.com/pypy/pypy-manylinux-demo.git pypy-demo
$ git sumbodule add https://github.com/pypy/pypy.packages.git
$ git submodule add https://github.com/pypy/manylinux.git
$ git submodule add https://github.com/pypy/fast-utf8-methods.git
$ git submodule add https://github.com/pypy/pypy-std-ssl.git
```
### git submodule deinit
Remove submodule from a super-project:
```shell
# Remove the submodule entry from .git/config
git submodule deinit -f path/to/submodule

# Remove the submodule directory from the super-project's .git/modules 
directory
rm -rf .git/modules/path/to/submodule

# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f path/to/submodule
```