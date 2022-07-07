# DJI Thermal SDK
> use ctypes to capsulate the DJI Thermal SDK so that we can directly use python to process thermal images. 


```python
import nbdev
from dji_thermal_sdk.dji_sdk import *
import dji_thermal_sdk.dji_sdk as DJI
import ctypes as CT
from ctypes import *
```

This version of DJI Thermal SDK is 1.3, which was published on 05/15/2022

## Install

`pip install dji_thermal_sdk`

## Load DLL

Normally, DJI SDK DLLs include `libdirp.dll, libv_dirp.dll, libv_girp.dll, libv_iirp.dll, libv_list.ini`.  

you should put all the dlls and your codes in a same folder.

```python
'''
try:
    _libdirp = cdll.LoadLibrary("libdirp.dll")
    DJI.set_dirp_dll(_libdirp)
except FileNotFoundError as err:
    print(err)
print(DJI._libdirp)
'''
import os
os.path.exists("libdirp.dll")
```




    True



## Get the handle of a R-JPEG image

DIRP_HANDLE is a void pointer, and it has been definded.  
you can get it by `package.DIRP_HANDLE`

```python
nbdev.show_doc(dirp_create_from_rjpeg)
```


<h4 id="dirp_create_from_rjpeg" class="doc_header"><code>dirp_create_from_rjpeg</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L162" class="source_link" style="float:right">[source]</a></h4>

> <code>dirp_create_from_rjpeg</code>(**`data`**, **`size`**, **`ph`**)

Parameters:
    [in] data: R-JPEG binary data buffer pointer
    [in] size: R-JPEG binary data buffer size in bytes
    [out]ph  : DIRP API handle pointer
        - reminder: use two-level pointer to assign value to one-level pointer
Return:
    int return code dirp_ret_code_e


## Get the resolution of a R-JPEG image

12
