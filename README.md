
# tutorial-tool

`tutorial-tool` is a simple video editing tool to automate creation of
tutorials by combining text and video

Here's a preview of a result:

![Preview][preview]

# Table of contents

* [Dependencies](#deps)
* [Installation](#install)
* [Example](#example)
* [Tips and tricks](#tips)

<a name="deps"/>

# Dependencies

* [Python][python]
    * `tutorial-tool` is a small Python application
* [Image magick][imagemagick]
    * Is used to produce still images that describe tutorial steps. Still images are generated from a step's text and background image
* [MLT][mlt]
    * Is used to combine still images and video parts into single video

<a name="install"/>

# Installation

**macOS**

1. Install [Homebrew][brew]
    * `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
1. Install Image magick
   * `brew install imagemagick`
1. Install MLT
   * `brew install mlt`
1. Install tutorial-tool
   * Download it from GitHub and move to desired location


**TODO Windows**

1. Install MSYS2?
1. Install Image magick
1. Install MLT

**TODO Linux**

1. Install Image magick
1. Install MLT

<a name="example"/>

# Example

**Note**: you may use `mplayer` to get timestamps in seconds of original video

Here's the usual workflow of making a new tutorial video:

1. Create one or several videos of something you want to combine into a tutorial
    * You can do it with software like [OBS Studio](https://obsproject.com).
1. Convert the video with MLT to 25 FPS
    * `melt -verbose -profile atsc_720p_25 **source_video.mp4** -consumer avformat:**destination_video.mp4** vcodec=libx264`
1. Create background image with 1280x720 resolution
    * You can do it with software like [GIMP](http://gimp.org).
1. Prepare the script to build final video
1. Bake the video
    * `/path/to/tutorial-tool /path/to/script | sh`
    * **Note**: `tutorial-tool` only prints Bash commands for you to execute, so you have to redirect `tutorial-tool` output to the shell
1. Get the resulting video in your temporary directory

Here's an example script:

```
background bg.png
text 5 Let's install Blender
video 0:6 install_blender.mp4
text 5 Installing it with apt
video 6:26 install_blender.mp4
text 5 We're still installing it
video 26:56 install_blender.mp4
text 5 Congratulations! We just finished installng Blender
```

This script contains all supported language constructs:

* `background [image]`
    * Specifies background **image** to use for text
* `text [seconds] [text]`
    * Specifies **text** to render for desired number of **seconds**
* `video [seconds_start]:[seconds_end] [video_file]`
    * Specifies **video_file** part that starts at **seconds_start** and ends at **seconds_end**

Here is the resulting [video at YouTube][result].

<a name="tips"/>

# Tips and tricks

* Save video frames, one for each second
    * `mkdir frames`
    * `ffmpeg -i video.mp4 -vf scale=640:-1:flags=lanczos,fps=1 frames/f%03d.png`
* Convert video frames to GIF
    * `convert -loop 0 frames/f*.png output.gif`
* Crop video
    * `ffmpeg -i in.mp4 -vf crop=1024:768:0:0 -c:v libx264 -crf 0 -c:a copy out.mp4`

[preview]: https://github.com/OGStudio/tutorial-tool-readme/blob/master/example/video.gif
[python]: http://python.org
[imagemagick]: http://imagemagick.org
[mlt]: http://mltframework.org
[brew]: https://brew.sh
[sample]: example/install_blender.mp4
[result]: https://youtu.be/ScwXSJpIXpQ

