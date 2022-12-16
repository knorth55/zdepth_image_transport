# zdepth_image_transport

ROS zdepth image transport plugin

`zdepth_image_transport` is for faster and lighter depth compression.
[Zdepth](https://github.com/catid/Zdepth) is a depth compression method with zstd library.

## Comparison 

### ZDepth (This repo) 

With Zdepth, we can get 30Hz rgb and 30Hz Zdepth.
~Zdepth's decompression is 7Hz, so the visualization seems a bit slow, but faster than compressedDepth (png).~
Now we got 30Hz for decompression, too.

Left: Original, Right: compressed -> decompressed

https://user-images.githubusercontent.com/9300063/199556928-821fe403-f63a-456a-a633-a407bf5fe384.mp4

### CompressedDepth (png)

Because compression of png is so slow, it causes slow down for other depth topics.
(image_transport is serial process, and if one compression is slow, others will be slow down).
The compression is 4Hz, so the raw depth is also 4Hz.
and the decompression is 4Hz.
Ofcourse, we can make it faster with lower png_level, but the compression will be worse.

Left: Original, Right: compressed -> decompressed

https://user-images.githubusercontent.com/9300063/199556980-136d74a3-0a09-4df1-9e8e-354b7b65463f.mp4


## How to try

```
roslaunch openni2 openni2.launch
rosrun image_transport republish zdepth in:=/camera/depth/image_raw raw out:=/camera/depth_decompressed/image_raw raw
```

## Topic size comparison

- ZDepth: `<array type: uint8, length: 35006>`
- compressedDepth (png): `<array type: uint8, length: 61084>`

- Original: `<array type: uint8, length: 614400>`

## Frequency Comparison

### Compression

- ZDepth

```
$ rostopic hz /camera/depth/image_raw/zdepth
subscribed to [/camera/depth/image_raw/zdepth]
average rate: 29.973
        min: 0.033s max: 0.034s std dev: 0.00010s window: 17
```

- Compressed Depth (png)
```
$ rostopic hz /camera/depth/image_raw/compressedDepth
subscribed to [/camera/depth/image_raw/compressedDepth]
average rate: 4.155
        min: 0.241s max: 0.241s std dev: 0.00000s window: 2
```

### Decompression

- Zdepth

```
$ rostopic hz /camera/depth/image_raw/zdepth
subscribed to [/camera/depth/image_raw/zdepth]
average rate: 29.609
        min: 0.013s max: 0.044s std dev: 0.00686s window: 29

```

- Compressed Depth (png)

```
$ rostopic hz /camera/depth_decompressed/image_raw
subscribed to [/camera/depth_decompressed/image_raw]
average rate: 4.175
        min: 0.233s max: 0.244s std dev: 0.00462s window: 4
```
