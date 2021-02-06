# Jupyter

## Notebooks

The RH15D comes with 3 sample notebooks (`/path/to/rh/doc/notebooks/`):

- `BasicOutput.ipynb`
- `InputData.ipynb`
- `Visualisation.ipynb`

They require the following packages:

``` sh
$ pip install matplotlib
$ pip install ipympl
$ pip install ipywidgets
$ pip install numba
```

Some import statements can be obsolete:

``` python
from helita.sim import rh15d, rh15d_vis
```

If so, replace them with this:

``` python
from helita.sim import rh15d
from helita.vis import rh15d_vis
```

When running some blocks, if you get an error like this:

```
/usr/local/lib/python3.9/site-packages/xarray/backends/scipy_.py in __init__(self, filename_or_obj, mode, format, group, mmap, lock)
    109     ):
    110         if group is not None:
--> 111             raise ValueError("cannot save to a group with the scipy.io.netcdf backend")
    112
    113         if format is None or format == "NETCDF3_64BIT":

ValueError: cannot save to a group with the scipy.io.netcdf backend
```

install the `netcdf4` package:

``` sh
$ pip install netcdf4
```

Now everything will be fine and you'll be able to run all blocks in all 3 notebooks. They might, however, still be missing some packages, but likely those will be fetched from the internet automatically.

If you move notebooks files to some other directory, relative paths used in the notebooks will break. To fix that, add some variable with root path to `rh` repository on your disk, for example in `InputData.ipynb`:

``` python
from pathlib import Path

rhRepoPath = Path("~/Documents/uio/rh")

# ...

rh15d_vis.InputAtmosphere(rhRepoPath / 'Atmos/FALC_82_5x5.hdf5')
```

## Running Jupyter

### Locally

The easiest would be to run Jupyter from VS Code: https://retifrav.github.io/blog/2020/05/27/jupyter-notebook-nginx/#notebook-in-visual-studio-code

### On beehive

``` sh
$ module list

$ which jupyter
/astro/local/anaconda/envs/py38/bin/jupyter

$ whereis jupyter
jupyter: /net/alruba2.uio.no/mn/alruba2/astro/local/anaconda/envs/py38/bin/jupyter /net/alruba2.uio.no/mn/alruba2/astro/local/anaconda/bin/jupyter

$ jupyter lab --no-browser
```

Tunnel to it from your local machine:

``` sh
$ ssh -N -L 8080:localhost:8888 -J login.uio.no beehive -v
```

Now you can open it <http://localhost:8080> in your browser or connect to it from VS Code.

If you have issues with ports, for example if Jupyter cannot bind to default port, take a look at a more [detailed guide](tunneling-jupyter.md).

By the way, there is also <https://jupyterhub.uio.no/>, but it's its own thing (*no access to home or wherever*), so it's not very useful.
