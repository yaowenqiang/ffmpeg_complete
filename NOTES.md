> sw_vers
> brew install ffmpeg

> ffprobe
> ffplay

> ffprobe filters.mp4
> ffprobe -v filters.mp4 -show_format
> ffprobe -v filters.mp4 -show_format -show_streams
> ffprobe -v filters.mp4 -show_format -show_streams -print_format json
> ffprobe -v filters.mp4 -show_streams -print_format json -select_streams v
> ffprobe -v filters.mp4 -show_streams -select_streams v

> ffprobe -v error  005\ Filters.mp4  -show_streams  -select_streams v -show_entries stream=codec_name


> ffprobe -v error  005\ Filters.mp4  -select_streams v -show_entries stream=codec_name

> ffprobe -v error  005\ Filters.mp4   -select_streams v -show_entries stream=codec_name   -print_format default=noprint_wrappers=1

> ffprobe -v error  005\ Filters.mp4   -select_streams v -show_entries stream=codec_name   -print_format default=noprint_wrappers=1:nokey=1


> ffprobe -v error  005\ Filters.mp4   -show_entries format=format_long_name  -print_format default=noprint_wrappers=1:nokey=1


> ffprobe https://test-videos.co.uk/vids/bigbuckbunny/webm/vp9/1080/Big_Buck_Bunny_1080_10s_5MB.webm

> https://test-videos.co.uk/

> ffprobe  -v error https://test-videos.co.uk/vids/bigbuckbunny/webm/vp9/1080/Big_Buck_Bunny_1080_10s_5MB.webm -show_format -show_streams -print_format json
