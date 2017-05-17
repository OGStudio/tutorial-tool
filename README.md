* [Overview](#overview)
* [Dependencies](#deps)
* [Installation](#installation)
  * [Install tutorial-tool under Windows](#install-windows)
  * [Install tutorial-tool under Linux](#install-linux)
  * [Install tutorial-tool under macOS](#install-macos)
* [Examples](#examples)
  * [Create video tutorial for Blender installation](#example-blender)
* [Tips and tricks](#tips)

<a name="overview"/>

Overview
========

**tutorial-tool** is a simple video editing tool to automates creation of tutorials
by combining text and video

Here's a preview:

![Preview](https://github.com/OGStudio/tutorial-tool-readme/example/video.gif)

<a name="deps"/>

Dependencies
============
* [Python](http://python.org)

  tutorial-tool is a small Python application.

* [Image magick](http://imagemagick.org)

  tutorial-tool uses Image magick to produce still images that describe
  tutorial steps. Still images are generated from a step's text
  and background image.

* [MLT](http://mltframework.org)

  tutorial-tool uses MLT to combine still images and video parts into
  single video.

<a name="installation"/>

Installation
============

<a name="install-windows"/>

Install tutorial-tool under Windows
-----------------------------------
TODO
1. Install MSYS2?
2. Install Image magick
3. Install MLT

<a name="install-linux"/>

Install tutorial-tool under Linux
---------------------------------
TODO
1. Install Image magick
2. Install MLT

<a name="install-macos"/>

Install tutorial-tool under macOS
---------------------------------

1. Install Homebrew

   ![Screenshot](https://github.com/OGStudio/tutorial-tool-readme/readme/install-macos-01.png)

   Install [Homebrew](https://brew.sh/) by running the following command:

   `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

2. Install Image magick
 
   ![Screenshot](https://github.com/OGStudio/tutorial-tool-readme/readme/install-macos-02.png)

   Install Image magick by running the following command:

   `brew install imagemagick`

3. Install MLT
 
   ![Screenshot](https://github.com/OGStudio/tutorial-tool-readme/readme/install-macos-03.png)

   Install MLT by running the following command:

   `brew install mlt`

4. Install tutorial-tool

   ![Screenshot](https://github.com/OGStudio/tutorial-tool-readme/readme/install-macos-04.png)

   Download it from GitHub and move to desired location.

Example workflow
================
1. Create one or several videos of something you want to teach

   You can do it with software like [OBS Studio](https://obsproject.com).

2. Convert the video with MLT to 25 FPS

   You can do it with the following command:
   > $ melt -verbose -profile atsc_720p_25 **source_video.mp4** -consumer avformat:**destination_video.mp4** vcodec=libx264

   We will use this [sample video](example/install_blender.mp4).

1. Create background image with 1280x720 resolution

   You can do it with software like [GIMP](http://gimp.org).
   
   We will use this [sample image](example/bg.png).

1. Prepare the script to build final video

   We will use this script:
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

     Specifies background **image** to use for text

   * `text [seconds] [text]`

     Specifies **text** to render for desired number of **seconds**

   * `video [seconds_start]:[seconds_end] [video_file]`
     
     Specifies **video_file** part that starts at **seconds_start** and ends at **seconds_end**

1. Bake the video

   Since **tutorial-tool** itself only prints BASH commands for you to execute,
   you should run it like this:
   > /path/to/tutorial-tool /path/to/script **| sh**

1. You get the following video as a result in your temporary directory

   Under Linux the final video is in
   > /tmp/tutorial-tool-cache/video.mp4

   Here is how it looks like: [YouTube video](https://youtu.be/ScwXSJpIXpQ)

<a name="deps"/>

Tips and tricks
===============

1. Save video frames, one for each second

   `
   mkdir frames
   `

   `
   ffmpeg -i video.mp4 -vf scale=640:-1:flags=lanczos,fps=1 frames/f%03d.png
   `

1. Convert video frames to GIF

   `convert -loop 0 frames/f*.png output.gif`

