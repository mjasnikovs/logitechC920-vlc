#logitech c920 to vlc stream in windows
## Add vlc to windows path

```
vlc --fullscreen dshow:// :dshow-vdev="HD Pro Webcam C920" :dshow-size="1920x1080" :dshow-aspect-ratio=16\:9 :dshow-chroma="MJPG" :no-dshow-config
```

# ffmpeg (Windows)
## show list of capture devices

```
ffmpeg -list_devices true -f dshow -i dummy
```

## list capture device options

```
ffmpeg -f dshow -list_options true -i video="C922 Pro Stream Webcam"
```

## Record segmented video
-segment_time 600 - video length in seconds

-segment_wrap 6 - video amount

```
ffmpeg -loglevel panic -hwaccel qsv -f dshow -video_size 1920x1080 -framerate 30 -vcodec mjpeg -i video="C922 Pro Stream Webcam" -codec:v libx264 -preset ultrafast -crf 24 -tune zerolatency -filter:v scale=1280:-1 -map 0 -f segment -segment_time 600 -segment_wrap 6 dvr_%04d.avi -codec:v copy -f nut - | ffplay -i -window_title "Krāsošanas kamera" -
```

## Pass video pipe to VLC

```
ffmpeg -hwaccel qsv -threads 1 -fflags nobuffer -f dshow -video_size 1920x1080 -framerate 24 -vcodec mjpeg -i video="C922 Pro Stream Webcam" -codec:v libx264 -preset ultrafast -crf 24 -tune zerolatency -filter:v scale=1280:-1 -map 0 -f segment -segment_time 600 -segment_wrap 6 dvr_%04d.avi -codec:v copy -f nut - | vlc --file-caching="0" -
```

## Pipe low latancy video
```
ffmpeg -loglevel panic -hwaccel qsv -threads 1 -fflags nobuffer -flags low_delay -strict experimental -f dshow -video_size 1280x720 -framerate 10 -pixel_format yuyv422 -i video="C922 Pro Stream Webcam" -codec:v libx264 -preset ultrafast -crf 24 -tune zerolatency -map 0 -f segment -segment_time 600 -segment_wrap 6 dvr_%04d.avi -codec:v copy -f nut - | ffplay -fflags nobuffer -flags low_delay -vf scale=1920x1080:flags=lanczos -window_title "kamera" -noborder -left 1920 -top 150 -fast -framedrop -
```