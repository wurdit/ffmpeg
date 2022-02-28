#  GIF-related tasks ffmpeg can do:

1. Convert a video directly to a GIF with the default palette.
4. Convert a video (or part of one) to a GIF while using only every "Nth" frame (E.g., Every 3rd or 5th frame.)
3. Convert ony a portion of a video to a GIF.
2. Generate an optimized palette to be used in a second step to make a better looking GIF.
5. Do any of the above while also cropping the video and/or resizing it to a specific size.
7. Do any of the above using a video that's streamed off a website like YouTube or Twitch instead of having to download the whole thing.
    * I haven't actually tested Twitch yet, but it definitely works on YouTube if you know the actual raw video URL which youtube-dl can give you.
6. Do any of the above, but instead just extract the frames as a series of JPG files. (Or PNG or whatever)
8. Take a series of sequentially named image files and turn them into a GIF at a specific framerate.

# FFMPEG Basics

You must have ffmpeg installed and configured in Windows Path. If not, see [Installing ffmpeg](install-windows.md). You need to be comfortable on the command line or else this guide is pretty useless.

## Windows Terminal

This is totally optional, but in Windows 10, I recommend installing "Windows Terminal" from the Microsoft Store. It's a new command line program with tabs, better fonts, and lots of features. It can be run the same as `cmd` but with `wt`. It's standard in Windows 11. Installing it adds "Open in Windows Terminal" to the right-click menu for folders and folder backgrounds, which is very handy. If you install it, you will probably want to set your default console to Command Prompt instead of Windows Powershell in its settings.

## Loading an input file

Doing this without any output specified produces an error, "At least one output file must be specified", but before the error it shows you the codec info for the video and/or audio, the duration, etc. This can be handy just to see these values.

`ffmpeg -i "C:\path\to\your\video.mp4"`

Quotes are only required if there are spaces in the path. If the video is in the current directory, you can omit the path and just use the file name.

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

In the examples, I will omit the `-hide_banner` argument, but using it can be nice.

## Output file

The last argument after all of the named ones is the output. (By "named", I mean all of the arguments that come after a `-option`)

`ffmpeg -i video.mp4 output.mkv`

This will convert the video to an MKV container, for example. You can specifiy whatever filename and extension you want. ffmpeg will figure out how to make the file or files you asked for.

To output the first frame of the video as an image, just output to a JPG or PNG with whatever name you want. This will give you a warning, but it works. 

`ffmpeg -i video.mp4 first-frame.jpg`

To avoid the warning, use `-frames:v 1` to tell ffmpeg to only take one frame.

`ffmpeg -i video.mp4 -frames:v 1 first-frame.jpg`

That's not very useful, so here's a command to get *all* frames as a sequence of JPG files. I want them all in a subfolder, so I put the subfolder name then a slash, then the output file name. **NOTE**: You have to make the output folder first. I use `mkdir` here to do it. As always, use quotes if your path has spaces.

```
mkdir frames
ffmpeg -hide_banner -i video.mp4 frames\%6d.jpg
```

Now I have a folder full of hundreds of frames from my ~16 second video.

Notice the odd filename. That's an automatic numbering scheme. It means "just number each file as an integer". If it were `%d`, the filenames would be 1.jpg, 2.jpg... 10.jpg.... 42069.jpg. But if you want them to sort well in all programs, it's best to give them leading zeroes like 000001.jpg and 042069.jpg. That's what the 6 does in `%6d`. It pads the numbers with zeroes to make them at least 6 digits long. You can choose more or fewer leading zeroes based on how many frames your video has, or just pick an arbitrarily large number of them. A 5 hour video at 48 FPS is still under a million frames, so 6 is usually good enough.

## Overwritting a file

If you don't specify otherwise, ffmpeg will ask you if you want to overwrite the output file if it already exists. Rather than having to answer that prompt every time, you can specify your answer in the command by using `-y` or `-n`.

`ffmpeg -y -i video.mp4 video.mkv`

When trying different commands over and over to get the desired result, I usually add `-y`, but I will omit this from the examples.

## Just playing a file

You can play a file with all of your options without actually saving it using `ffplay` instead of `ffmpeg`. The options are all the same, except you can't specify an output file. This opens a new window with the video, but there is no way to pause, rewind or anything. You just watch and then close it.

`ffplay -i video.mp4`

## Some common arguments

You'll see these in the exmples. You don't necessarily need to read and understand all of these just yet.

`-i [path to file]` Specify an input file. Can be used multiple times to input multiple files, for example, video and audio, or multiple languages, subtitles, etc.

