#  GIF-related tasks ffmpeg can do:

1. Convert a video directly to a GIF with the default palette.
2. Generate an optimized palette to be used in a second step to make a GIF.
3. Convert ony a portion of a video to a GIF.
4. Convert a video (or part of one) to a GIF while using only every "Nth" frame (3rd or 5th, etc.)
5. Do any of the above while also cropping the video and/or resizing it to a specific size.
7. Do any of the above using a video that's streamed off a website like YouTube or Twitch instead of having to download the whole thing.
    * I haven't actually tested Twitch yet, but it definitely works on YouTube if you know the actual raw video URL which youtube-dl can give you.
6. Do any of the above, but instead just extract the frames as a series of JPG files. (Or PNG or whatever)
8. Take a series of sequentially named JPG files and turn them into a GIF at a specific framerate.

# FFMPEG Basics

You must have ffmpeg installed and configured in Windows Path. If not, see [Installing ffmpeg](install-windows.md). You need to be comfortable on the command line or else this guide is pretty useless.

## Windows Terminal

In Windows 10, I recommend installing "Windows Terminal" from the Microsoft Store. It's a new command line program with tabs, better fonts, and lots of features. It can be run the same as `cmd` but with `wt`. It's standard in Windows 11. Installing it adds "Open in Windows Terminal" to the right-click menu for folders and folder backgrounds, which is very handy. You will probably want to set your default console to Command Prompt instead of Windows Powershell in the settings.

## Loading an input file

Doing this without any output specified produces an error, "At least one output file must be specified", but before doing so it shows you the codec info for the video and/or audio, the duration, etc. This can be handy just to see these values.

`ffmpeg -i "C:\path\to\your\video.mp4"`

Quotes areonly required if there are spaces in the path. If the video is in the current directory, you can omit the path and just use the file name.

`ffmpeg -i video.mp4`

In the full output below, everything above "Input #0" is the version and build info. You can hide this info with the `-hide_banner` option.

Notice the video stream info in this line:

`Stream #0:0[0x1](und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, smpte170m/bt470bg/smpte170m, progressive), 854x480 [SAR 1:1 DAR 427:240], 1001 kb/s, 30 fps, 30 tbr, 30k tbn (default)`

It contains some information about the video stream. A similar line will appear for the audio streams, other video streams, and subtitle streams.

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

In the examples later, I will omit the `-hide_banner` argument, but using it can be nice.

## Output file

The last argument after all of the named ones is the output. (By "named", I mean all of the arguments that come after a `-option`)

`ffmpeg -i video.mp4 output.mkv`

This will convert the video to an MKV container, for example. You Can specifiy whatever filename and extension you want, and ffmpeg will figure out what you want and how to do it.

To output the first frame of the video as an image, just output to a JPG or PNG with whatever name you want.

`ffmpeg -i video.mp4 first-frame.jpg`

That's not very useful, so here's a command to get all frames as a sequence of JPG files. I want them all in a subfolder, so I put the subfolder name then a slash, then the output file name. **NOTE**: You have to make the output folder first. I use `mkdir` here to do it. As always, use quotes if your path has spaces.

```
mkdir frames
ffmpeg -hide_banner -i video.mp4 frames\%6d.jpg
```

Notice the odd filename. That's an automatic numbering scheme. It means just number each file as an integer. If it were `%d`, each file name would be an integer starting with 1. So, 1.jpg, 2.jpg... 10.jpg.... 42069.jpg. But if you want them to sort well in *all* programs, it's best to give them leading zeroes like 000001.jpg and 042069.jpg. That's what the 6 does in `%6d`. It pads the numbers with zeroes to make them at least 6 digits long. You can choose more or fewer leading zeroes based on how many frames your video has, or just pick an arbitrarily large number of them. A 5 hour video at 48 FPS is still under a million frames, so 6 is usually good enough.

## Overwritting a file

If you don't specify otherwise, ffmpeg will ask you if you want to overwrite the output file if it already exists. Rather than having to answer that prompt every time, you can specify your answer in the command by using `-y` or `-n`.

`ffmpeg -y -i video.mp4 video.mkv`

When trying different commands over and over to get the desired result, I usually add `-y`, but I will omit this from the examples.

## Just playing a file

You can play a file with all of your options without actually saving it using `ffplay` instead of `ffmpeg`. The options are all the same, except you can't specify an output file. This opens a new window with the video, but there is no way to pause, rewind or anything. You just watch and then close it.

