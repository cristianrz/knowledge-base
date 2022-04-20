# yt-dlp

## Download music

```bash
yt-dlp.exe \
	-x \
	--audio-format mp3 \
	--ffmpeg-location "C:\ProgramData\ffmpeg\bin\ffmpeg.exe" \
	--output '%(title)s.%(ext)s' \
	"https://www.youtube.com/watch?v=mzB1VGEGcSU"
```
