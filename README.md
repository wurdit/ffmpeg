#  GIF-related tasks ffmpeg can do:

1. Extract all frames from a video as numbered jpg files (or PNG or whatever).
2. Extract all frames from only a portion of a video.
3. Extract only every "Nth" frame, like every 2nd frame or every 3rd frame, etc.
4. Generate an optimized GIF palette by looking at every frame of an image sequence.
5. Turn a seqence of image files into a GIF using an optional palette.
6. Directly download a video from the web while doing any or all of the above. 
    * This can save a lot of time if you only want a few seconds of a very long video.

# FFMPEG Basics

You must have ffmpeg installed and configured in Windows Path. If not, see [Installing ffmpeg](install-windows.md). You need to be comfortable on the command line or else this guide is pretty useless.

## Windows Terminal

In Windows 10, I recommend installing "Windows Terminal" from the Microsoft Store. It's a new command line program with tabs, better fonts, and lots of features. It can be run the same as `cmd` but with `wt`. It's standard in Windows 11. Installing it adds "Open in Windows Terminal" to the right-click menu for folders and folder backgrounds, which is very handy. You will probably want to set your default console to Command Prompt instead of Windows Powershell in the settings.

## Loading an input file

Doing this without any output specified produces an error, "At least one output file must be specified", but before doing so it shows you the codec info for the video and/or audio, the duration, etc. This can be handy just to see these values.

`ffmpeg -i "C:\path\to\your\video.mp4"`

Quotes are required if there are any spaces. If the video is in the current folder, you can omit the path and just use the file name.

`ffmpeg -i video.mp4`

In the full output below, everything above "Input #0" is the version info and options this particular EXE was built with. You can hide this info with the `-hide_banner` option.

Notice the video stream info in this line:

`Stream #0:0[0x1](und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, smpte170m/bt470bg/smpte170m, progressive), 854x480 [SAR 1:1 DAR 427:240], 1001 kb/s, 30 fps, 30 tbr, 30k tbn (default)`

It contains some information about the video stream. A siilar line will appear for the audio track(s) if present.

Full output:

```
ffmpeg version n5.0-4-g911d7f167c-20220227 Copyright (c) 2000-2022 the FFmpeg developers
  built with gcc 11.2.0 (crosstool-NG 1.24.0.533_681aaef)
  configuration: --prefix=/ffbuild/prefix --pkg-config-flags=--static --pkg-config=pkg-config --cross-prefix=x86_64-w64-mingw32- --arch=x86_64 --target-os=mingw32 --enable-gpl --enable-version3 --disable-debug --enable-shared --disable-static --disable-w32threads --enable-pthreads --enable-iconv --enable-libxml2 --enable-zlib --enable-libfreetype --enable-libfribidi --enable-gmp --enable-lzma --enable-fontconfig --enable-libvorbis --enable-opencl --disable-libpulse --enable-libvmaf --disable-libxcb --disable-xlib --enable-amf --enable-libaom --enable-avisynth --enable-libdav1d --enable-libdavs2 --disable-libfdk-aac --enable-ffnvcodec --enable-cuda-llvm --enable-frei0r --enable-libgme --enable-libass --enable-libbluray --enable-libmp3lame --enable-libopus --enable-librist --enable-libtheora --enable-libvpx --enable-libwebp --enable-lv2 --enable-libmfx --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenh264 --enable-libopenjpeg --enable-libopenmpt --enable-librav1e --enable-librubberband --enable-schannel --enable-sdl2 --enable-libsoxr --enable-libsrt --enable-libsvtav1 --enable-libtwolame --enable-libuavs3d --disable-libdrm --disable-vaapi --enable-libvidstab --enable-vulkan --enable-libshaderc --enable-libplacebo --enable-libx264 --enable-libx265 --enable-libxavs2 --enable-libxvid --enable-libzimg --enable-libzvbi --extra-cflags=-DLIBTWOLAME_STATIC --extra-cxxflags= --extra-ldflags=-pthread --extra-ldexeflags= --extra-libs=-lgomp --extra-version=20220227
  libavutil      57. 17.100 / 57. 17.100
  libavcodec     59. 18.100 / 59. 18.100
  libavformat    59. 16.100 / 59. 16.100
  libavdevice    59.  4.100 / 59.  4.100
  libavfilter     8. 24.100 /  8. 24.100
  libswscale      6.  4.100 /  6.  4.100
  libswresample   4.  3.100 /  4.  3.100
  libpostproc    56.  3.100 / 56.  3.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'video.mp4':
  Metadata:
    major_brand     : M4V
    minor_version   : 1
    compatible_brands: isomavc1mp42
    creation_time   : 2021-11-01T01:52:15.000000Z
  Duration: 00:00:15.80, start: 0.000000, bitrate: 1004 kb/s
  Stream #0:0[0x1](und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, smpte170m/bt470bg/smpte170m, progressive), 854x480 [SAR 1:1 DAR 427:240], 1001 kb/s, 30 fps, 30 tbr, 30k tbn (default)
    Metadata:
      creation_time   : 2021-11-01T01:52:15.000000Z
      handler_name    : ETI ISO Video Media Handler
      vendor_id       : [0][0][0][0]
      encoder         : Elemental H.264
At least one output file must be specified
```

