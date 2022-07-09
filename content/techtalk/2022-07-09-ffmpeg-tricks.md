---
title: "FFmpeg Tricks"
date: 2022-07-09
---

## Clip a portion of a video

The following command saves a five-second clip from `input.mkv` to
`output.mkv`.

```
ffmpeg -i input.mkv -ss 00:05:10.500 -t 00:00:05.000 output.mkv
```

- `-ss 00:05:10.500`: start clipping at hh:mm:ss.ms
- `-t 00:00:05.000`: clip a duration of hh:mm:ss.ms


## Remove black borders

The following steps consecutively detects and crops black borders
around a video file. This tutorial assumes the aspect ratio of the
video is the same across the whole video, i.e., it does not change,
i.e., no post-Inception Nolan films.

### Detect which areas to crop

```
ffmpeg -i input.mkv -ss 00:05:00 -t 1 -vf cropdetect -f null - 2>&1 | awk '/crop/ { print $NF }' | tail -1
```

The command above will utilize ffmpeg's cropdetect functionality to
detect which area to crop. The portion with `awk` and `tail` are for
ease of usage.

- `ffmpeg`: the command-line tool
  - `-i input.mkv`: the video file
  - `-ss 00:05:00`: start at the fifth minute
  - `-t 1`: run for one minute
  - `-vf cropdetect`: use cropdetect video filter
  - `-f null`: [use null muxer][null muxer]; don't produce output file
  - `-`: [use stdin/stdout][dash pipe] (depending on input/output stream)
- `awk '/crop/ { print $NF }'`: print the last column of lines
  containing the word "crop"
- `tail -1`: print only the last line

### Check if cropdetect was correct

Now check if the dimensions returned by cropdetect is correct. The
following command will play the video in the default media player
with the video cropped. (Don't forget to replace the crop dimensions
in the command below with yours.)

```
ffplay -vf crop=960:720:0:0 input.mkv
```

### Crop the video

If the crop dimension is correct, it's time for actually cropping the
video.

```
ffmpeg -i input.mkv -vf crop=<crop_dimension> -c:a copy input_cropped.mkv
```




[null muxer]: https://trac.ffmpeg.org/wiki/Null
[dash pipe]: https://stackoverflow.com/questions/63596219/what-does-dash-mean-as-ffmpeg-output-filename
