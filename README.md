# Welcome
Just a place to dump info on ffmpeg for sharing with friends

You must have ffmpeg installed and configured in Windows Path. If not, see [Installing ffmpeg](install-windows.md).

GIF-related tasks ffmpeg can do:

1. Extract all frames from a video as numbered jpg files (or PNG or whatever).
2. Extract all frames from only a portion of a video.
3. Extract only every "Nth" frame, like every other frame or every 3rd frame. etc.
4. Generate an optimized GIF palette by looking at every frame of an image sequence.
5. Turn a seqence of image files into a GIF using an optional palette.

## Windows Terminal

In Windows 10, I recommend installing "Windows Terminal" from the Microsoft Store, which adds "Open in Windows Terminal" to the right-click menu for folders and folder backgrounds. It's a new command line program with tabs, better fonts, and lots of features. It can be run the same as `cmd` but with `wt`. It's standard in Windows 11.

## Loading a file

Doing this without any output parameters produces an error, "At least one output file must be specified", but before doing so it shows you the codec info for the video and/or audio, the duration, etc. This can be handy just to see these values.

`ffmpeg -i "C:\path\to\your\video.mp4"`

Quotes are required if there are any spaces. If the video is in the current folder, you can omit the path and just use the file name.

`ffmpeg -i video.mp4`

Example output (abridged). Notice the video in Stream #0:0: codec (h264), pixel format (yuv420p), dimensions (1920x1080), fps, duration (00:50:03.334)

```
  Stream #0:0: Video: h264 (High), yuv420p(tv, bt709, progressive), 1920x1080 [SAR 1:1 DAR 16:9], 24 fps, 24 tbr, 1k tbn (default) (original)
    Metadata:
      BPS             : 5063372
      DURATION        : 00:50:03.334000000
      NUMBER_OF_FRAMES: 72080
      NUMBER_OF_BYTES : 1900874782
  Stream #0:1(eng): Audio: eac3, 48000 Hz, 5.1(side), fltp, 768 kb/s (default) (original)
    Metadata:
      BPS             : 768000
      DURATION        : 00:50:03.360000000
      NUMBER_OF_FRAMES: 93855
      NUMBER_OF_BYTES : 288322560
```

## Output

The last argument after all of the named parameters is the output.

`ffmpeg -i video.mp4 video.
