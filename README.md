# UIO

UIO stuff: installation instructions, notes, etc.

<!-- MarkdownTOC -->

- [Environment](#environment)
    - [Local](#local)
    - [beehive](#beehive)
        - [SSH](#ssh)
        - [Modules](#modules)
- [Home folder](#home-folder)
- [AST5210](#ast5210)

<!-- /MarkdownTOC -->

## Environment

### Local

I use Mac OS, so all the instructions are mostly relevant to this OS.

My environment:

``` sh
$ sw_vers -productVersion
10.15.7

$ python --version
Python 3.8.7

$ pip --version
pip 20.3.3 from /usr/local/lib/python3.8/site-packages/pip (python 3.8)

$ gcc --version
Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 12.0.0 (clang-1200.0.32.28)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin

$ gfortran --version
GNU Fortran (Homebrew GCC 10.2.0_2) 10.2.0
```

If you have higher version of Python, then you might have problems with some packages, as for example `numba` package does not support Python 3.9 yet, so you'll need to downgrade to Python 3.8:

``` sh
$ brew unlink python@3.9
$ brew link python@3.8
```

Just in case, check versions for both Python and pip after that.

### beehive

#### SSH

University hosts are not exposed to the internet, so you can't connect to them directly via SSH. However, there is a gateway `login.uio.no`, from which you can reach them.

Generate an SSH key, put its public part to your `~/.ssh/authorized_keys` on `login.uio.no` (`home directory is shared across all servers`) and add the following to your local SSH config:

```
Host login.uio.no
HostName login.uio.no
User YOUR-USERNAME
IdentityFile ~/.ssh/uio

Host beehive
HostName beehive
User YOUR-USERNAME
IdentityFile ~/.ssh/uio
```

Now you will be able to jump to `beehive` host like this:

``` sh
$ ssh -J login.uio.no beehive
```

#### Modules

Majority of tools on university hosts (*such as `beehive`*) is loaded from so-called modules. You might want to have the some of the modules to load every time you log-in. To do that, create a special `.modulerc` file in your home directory:

```
$ ssh -J login.uio.no beehive
$ nano ~/.modulerc
```

Add the following contents there:

```
#%Module1.0

set version 1.0
module load Intel_parallel_studio/2018 hdf5/Intel/1.10.1 python/3.8 git gcc
```

Check if it works:

```
$ source ~/.modulerc
$ module list
```

One would expect this to work automatically on every log-in, but sadly that is not the case. Weirdly enough, simply listing modules is enough. Here's an example of Jupyter miraculously appearing after merely running the list command:

``` sh
$ which jupyter
/usr/bin/which: no jupyter in (/usr/lib64/qt-3.3/bin:/astro/local/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/lib64/openmpi/bin:/opt/ibutils/bin)

$ module list
Currently Loaded Modulefiles:
  1) Intel_parallel_studio/2018/3.051   3) python/3.8                         5) gcc/9.3.1
  2) hdf5/Intel/1.10.1                  4) git/2.9

$ which jupyter
/astro/local/anaconda/envs/py38/bin/jupyter
```

## Home folder

You can mount your university home folder in your local filesystem. Install [SSHFS](https://osxfuse.github.io/) and then:

``` sh
$ mkdir ~/Documents/uio/home
$ sshfs login.uio.no:/uio/hume/student-u22/YOUR-USERNAME ~/Documents/uio/home
```

## AST5210

Installation order:

1. [RH 1.5D](instructions/rh15d.md)
2. [Helita](instructions/helita.md)
3. [Jupyter](instructions/jupyter.md)
