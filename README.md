# `ffmpeg_debug_qp`

Authors: Werner Robitza, Steve Göring, Pierre Lebreton, Nathan Trevivian

Synopsis: Prints QP values of input sequence on a per-frame, per-macroblock basis to STDERR.

# Requirements


## UNIX platform

For building

- libavdevice, libavformat, libavfilter, libavcodec, libswresample, libswscale, libavutil
- C compiler

For example on Ubuntu:

    sudo apt install libavdevice-dev libavformat-dev libavfilter-dev libavcodec-dev libswresample-dev libswscale-dev libavutil-dev
    sudo apt install build-essential pkg-config     


## Windows platform

For building

- Visual Studio >= 2015 with C/C++ compiler installed with 64 bit support 
- Depending libraries (FFmpeg) are provided along the project, therefore no extra libraries are needed.


Please note that building the tool on windows is not necessary, as pre-build binaries can be found in the repository. See the archive: `build\bin.7z`. 


# Supported scenarios

Supported input:

- MPEG-2
- MPEG-4 Part 2
- H.264 / MPEG-4 Part 10 (AVC)

Supported formats:

- MPEG-4 Part 14
- H.264 Annex B bytestreams


# Build the tool

## UNIX platform

run the command `make` 

## Windows platform

Note that a binary is available without needing to compile the project. It can be found in the archive `build\bin.7z`. 

- Open the solution file "ffmpeg-debug-qp.sln" which can be found in `build\ffmpeg-debug-qp\`
- Make sure to compile in release mode (See the dropdown on the top menu bar. This is not necessary per-se, but beneficial for speed at runtime)
- Build the tool ctrl+shift+B
- The binary will be available in `build\bin\`, required DLL files can be found in the 7zip archive which can be found in `build\bin.7z`
- Copy DLL and binary to the root of the folder `ffmpeg-debug-qp` so depending scripts such as `parse-qp-output.py` can find the binary. 


# Usage

The main tool is a python library that first calls to ffmpeg-debug-qp and then parses and outputs the results. See example.py for more.

For help:

    python3 extract.py -h

To run:

    python3 extract.py -f -of json -m input.mp4 output_file.json

This produces a JSON file describing a list of frames and each of their macroblocks in the format:

```
  [
      {
          "frameType": "I", 
          "frameSize": 7787, 
          "qpAvg": 26.87280701754386, 
          "qpValues": [
              {
                  "qp": 25, 
                  "type": "i", 
                  "segmentation": "", 
                  "interlaced": ""
              }, 
              {
                  "qp": 26, 
                  "type": "i", 
                  "segmentation": "", 
                  "interlaced": ""
              }, ...
```

The frame and macroblock types are as per ffmpeg debug information. Same goes for segmentation and interlaced values.

For example outputs, see:

* Line-delimited JSON
  * [Averages only](examples/example-avgs.ldjson)
  * [Macroblock data](examples/example-mbdata.ldjson)
* JSON
  * [Averages only](examples/example-avgs.json)
  * [Macroblock data](examples/example-mbdata.json)
* CSV
  * [Averages only](examples/example-avgs.csv)
  * [Macroblock data](examples/example-mbdata.csv)

# Acknowledgement

This code is based on:

- the code from [Fredrik Pihl](https://gist.github.com/figgis/ea9ac513cdd99a10abf1)
- which is adapted from the code example `demuxing_decoding.c` by Stefano Sabatini

See also [this thread](https://ffmpeg.org/pipermail/libav-user/2015-May/008122.html) on the libav-user mailing list.

Test video part of Big Buck Bunny (c) copyright 2008, Blender Foundation / www.bigbuckbunny.org

# License

MIT License

Copyright (c) 2016-2020 Werner Robitza, Steve Göring, Fredrik Pihl, Stefano Sabatini, Nathan Trevivian

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

FFmpeg libraries are licensed under the GNU Lesser General Public License, version 2.1.
