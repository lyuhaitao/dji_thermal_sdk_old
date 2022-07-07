# DJI Thermal SDK
> use ctypes to capsulate the DJI Thermal SDK so that we can directly use python to process thermal images. 


```python
%load_ext autoreload
%autoreload 2
```

```python
import dji_thermal_sdk.dji_sdk as DJI
from dji_thermal_sdk.dji_sdk import *
import dji_thermal_sdk.utility as utility
import ctypes as CT
from ctypes import *
```

```python
try:
    #_libdirp = cdll.libdirp
    _libdirp = cdll.LoadLibrary("libdirp.dll")
    DJI.set_dirp_dll(_libdirp)
except FileNotFoundError as err:
    print("Please copy libdirp.dll, lib_dirp.dll, lib_girp.dll, lib_iirp.dll, and lib_list.ini. to your folder.")
```

This version of DJI Thermal SDK is 1.3, which was published on 05/15/2022

## Install

`pip install dji_thermal_sdk`

## How to use

Get the handle of a R-JPEG image.  
- A handle is a void pointer, which has been defined to be `DIRP_HANDLE`.
- invoke the function `dirp_create_from_rjpeg`

```python
import os
import matplotlib.pyplot as plt
```

```python
# get all jpg images in a directory
root_directory = r'dataset'
ret = utility.FindFilesByExtension(root_directory, "JPG")
print("File Name\tFile Path")
for f in ret:
    print(f"{f.file_name}\t{f.file_path}")
#

rd = ret[0].file_path
with open(rd, 'rb') as f:
    content = f.read()
# method 1 to get the file size
file_stat = os.stat(rd)
size = c_int32(file_stat.st_size)
print(f"File size: {size}")

# the method to create a string buffer, which is important.
rjpeg_data = CT.create_string_buffer(len(content))
rjpeg_data.value = content
# test the function to create a handle of an image
ret = dirp_create_from_rjpeg(rjpeg_data,size, CT.byref(DIRP_HANDLE))
print(f'ret = {ret}')
if ret == 0:
    print("successfully get the r-jpeg handle.")
#
print(f"DIRP_HANDLE: {DIRP_HANDLE}  address: {hex(DIRP_HANDLE.value)} ")
```

    File Name	File Path
    Deer_Goats_Unsure__2022-02-02__02-42-12(2).JPG	dataset\Deer_Goats_Unsure__2022-02-02__02-42-12(2).JPG
    File size: c_long(1367428)
    ret = 0
    successfully get the r-jpeg handle.
    DIRP_HANDLE: c_void_p(2348746175776)  address: 0x222dc2e7520 
    

Get the resolution of a R-jpge image  
- declare `dirp_resolution_t` variable and create a instance.  
- invoke the function `dirp_get_rjpeg_resolution`

```python
rjpeg_resolution = dirp_resolution_t()
ret = dirp_get_rjpeg_resolution(DIRP_HANDLE, CT.byref(rjpeg_resolution))
print(f'ret = {ret}')
if ret == 0:
    print("successfully get the resolution.")
out = f'Height: {rjpeg_resolution.height}, width: {rjpeg_resolution.width}'
out
```

    ret = 0
    successfully get the resolution.
    




    'Height: 512, width: 640'



```python
nbdev.showdoc.show_doc(dirp_get_rjpeg_resolution)
```


<h4 id="dirp_get_rjpeg_resolution" class="doc_header"><code>dirp_get_rjpeg_resolution</code><a href="https://github.com/haitaolyu/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L330" class="source_link" style="float:right">[source]</a></h4>

> <code>dirp_get_rjpeg_resolution</code>(**`h`**, **`rjpeg_info`**)

Get R-JPEG image resolution information.
Parameters
    [in]h: DIRP API handle
    [out]rjpeg_info: R-JPEG basic information pointer
Returns
    int return code dirp_ret_code_e

