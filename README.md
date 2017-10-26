#logitech c920 to vlc stream in windows
## Add vlc to windows path

```
vlc --fullscreen dshow:// :dshow-vdev="HD Pro Webcam C920" :dshow-size="1920x1080" :dshow-aspect-ratio=16\:9 :dshow-chroma="MJPG" :no-dshow-config
```