`ffplay -i video.mp4`

## Some common arguments

`-i [path to file]` Specify an input file. Can be used multiple times to input multiple files, for example, video and audio, or multiple languages, subtitles, etc.

`-y` Overwrite the output if it exists. 

`-vf "filter1=settings,filter2=settings"` Apply video filters like cropping, scaling, deinterlacing, frame skipping, or dozens of other possible commands. See: https://ffmpeg.org/ffmpeg-filters.html for a full list. Not all filters have or need settings. Quotes are only required if there are spaces in the filter list. If you need a comma inside a filter's settings, it needs to be escaped with a backslash, like `select=mod(n\,10),yadif`. The comma after `mod` is for separating the two arguments to the `mod` function, and `yadif` is a second filter with no settings. If you didn't escape the first comma, it makes it look like it's starting a new video filter to the `-vf` parser, so it needs to be escaped to prevent that or you'll get an error.

`-r [number]` Change the framerate of the video.

Specify it before the **input** to change the input framerate which will speed up or slow down the input video. A 30 fps video input as 60 fps will play twice as fast. (This only affects the video, not the audio, but this document is about GIFs, so it's outside its scope) 

Specify it before the **output** to change the output framerate which will duplicate or drop frames to achieve the specified FPS *without* changing the apparent speed. The default output framerate is the same as the input. If the input was an image sequence, then the default is 25.

`-vsync [passthrough|cfr|vfr]` Specify how to handle videos with frames that have irregular timestamps. Useful for avoiding duplicate frames when extracting images from certain video, in which case I think the `-vsync vfr` option is the one you want (for variable frame rate).

`qscale:v [number]` A quality preset for the video portion of the ouput. This applies to codecs that have variable birtate presets or for image outputs like jpeg. A smaller number is higher quality, starting with "0". For jpg, 0, through 2 are all the same. The default is pretty bad, so I always specify 2. Can be abbreviated `-q:v`. (Use `a` instead of `v` for audio quality.)

`-ss [timestamp or value in seconds]` Seek. If specified **before** the input, it will perform a fast approximate seek, then do an accurate seek as it gets close. This is good for very large files or web files. If specified **after** the input, it decodes the entire video until it hits that point. Avoid that. Examples: `-ss 01:00` to seek to 1 minute. The full format is `HH:MM:SS.mmmm`. Can use fractional seconds like `-ss 01:05.333`. Values without a colon are seconds. `120` is two minutes, for example. There are other options, too: https://ffmpeg.org/ffmpeg-utils.html#time-duration-syntax

`-to [timestamp]` When to stop reading the video. Often used with `-ss` to only select a portion of a video. Example: `-ss 01:00 -to 01:05` to take the 5 seconds between 01:00 and 01:05.

`-t` Rather than t timestamp as in `-to`, you can specify a number of seconds to read after seeking, or from the start of a file.

You'll see some of these in the examples.

# Example Usage

## Unoptimized GIF

`ffmpeg -i video.mp4 video.gif`

It will look like pretty bad because it will use the default GIF palette, but it will have smooth motion. **WARNING:** If your video is more than a few seconds long, the output file will be huge! Don't try this with a long video.

## Unoptimized GIF with frame skipping

If you don't want your GIF to be the ame high framerate as the source, then you can skip some frames. This uses a video filter called `framestep`. This example will only use every 5th frame, which will result in an output GIF (or video) having 1/5th the framerate of the input video. So a 60 FPS input will end up as 12 FPS, and the other frames are dropped. The speed and duration of the video will remain the same because the frames keep their original timestamps; it will just look choppier. This is often desireable for a GIF.

`ffmpeg -i video.mp4 -vf framestep=5 video.gif`

Examples often found on the web use the `select` filter and then a math expression to select only frames where the math expression evaluates to `1`. They use functions like `mod` which finds remainders after division, and the variable `n` which is the frame number. Maybe these are from before `framestep` existed. I dunno.

Example: 

`ffmpeg -i video.mp4 -vf select=not(mod(n\,5)) video.gif`

That does the exact same thing as above. `mod` yields the remainder of dividing the frame number, `n`, by 5. If the frame number is a multiple of 5, then the remainder is 0, and `not` turns that into 1 and `select` will take that frame, otherwise it drops it since `not` turns all other numbers into 0 making `select` ignore them. (Notice the escaped comma since commas are used to separate different filters. See `-vf` above under "Some handy ffmpeg arguments".)

You can do other math in `select` to achieve different selections of frames.

