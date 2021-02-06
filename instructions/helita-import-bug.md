# Helita import bug

This bug is [already fixed](https://github.com/ITA-Solar/helita/issues/25) (`169729242cd83244994c4a29767a4b64b2159e2b`), ignore these instructions.

There is an error in `rh15d_vis.py`. When you'll try to import Helita in your Jupyter notebook, you'll get this error:

```
---> 10 from scipy.integrate.quadrature import cumtrapz
     11 from scipy.interpolate import interp1d
     12 from astropy import units as u

ModuleNotFoundError: No module named 'scipy.integrate.quadrature'
```

To fix the problem, edit this file:

``` diff
--- a/helita/vis/rh15d_vis.py
+++ b/helita/vis/rh15d_vis.py
-from scipy.integrate.quadrature import cumtrapz
+from scipy.integrate import quadrature
+from scipy.integrate import cumtrapz
```

and then re-install Helita:

``` sh
pip install -e .
```
