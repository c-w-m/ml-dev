# ![Machine Learning Development](https://github.com/c-w-m/ml-dev)

#### Table of Contents
* [Setup](#markdown-header-setup)

> Note: The following projects are based on opens source tools and free 
> services.

This is an introduction level set of projects that focus on machine learning 
(ML).

Remember to use the `recursive` clone to get all the submodules.

## Setup

### Clone - recursive to get submodules
```shell
$ git clone --recurse-submodules https://github.com/c-w-m/ml-dev.git
```

### Submodules
```shell
# The following submodules were added:
$ cd src
$ git submodule add https://github.com/c-w-m/colab.git
$ git submodule add https://github.com/c-w-m/colabtools.git
$ git submodule add https://github.com/c-w-m/pdsh.git

```
#### Removing Submodules
```shell
# Remove the submodule entry from .git/config
git submodule deinit -f path/to/submodule

# Remove the submodule directory from the super-project's .git/modules 
directory
rm -rf .git/modules/path/to/submodule

# Remove the entry in .gitmodules and remove the submodule directory located at path/to/submodule
git rm -f path/to/submodule
```

### Helper Scripts
Two helper scripts are included, one to set the terminal path to the virtual
environment, and the other removes all the `tox` created artifacts.  Remember to
tweak these as needed for the specific project directory names.

#### Set path
* First make sure your environment is correctly set (see [Ubuntu ReadMe](doc/Ubuntu/ReadMe.md))
* make the script executable
```shell
$ chmod +x setpath.sh
```
* run
```shell
$ source ./setpath.sh
```

#### Clean Up
Run this if you want to remove all the `tox` cache directories (i.e., everything
that is not checked into the repository).
* make the script executable
```shell
$ chmod +x clean_after_tox.sh
```
* run
```shell
$ sh ./clean_after_tox.sh
```

## References

### Tutorials
* []()

### Misc Code
* []()

### Blogs
* []()

### Google SAS
* []()

## eLib
* []()
