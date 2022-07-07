# DJI Thermal SDK
> use ctypes to capsulate the DJI Thermal SDK so that we can directly use python to process thermal images. 


```python
import nbdev
from dji_thermal_sdk.dji_sdk import *
import dji_thermal_sdk.dji_sdk as DJI
import ctypes as CT
from ctypes import *
import os
```

This version of DJI Thermal SDK is 1.3, which was published on 05/15/2022

## Install

`pip install dji_thermal_sdk`

## Load DLL

Normally, DJI SDK DLLs include `libdirp.dll, libv_dirp.dll, libv_girp.dll, libv_iirp.dll, libv_list.ini`.  

you should put all the dlls and your codes in a same folder.  

The reason that the following codes are commented is because it can't pass the GitHub CI test, but it works well.

DJI dlls use C++, and when we use ctypes to invoke them, python complier will pop out 'invalid ELF header' error.

```python
'''
try:
    _libdirp = cdll.LoadLibrary("./libdirp.dll")
    DJI.set_dirp_dll(_libdirp)
except FileNotFoundError as err:
    print(err)
print(DJI._libdirp)
'''
```

    <CDLL 'D:\LYU\Code\git_repository\dji_thermal_sdk\libdirp.dll', handle 7ffe6f310000 at 0x1aa05b61850>
    




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


```python
'''
rd = r"dataset\Deer_Goats_Unsure__2022-02-02__02-42-12(2).JPG"
with open(rd, 'rb') as f:
    content = f.read()
    print(len(content))
# method1 to get the file size
print(f"File size: {os.path.getsize(rd)}")
# method 2 to get the file size
file_stat = os.stat(rd)
size = c_int32(file_stat.st_size)
print(f"File size: {size}")

# the method to create a string buffer, which is important.
rjpeg_data = CT.create_string_buffer(len(content))
rjpeg_data.value = content
print(f"rjpeg_data: {rjpeg_data}")

# test the function to create a handle of an image
ret = dirp_create_from_rjpeg(rjpeg_data,size, CT.byref(DIRP_HANDLE))
print(f'ret = {ret}')
if ret == 0:
    print("successfully get the r-jpeg handle.")
#
print(f"DIRP_HANDLE: {DIRP_HANDLE}  address: {hex(DIRP_HANDLE.value)} ")
'''
```

## Get the version of API

```python
nbdev.show_doc(dirp_get_api_version)
```


<h4 id="dirp_get_api_version" class="doc_header"><code>dirp_get_api_version</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L193" class="source_link" style="float:right">[source]</a></h4>

> <code>dirp_get_api_version</code>(**`version`**)

Parameters:
    [out] version DIRP API version information pointer
Return:
    int return code dirp_ret_code_e


```python
nbdev.show_doc(dirp_api_version_t)
```


<h2 id="dirp_api_version_t" class="doc_header"><code>class</code> <code>dirp_api_version_t</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L68" class="source_link" style="float:right">[source]</a></h2>

> <code>dirp_api_version_t</code>() :: `Structure`

API version structure definition


```python
'''
jpeg_version = dirp_api_version_t() 
ret = dirp_get_api_version(CT.byref(jpeg_version))
if ret == DIRP_SUCCESS:
    print("Success")
#
print(f"jpeg_version.api: \t {jpeg_version.api}")
print(f"jpeg_version.magic: \t {jpeg_version.magic}")
'''
```

## Get Color Bar

```python
nbdev.show_doc(dirp_get_color_bar)
```


<h4 id="dirp_get_color_bar" class="doc_header"><code>dirp_get_color_bar</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L205" class="source_link" style="float:right">[source]</a></h4>

> <code>dirp_get_color_bar</code>(**`h`**, **`color_bar`**)

Parameters:
    [in]  h: DIRP API handle
    [out] color_bar: ISP color bar parameters pointer
Return:
    int return code dirp_ret_code_e


```python
nbdev.show_doc(dirp_color_bar_t)
```