If you instead use `ffmpeg -hide_banner -i video.mp4` you get less junk on top:

In the examples, I will omit the `-hide_banner` argument, but using it can be nice.

```
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'video.mp4':
  Metadata:
    major_brand     : M4V
    minor_version   : 1
    compatible_brands: isomavc1mp42
    creation_time   : 2021-11-01T01:52:15.000000Z
  Duration: 00:00:15.80, start: 0.000000, bitrate: 1004 kb/s
  Stream #0:0[0x1](und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, smpte170m/bt470bg/smpte170m, progressive), 854x480 [SAR 1:1 DAR 427:240], 1001 kb/s, 30 fps, 30 tbr, 30k tbn (default)
    Metadata:
      creation_time   : 2021-11-01T01:52:15.000000Z
      handler_name    : ETI ISO Video Media Handler
      vendor_id       : [0][0][0][0]
      encoder         : Elemental H.264
At least one output file must be specified
```

## Overwritting a file

If you don't specify otherwise, ffmpeg will ask you if you want to overwrite the output file if it already exists. Rather than having to answer that prompt every time, you can specify your answer in the command by using `-y` or `-n`.

`ffmpeg -y -i video.mp4 video.mkv`

When trying different commands over and over to get the desired result, I usually add `-y`, but I will omit this from the examples.

## Just playing a file

You can play a file with all of your options without actually saving it using `ffplay` instead of `ffmpeg`. The options are all the same, except you can't specify an output file. This opens a new window with the video.

`ffplay -i video.mp4`

## Some handy ffmpeg arguments

`-i [path to file]` Specify an input file. Can be used multiple times to input multiple files, for example, video and audio, or multiple languages, subtitles, etc.

`-y` Overwrite the output if it exists. 

`-vf "filter1=settings,filter2=settings"` Apply video filters like cropping, scaling, deinterlacing, frame skipping, or dozens of other possible commands. See: https://ffmpeg.org/ffmpeg-filters.html for a full list.

`-r [number]` Change the framerate of the video. Specify it before the input to change the input framerate which will speed up or slow down the input video, or before the output to change the output framerate which will duplicate or drop frames to achieve the specified FPS *without* changing the apparent speed.

`-vsync [passthrough|cfr|vfr]` Specify how to handle videos with frames that have irregular timestamps. Useful for avoiding duplicate frames when extracting images from certain video, in which case I think the `-vsync vfr` option is the one you want (for variable frame rate).

`qscale:v [number]` A quality preset for the video portion of the ouput. This applies to codecs that have variable birtate presets or for image outputs like jpeg. A smaller number is higher quality, starting with "0". For jpg, 0, through 2 are all the same. The default is pretty bad, so I always specify 2. Can be abbreviated `-q:v`. (Use `a` instead of `v` for audio quality.)

## Output file

The last argument after all of the named parameters is the output.

`ffmpeg -hide_banner -i video.mp4 video.mkv`

This will convert the video to an MKV container, for example.

# Example Usage

## Unoptimized GIF

`ffmpeg -i video.mp4 video.gif`

It will look like pretty bad because it will use the default GIF palette, but it will have smooth motion. **Warning:** If your video is more than a few seconds long, the output file will be huge! Don't try this with a long video.

## Unoptimized GIF with frame skipping

`ffmpeg -i video.mp4 -
