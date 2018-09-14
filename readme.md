# youtuble-dl usage


## Ref.
- [youtube-dl](https://github.com/rg3/youtube-dl)
- [jq](https://github.com/stedolan/jq)
- [ffmpeg](https://github.com/FFmpeg/FFmpeg)
- [mpg123](https://www.mpg123.de)


## dowload yt video convert to mp3
```
youtube-dl -f bestaudio --extract-audio --audio-format mp3 --audio-quality 0 --embed-thumbnail --add-metadata -o "%(title)s.%(ext)s" "{{yt_url}}"
```

## stream yt video
```
youtube-dl -f bestaudio --quiet -o - "{{yt_url}}" | ffmpeg -i - -vn -codec:a libmp3lame -q:a 0 -hide_banner -loglevel panic -f mp3 - | mpg123 -v -
```

## stream yt play list
```
youtube-dl -j --flat-playlist --playlist-random "{{yt_list_url}}" | jq -r ".id" | sed -e "s/^/https:\/\/www.youtube.com\/watch?v=/" | xargs -n 1 -I url sh -c "youtube-dl -f bestaudio --quiet -o - url | ffmpeg -i - -vn -codec:a libmp3lame -q:a 0 -hide_banner -loglevel panic -f mp3 - | mpg123 -v -"
```