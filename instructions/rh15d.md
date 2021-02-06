# RH15D

<https://tiagopereira.space/ast5210/>

## Dependencies

``` sh
$ brew install open-mpi
$ brew install hdf5-mpi
```

## Build

Clone repository:

``` sh
$ cd ~/Documents/uio
$ git clone git@github.com:ITA-Solar/rh.git
```

Edit `Makefile.config`:

```
CC       = mpicc
CFLAGS   = -O2 -DHAVE_F90 -Wformat

F90C     = gfortran
F90FLAGS = -O2

LD       = mpicc
LDFLAGS  =

AR       = ar
ARFLAGS  = rs
RANLIB   = ranlib

HDF5_DIR = /usr/local/opt/hdf5-mpi

OS = $(shell uname -s)
```

Build main libraries:

``` sh
$ make -j12
```

Build executables:

``` sh
$ cd rh15d
$ make -j12
```

You should get the following binaries:

- `rh15d_lteray`
- `rh15d_ray`
- `rh15d_ray_pool`

You can test it by running the following:

``` sh
$ cd run_example
$ mpiexec -np 4 ../rh15d_ray_pool
```

Results will be written to `output` folder.
