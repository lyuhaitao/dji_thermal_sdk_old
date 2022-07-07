# DJI Thermal SDK
> use ctypes to capsulate the DJI Thermal SDK so that we can directly use python to process thermal images. 


```python
%load_ext autoreload
%autoreload 2
```

This version of DJI Thermal SDK is 1.3, which was published on 05/15/2022

## Install

`pip install dji_thermal_sdk`

## How to use

Get the handle of a R-JPEG image.  
- A handle is a void pointer, which has been defined to be `DIRP_HANDLE`.
- invoke the function `dirp_create_from_rjpeg`

Get the resolution of a R-jpge image  
- declare `dirp_resolution_t` variable and create a instance.  
- invoke the function `dirp_get_rjpeg_resolution`

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

