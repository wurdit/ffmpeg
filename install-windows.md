# Installing ffmpeg in Windows

## What you need

1. Download ffmpeg, save it somewhere handy, and configure it in the Windows PATH environment variable.
2. (Optional) Download either youtube-dl or yt-dlp or both, also configured in the PATH variable.

See the Install instructions below if you don't know how to regiter programs in Windows PATH.

## Download ffmpeg

ffmpeg is a very powerful, general purpose command-line video editor. Easy to use in batch files or on the command line.

https://github.com/BtbN/FFmpeg-Builds/releases

Get `ffmpeg-n5.0-latest-win64-gpl-shared-5.0.zip` (Or 5.1 or whatever is most current.)

## Download youtube-dl

youtube-dl is an internet video downloader. It lets you easily download YouTube videos and also works on nearly every video site on the internet.

https://github.com/ytdl-org/youtube-dl/releases

Get `youtube-dl.exe` from the top-most release.

youtube-dl is ironically bad at downloading YouTube videos right now. There are forks of the project that are much, *much* faster at downloading videos from YouTube. There's some bug in the current official version that slows it down.

The fork I use is called yt-dlp. It behaves differently from the official youtube-dl in little ways, so online tutorials may not 100% work with it without changing how you do it, but for 99% of the use cases it's perfect. You may as well get both and use whichever one you prefer.

https://github.com/yt-dlp/yt-dlp/releases

Get `yt-dlp.exe` from the top-most release.

## Installing the programs

The above downloads aren't installers, they're just EXE files that you run on the command line. If you try to double-click one of the EXEs, it will pop up a command line window and just disappear since you didn't tell it what to do.

To "install" them, you have to copy them to somewhere handy, then modify your Windows "Path" variable to point to their folders.

1. Go to C:\Users\[YourLogin] and make a folder called "Programs". (Or make this folder wherever you want. Just don't make it in a folder that requires admin rights to edit.)
2. Unzip `ffmpeg-n5.0-latest-win64-gpl-shared-5.0.zip`. (It will extract into a folder with the same name, so don't accidentally create *another* containing folder, which is something 7-zip can do if you tell it to. If you open the folder, you should see a "bin" folder in it.)
3. (Optional) Rename the unzipped folder to just "ffmpeg". In the future, you can overwrite these files with a new version and not have to change your Path variable again.
4. Move that whole folder to the "Programs" folder.
5. Click on the start menu and start typing "environment". After typing even just "env" you should see "Edit the system environment variables". Click that. It opens the "System Properties" window. (See: https://i.imgur.com/Rtl6SqP.png for a screenshot of steps 6 through 10)
6. Towards the bottom click "Environment Variables...". This opens a new window named "Environment Variables".
7. The upper half of the window says "User variables for [YourName]". In that section double-click on the one named "Path". This will open yet another window named "Edit environment variables".
8. In the "Edit environment variables" window, click the "Browse..." button.
9. Navigate the tree to "C:\Users\[YourLogin]\Programs\ffmpeg\bin". With that folder highlighted, click OK.
10. (Optional) Click "Browse..." again and add just "C:\Users\[YourLogin]\Programs" to the list. If you downloaded youtube-dl and/or yt-dlp, copy those EXEs to the Programs folder. Don't put them inside a subfolder.

Your folder should look something like this: https://i.imgur.com/zQ5zHbd.png

## Testing the install

1. Open a new command line. (WindowsKey-R -> cmd -> OK)
2. Type `where ffmpeg` and press Enter.
3. (Optional) Type `where youtube-dl` or `where yt-dlp`.
4. Type `ffmpeg -version` and you should see the version and build info.

You should see the path to the EXE. If you typed it wrong or it's not setup correctly you will see output saying it couldn't be found: https://i.imgur.com/xWqM5Gv.png