`-y` Overwrite the output if it exists. 

`-vf "filter1=settings,filter2=settings"` Apply video filters like cropping, scaling, deinterlacing, frame skipping, or dozens of other possible commands. See: https://ffmpeg.org/ffmpeg-filters.html for a full list. Not all filters have or need settings. Quotes are only required if there are spaces in the filter list. If you need a comma inside a filter's settings, it needs to be escaped with a backslash, like `select=mod(n\,10),yadif`. The comma after `mod` is for separating the two arguments to the `mod` function, and `yadif` is a second filter with no settings. If you didn't escape the first comma, it makes it look like it's starting a new video filter to the `-vf` parser, so it needs to be escaped to prevent that or you'll get an error.

`-filter_complex` Let's you specify multiple inputs and map them to various filters. You can also create temporary streams in one filter and use them in later filters. I will go into this only as needed in the exmaples.

`-r [number]` Change the framerate of the video.

* Specify it before the **input** to change the input framerate which will speed up or slow down the input video. A 30 fps video input as 60 fps will play twice as fast. (This only affects the video, not the audio, but this document is about GIFs, so it's outside its scope) 
* Specify it before the **output** to change the output framerate which will duplicate or drop frames to achieve the specified FPS *without* changing the apparent speed. The default output framerate is the same as the input. If the input was an image sequence, then the default is 25.

`-vsync [passthrough|cfr|vfr]` Specify how to handle videos with frames that have irregular timestamps. Useful for avoiding duplicate frames when extracting images from certain video, in which case I think the `-vsync vfr` option is the one you want ("vfr" for variable frame rate). Don't worry about this option unless you encounter duplcate images in your image output.)

`qscale:v [number]` A quality preset for the video/image portion of the ouput. This applies to codecs that have variable birtate presets and for image outputs like jpeg. A smaller number is higher quality, starting with "0". For jpg, 0, through 2 are all the same. The default is pretty bad, so I always specify 2. Can be abbreviated `-q:v`. (Use `a` instead of `v` for audio quality.) Example: `-q:v 2`

`-ss [timestamp]` Seek to a point in the video and ignores everything before it. If specified **before** the input, it will perform a fast approximate seek, then do an accurate seek as it gets close. This is good for very large files or web files. If specified **after** the input, it decodes the *entire* video until it hits that point. Avoid that. Examples: `-ss 01:00` to seek to 1 minute. The full format is `HH:MM:SS.mmmm`. Can use fractional seconds like `-ss 01:05.333`. Values without a colon are seconds. `120` is two minutes, for example. There are other options, too: https://ffmpeg.org/ffmpeg-utils.html#time-duration-syntax

`-to [timestamp]` When to stop reading the video. Often used with `-ss` to only select a portion of a video. Example: `-ss 01:00 -to 01:05` to take just the 5 seconds between 01:00 and 01:05.

`-t` Rather than t timestamp as in `-to`, you can specify a number of seconds to read after seeking, or from the start of a file.

You'll see some of these in the examples.

# Example Usage

## Unoptimized GIF

`ffmpeg -i video.mp4 video.gif`

It will look like pretty bad because it will use the default GIF palette, but it will have smooth motion. **WARNING:** If your video is more than a few seconds long, the output file will be huge! Don't try this with a long video.

## Unoptimized GIF with frame skipping

If you don't want your GIF to be the ame high framerate as the source, then you can skip some frames. This uses a video filter called `framestep`. This example will only use every 5th frame, which will result in an output GIF (or video) having 1/5th the framerate of the input video. So a 60 FPS input will end up as 12 FPS, and the other frames are dropped. The speed and duration of the video will remain the same because the frames keep their original timestamps; it will just look choppier. This is sometimes desireable for a GIF.

`ffmpeg -i video.mp4 -vf framestep=5 video.gif`

Examples often found on the web use the `select` filter and then a math expression to select only frames where the math expression evaluates to `1`. They use functions like `mod` which finds remainders after division, and the variable `n` which is the frame number. Maybe these are from before `framestep` existed. I dunno.

Example: 

`ffmpeg -i video.mp4 -vf select=not(mod(n\,5)) video.gif`

That does the exact same thing as above. `mod` yields the remainder of dividing the frame number (`n`) by 5. If the frame number is a multiple of 5, then the remainder is 0, and `not` turns that into 1 so `select` will take that frame. Otherwise it drops it since `not` turns all other numbers into 0 making `select` ignore them. (Notice the escaped comma since commas are used to separate multiple filters. See `-vf` above under "Some handy ffmpeg arguments".)

You can do other math in `select` to achieve different selections of frames. That's not in the scope of the document, though.

## Unoptimized GIF of a portion of a video

If you don't want the whole video, you can use seek (`-ss`) and to (`-to`) to select a range of the video. The position of `-ss` in the command relative to the input is important. Read more about it in "Some handy ffmpeg arguments". In short, always put it **before** the input unless it seems inaccurate.

`ffmpeg -ss 00:10 -to 00:15 -i video.mp4 five-seconds.gif`

This makes a 5 second GIF starting at 10 seconds into the video. You can combine this with video filters like the `framestep` above.

`ffmpeg -ss 00:10 -to 00:15 -i video.mp4 -vf framestep=5 five-seconds-choppy.gif`

## Making an optimized palette GIF

To generate a good palette, we use the `palettegen` filter. It has options that can improve the colors, but they're usually not needed. For example, you can have a different palette *per frame*. You can read about these options elsewhere.

### Step 1: make the palette

You use the `palettegen` filter and specify a PNG output.

`ffmpeg -i video.mp4 -vf palettegen video-palette.png`

You should use the same "seek" and "to" as your final GIF in order to only look at the colors in that portion of the video. 

`ffmpeg -ss 00:10 -to 00:15 -i video.mp4 -vf palettegen video-palette.png`

### Step 2: use the palette and the input video to make a GIF

You use two inputs and now we have to use a complex filter since `paletteuse` requires two inputs. You specify the inputs first, then the filter name. `[0:v]` means "the video portion of input 0" (the first input). `[1:v]` means the same for the second input, even though it's not a video. Images are considered video in this context. (They certainly aren't audio.) With `-filter_complex`, you use a semicolon, `;` to separate filters, not a comma.