<h2 id="dirp_color_bar_t" class="doc_header"><code>class</code> <code>dirp_color_bar_t</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L74" class="source_link" style="float:right">[source]</a></h2>

> <code>dirp_color_bar_t</code>() :: `Structure`

Color bar parameters structure definition


```python
'''
jpeg_color_bar = dirp_color_bar_t()
ret = dirp_get_color_bar(DIRP_HANDLE, CT.byref(jpeg_color_bar))
if ret == DIRP_SUCCESS:
    print("Success")
print(f"jpeg_color_bar.high: \t {jpeg_color_bar.high}")
print(f"jpeg_color_bar.low: \t {jpeg_color_bar.low}")
print(f"jpeg_color_bar.manual_enable: \t {jpeg_color_bar.manual_enable}")
'''
```

## Get the resolution of a R-JPEG image

nbdev.show_doc(dirp_create_from_rjpeg)

```python
nbdev.show_doc(dirp_get_rjpeg_resolution)
```


<h4 id="dirp_get_rjpeg_resolution" class="doc_header"><code>dirp_get_rjpeg_resolution</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L336" class="source_link" style="float:right">[source]</a></h4>

> <code>dirp_get_rjpeg_resolution</code>(**`h`**, **`rjpeg_info`**)

Get R-JPEG image resolution information.
Parameters
    [in]h: DIRP API handle
    [out]rjpeg_info: R-JPEG basic information pointer
Returns
    int return code dirp_ret_code_e


```python
nbdev.show_doc(dirp_resolution_t)
```


<h2 id="dirp_resolution_t" class="doc_header"><code>class</code> <code>dirp_resolution_t</code><a href="https://github.com/lyuhaitao/dji_thermal_sdk/tree/master/dji_thermal_sdk/dji_sdk.py#L133" class="source_link" style="float:right">[source]</a></h2>

> <code>dirp_resolution_t</code>() :: `Structure`

The image size structure definition


```python
'''
rjpeg_resolution = dirp_resolution_t()
ret = dirp_get_rjpeg_resolution(DIRP_HANDLE, CT.byref(rjpeg_resolution))
print(f'ret = {ret}')
if ret == 0:
    print("successfully get the resolution.")

out = f'Height: {rjpeg_resolution.height}, width: {rjpeg_resolution.width}'
out
'''
```

## Set Pseudo Color

```python
nbdev.show_doc(dirp_set_pseudo_color)
```

```python
'''
ret = dirp_set_pseudo_color(DIRP_HANDLE, c_int(0))
if ret == DIRP_SUCCESS:
    print("Success")
else:
    print(f"Error: ret={ret}")
'''
```

## Transform a thermal image by a specific palette

```python
nbdev.show_doc(dirp_process)
```

```python
'''
import matplotlib.pyplot as plt
import numpy as np
size = rjpeg_resolution.height * rjpeg_resolution.width * 3 * CT.sizeof(c_uint8)
raw_image_buffer = CT.create_string_buffer(size)
print(raw_image_buffer.raw[100])
ret = dirp_process(DIRP_HANDLE,byref(raw_image_buffer), size)
if ret == DIRP_SUCCESS:
    print("Success")
else:
    print(f"Error: ret={ret}")
#
raw_file_path = os.path.splitext(rd)[0] + ".raw"
print(raw_file_path)
with open(raw_file_path, 'wb') as f:
    f.write(raw_image_buffer.raw)
#
if os.path.exists(raw_file_path):
    print(f"Success! file size: {os.path.getsize(raw_file_path)}")
else:
    print("Error")
#
with open(raw_file_path, encoding='cp1252') as fin:
    img = np.fromfile(fin, dtype = np.uint8)
    print(img.shape)
    img.shape = (512,640,3)
    #original = Image.fromarray(img)
#

fig = plt.figure(figsize=(10,8))
plt.imshow(img, cmap='gray')
'''
```
