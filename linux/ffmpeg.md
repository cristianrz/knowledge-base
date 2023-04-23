# ffmpeg

## H.265 to H.264

```bash
ffmpeg \
	-i "$1" \
	-c:v libx264 \
	-c:a aac \
	-preset slow \
	-crf 20 \
	-b:a 160k \
	-vf format=yuv420p \
	-movflags +faststart \
	-vf subtitles="$1" \
	"$1".mp4
```

## Image based subtitles

```bash
ffmpeg -i "$i" -filter_complex "[0:v][0:s]overlay[v]" -map "[v]" -map 0:a <output options> "$i".new
```

```bash
ffmpeg -i "Adventure Time S03E01 Conquest of Cutss.mkv.invalid" -i 'Adventure Time S03E01 Conquest of Cutss.srt' -map 0:0 -c:v:0 copy -map 0:1 -c:a:1 copy -map 1 -c:s mov_text  "Adventure Time S03E01 Conquest of Cutss.mp4"
```

works but leaves old sub

```bash
ffmpeg -i Adventure\ Time\ S03E01\ Conquest\ of\ Cutss.mkv.invalid -i Adventure\ Time\ S03E01\ Conquest\ of\ Cutss.srt -map 0:0 -c:v copy -map 0:1 -c:a copy -map 1 -c:s mov_text Adventure\ Time\ S03E01\ Conquest\ of\ Cutss.mp4
```

## Embed SRT

```bash

filename=$(basename -- "$fullfile")
extension="${filename##*.}"
filename="${filename%.*}"

ffmpeg \
	-i "$filename".mkv \
	-i "$filename".srt \
	-map 0:0 -c:v copy \
	-map 0:1 -c:a copy \
	-map 1 -c:s mov_text -metadata:s:s:0 language=eng \
	-map_chapters -1 \
	"$filename".mp4
```