`ffmpeg -ss 00:10 -to 00:15 -i video.mp4 -i video-palette.png -filter_complex [0:v][1:v]paletteuse video.gif`

It's a lot slower than not using a custom palette, but the results don't look like a GIF from the 90's.

### Doing it in one step

If you care, you can do it all in one step and not save the palette to the disk. This time, I added spaces to aid readability, so quotes are used. This takes the first input's video stream, `[0:v]`, and runs `palettegen` on it and creates a temporary output stream named "pal", which is our palette. Then the video and "pal" are sent to `peletteuse`. This results in an identical GIF as above.

`ffmpeg -ss 00:10 -to 00:15 -i video.mp4 -i video-palette.png -filter_complex "[0:v] palettegen [pal]; [0:v][pal] paletteuse" one-step.gif`

## Cropping the input

The `crop` filter takes the follwing settings: `width:height:x:y`. E.g., `-vf crop=256:256:100:100`. This makes a 256x256 px crop that starts 100 pixels in from the left, and 100 pixels down from the top. Rather than guessing, grab one screen with ffmpeg, and then use Photoshop or Paint to make your desired selection and find the top-left pixel and size of that selection. If you have Photoshop I'll assume you know how to do that using the Info window, so I'll only explain Paint.

### Grab one frame for reference

Use `-ss` to seek to the part of the video where you want your GIF to start, then grab one frame as a PNG. You don't need `-to` since we are going to specify `-frames:v 1` to limit the output to one frame. Even that's not required, but you'll get a warning.

`ffmpeg -ss 00:10 -i video.mp4 -frames:v 1 frame.png`

### Using Paint to find crop settings

1. Open your frame in Paint and zoom out until it fits comfortably in the window.
    * Zoom into just the section you want for more precise measurements.
3. Using the Select tool, start drawing a box around the area you want, **but do not let go!**
4. Look at the numbers circled in red at the bottom of the window: https://i.imgur.com/my0JUFI.png
    * The first set of numbers is the x,y position of the top-left pixel of your selection.
    * The second set is the width and height.
    * If you don't see the first set of numbers, it's because you let go of the mouse in step 2. Don't do that.
5. Get the size of the box *about* the size you want. If it's about 580 x 580, but your selection is actually 577 x 579, don't try to make it perfect; it's not important. Just note that 580 x 580 is what you want.
6. Memorize all the numbers before you let go of the mouse then format your `crop` filter.

In the above example, the size can be rounded to 580 x 580, and the top-left pixel is (477, 301), so our crop command is `crop=580:580:477:301`. Super easy!

I'll make a new frame using this filter:

`ffmpeg -ss 00:10 -i video.mp4 -vf crop=580:580:477:301 -frames:v 1 cropped-frame.png`

Result: https://i.imgur.com/6UyWyfb.png Perfect! No guesswork involved.


















