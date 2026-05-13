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

> ffplay -v error filters.mp4

> ffplay -v error filters.mp4 -noborder -x 600 -y 600
> ffplay -v error filters.mp4 -noborder -y 600
> ffplay -v error filters.mp4 -noborder -y 600 -top 0 -left 0

> ffplay -v error filters.mp4 -noborder -y 600 -fs # fullscreen

disable the audio

> ffplay -v error filters.mp4 -noborder -y 600 -fs -an


disable the video
> ffplay -v error filters.mp4 -noborder -y 600 -fs -vn

> ffplay -v error filters.mp4 -noborder -y 600 -fs -vn -showmode waves

> ffplay -v error filters.mp4 -noborder -loop 0

> space to pause/resume
> m to mute/unmute
> f/double click left to toggle fullscreen
> 9 to volume down
> 0 to volume up

> / to volume down
> * to volume up

> w to cycle modes

> rdft 
> s to frame step 

> <- back 10s
> -> forward 10s

> down arrow back 1 min
> up arrow forward 1 min
> esc to quit

> yuv

image resolution

HD(1920x1080)
UHD(3840x2160)
4K(4096x2160)


audio channels

mono
sterer
5.1
7.1

video

Video Frame Rate(Frame Per Second)

23.98
34.25
29.97
30
50
59.94
60

Video Compression

works by removing redundant information
Spatial redundancey - within a frame
Temporal redundancey - across frames

Codecs and Containers

codec

H.264
H.265
VP9
Prores
DNxHD

Audio codec

PCM
AAC
Mp3

What is a container?

package/wrapper for the media essence
File format
How the media data is organized inside a file

Container examples

MP4
MXF
qt/mov(quick time)
MKV

audio

wav
m4a


MP4
  h.264
  aac

MXF
  DNxHD
  PCM
MOV
  Prores
  PCM

Transcoding

What is transcoding

From one codec to another

example, prores to h.264


transmuxing

from one container to another

example: mxf to mp4

Thumbnail generation


frame rate conversion

support different television standards(pal/ntsc)

higher fps: preserve slow motion quality for editing
lower fps: playback/streaming

bitate conversion

GOP(group of pictures)

1-frame only: good for editing
long gop: good for compression and streaming

overlay

channel logo
watermark
graphics
lower-third

timecode


audio volume adjustment

amplify
normalize


audo resampling

waveform


ffmpeg Architecture

input ->unpack -> uncompress -> filter -> compress -> pack -> output


input(mxf(prores,pcm))
demuxer (libavformat) -> uncompess
decoder (libavcodec)
filer1
filer2
filter3(libavfilter)
encoder(libavcodec)
muxer(liaavformat)
output(mp4, h.264, aac)

protocols,devices, and formats

ffmpeg

-f <input device or format>
-i <input-protocol>:<input-identifier>
... <filters and other optinos> ...
-f <outpuf device or format>
<output-protocol>:<output-identifier>

Listing supported Protocols, Devices and Formats

> ffmpeg -protocols
> ffmpeg -devices
> ffmpeg -formats

Files

> ffmpeg -f mov -i file:input.mov ... /tmp/output.mp4
> ffmpeg  -i sub/dir/input.mov ... ../relative/path/output.mp4
> ffmpeg  -i input.mov ... -f mxf output-without-extension

Networks

http(s), ftp,rtp,...

> ffmpeg -i https://some-server/media.mp4  ... disk-file.mp4
> ffmpeg -i rtp://localhost:1234 ... ftp://abc.com/123/out.mxf

Pipes, stdin, stdout

> ffmpeg -f mov -i - ... -f mxf -
> ffmpeg -f mov -i pipe:0 ... -f mxf pipe:1

Generated inputs

- useful for testing

> ffmpeg -f lavfi -i testsrc=duration=1:size=1920x1080 ...
> ffmpeg -f lavfi -i color=color=red ...

> lavfi stands for libfav input


Screen capture


Windows: ffmpeg -f gdigrab -i desktop ...
macOS: ffmpeg -f avfoundation -i "1" ...

> ffmpeg -f avfoundation -i "0" output.mp4 

Linux: ffmpeg -f x11grab -i $DISPLAY ...

Webcam capture

windows: ffmpeg -f dshow -i video="usb2.0 HD UVC WebCam" ...
macOS: ffmpeg -f avfoundation -i "default" ...
linux: ffmpeg -f v412 -i /dev/video0 ...


Microphone capture


Windows: ffmpeg -f dshow -i audio="Microphone (Realtek)" ...
macOS: ffmpeg -f avfoundation -i "2"
Linux: ffmpeg -f alsa -i hw:1 ...


Stream Selection

What is a stream?


+ Video or audio track
+ Usually one video stream
+ One or more audio streams


Inspectintg streamings with ffprobe




ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq '.streams | length'

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq -r '.streams[].codec_name' // 无双引号

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq '.streams[].codec_name' // 有双引号

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq -r '.streams[] | {codec_name}' // 带上键名

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq -r '.streams[] | {codec_long_name}' // 带上键名

> ffprobe 005\ Filters.mp4 -v error -show_format -show_streams -print_format json | jq -r '.streams[] | {codec_long_name,codec_type}'



Stream Selection

+ Requierd for filters and outputs (-map)
+ Example: extract first audio stream

Stream selection syntax:

<input-index>:<stream-type>:<stream-index>

> ffmpeg -i background.mp4 -i overlay.png -i sounds.wav ...

<input-index>:<stream-index>

Example: 0:1,1:0,...
Particular stream in the input

<input-index>:<stream-type>
Example: 0:v,1:a,...

inplies all streams of that type of the input

<input-index>:<stream-type>:<stream-index>
Example: 0:v:0,0:a:2,...
Stream index is in reference to the list of streams of specified type only



> ffmpeg -v error -y -i multitrack.mp4 -to 1 multitrack-1s.mp4


> ffprobe -v error -show_format -show_streams -print_format json filters-1s.mp4 | jq '.streams[] | {duration,codec_name}'


By default ,will only keep one video and one audio stream




> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0 multitrack-1s.mp4

> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:v multitrack-1s.mp4 // just the video

> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:0 multitrack-1s.mp4 // just the video

> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:a multitrack-1s.mp4 // just the audio

> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:1 multitrack-1s.mp4 // just the audio

> ffmpeg -v error -y -i multitrack.mp4 -to 1 -map 0:a:0 multitrack-1s.mp4 // just the audio

> ffmpeg -v error -y -i multitrack.mp4 -i second-input.mp4  -to 1 -map 1:v:0 -mpa 0:a:1 multitrack-1s.mp4 // multiple input files

Filters

What is a filter?


Changes the media in some way
Usually works on either video or audio
Most common filters come from libavfilter

Filter Options

Syntax

- filter=key1=value1:key2=value2...

Example: 
- scale=width=1920:height=1080
- scale=w=1920:h=1080
- scale=1920:1080

scale - 1 input 1 output
split - 1 input , 2 output
overlay - 2 inputs, 1 output

labeling inputs and outputs

+ readable names
+ Useful for forming non-linear filter graphs
+ Enclosed in square brackets [ a_label ]
+ Stream selecters can be used as input labels, e.g. [0:v],[1:a:2] etc.
+ syntax:
  + [in_1][in_2] ... filter_name=<options...>[out_1][out_2]


> split=2[sd_in][hd_in]
> [bg][ol]overlay

filter chain

+ Sequence of multiple filters
+ Each filter connected to the next in the chain
+ Filters separaged with comma ','
+ Example
  + filter1=k11=v11:k22=v22,filter2=k21=v22,filter3=k23=v31:k32=v32

Filter Graphs

+ Multiple filter chains
+ Can be non-linear
+ Multiple inputs/outputs
+ Chains separated with semicolon ';'
+ Can be specified with -vf, -af, or -filter_complex





Filter Graphs(-vf)

+ Simple video processing graphs with single input and output

> ffmpeg -v error -y -i bullfinch.mp4 -vf "split[bg][ol];[bg]scale=width=1920:height=1080,format=gray[bg_out];[ol]scale=-1:480;hflip[ol_out];[bg_out][ol_out]overlay=x=w-w:y=(H-h)/2" ol.mp4

> ffmpeg -v error -y -i bullfinch.mp4 -vf "split[bg][ol];[bg]scale=width=1920:height=1080,format=gray[bg_out];[ol]scale=-1:480,hflip[ol_out];[bg_out][ol_out]overlay=x=W-w:y=(H-h)/2" ol.mp4

Filter Graphs(-af)

+ Simple audio processing graphs with single input and output

> ffmpeg -y -i four_channel_stream.wav -af "asplit=2[voice][bg];volume=volume=2,pan=mono|c0=c0+c1[voice_out];[bg]volume=volume=0.5,pan=mono|c0=c2+c3[bg_out];[voice_out][bg_out]amerge=inputs=2" audio_out.wav

Filter Graphs(-filter_complex)

+ More versatile
+ Multiple inputs and outputs
> Audio and Video


> ffmpeg -v error -y -i bullfinch.mp4 -i ffmpeg-logo.png -filter_complex "[1:v]scale=-1:200[small_logo];[0:v][small_logo]overlay=x=W-w-50:y=H-h-50,split=2[sd_in][hd_in];[sd_in]=scale=-2:480[sd];[hd_in]scale=-2:1080[hd];[0:a]pan=stereo|FL=c0+c2|FR=c1+c3[stereo_mix]" -map [sd] sd.mp4 -map [hd] hd.mp4 -map [stereo_mix] stereo_mix.mp3


Encoding

Choosing a codec

+ Compression
+ Quality vs Size
+ Stream vs post-production
+ Target application
+ Compatibility

Encoder options

+ Global
  + profile
  + bitrate
  + GDP size
+ Private
  + x264-params


Examples

> ffprobe -v error bullfinch.mp4 -select_streams v -show_entries stream=codec_name -print_format default=noprint_wrappers=1

> ffmpeg -v error -y  -i bullfinch.mov  transcoded.mxf
> ffprobe -v error transcoded.mxf -select_streams v -show_entries stream=codec_name -print_format default=noprint_wrappers=1

> ffmpeg -v error -y  -i bullfinch.mov  transcoded.mp4
> ffprobe -v error transcoded.mp4 -select_streams v -show_entries stream=codec_name -print_format default=noprint_wrappers=1

> ffmpeg -v error -y  -i bullfinch.mov -vcodec libx264 -g 30 transcoded.mxf
> ffmpeg -encoders 

> ffmpeg -v error -y  -i bullfinch.mov -vcodec libvpx-vp9  transcoded.mxf

> ffmpeg -v error -y  -i bullfinch.mov -vcodec libvpx-vp9  transcoded.mp4
> ffprobe -v error transcoded.mp4 -select_streams a -show_entries stream=codec_name -print_format default=noprint_wrappers=1

> $ ffmpeg -v error -y  -i bullfinch.mov -vcodec libvpx-vp9  -acodec libmp3lame  transcoded.mp4

> ffprobe -v error transcoded.mp4 -select_streams a -show_entries stream=codec_name -print_format default=noprint_wrappers=1

H.264/AVC(advanced video coding)

Encoder 

+ libx264

Profiles

+ profile: baseline / main / high

Rate control

+ CRF: Constant quality, variable bitrate
+ Two-pass ABR: Variable quality,constant bitrate

CRF: target a quality

- crf: 0-51(high to low),default is 23

> ffprobe -v error bullfinch.mov -select_streams v -show_entries stream=codec_name,bit_rate -print_format default=noprint_wrappers=1


> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 transcoded.mp4 

> ffprobe -v error transcoded.mp4 -select_streams v -show_entries stream=codec_name,bit_rate -print_format default=noprint_wrappers=1

> ffplay -v error -an transcoded.mp4 

ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -crf 45 transcoded.mp4

ABR: Target a bitrate

- b:v:bitrate

> ffprobe -v error transcoded.mp4 -select_streams v -show_entries stream=codec_name,bit_rate -print_format default=noprint_wrappers=1


> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M transcoded.mp4

> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M -pass 1 -f null /dev/null
> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -b:v 2M -pass 2 transcoded.mp4


Preset: speed vs compression

+ ultrafast
+ superfast
+ veryfast
+ faster
+ fast
+ medium(default)
+ slow
+ slower
+ veryslow
+ placebo

> du -sh transcoded.mp4
> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -preset ultrafast transcoded.mp4
> ffmpeg -v error -y -i bullfinch.mov -vcodec libx264 -preset slow transcoded.mp4



Streaming playback

> .m3u8

+ RTMP(RealTime Message Protocol)
  + Based on TCP
  + Low-latency
  + Originally developed by Macromedia (acquired by Adobe)
  + Adoby Flash Player
  + Hugrly popular until recently
  + Not being updated any more
  + Modern codecs not supported
  + less popular now
  + Requires extra browser plugin
  + Flash support dropped
  + Requires RTMP server
+ HTTP
  + Widest reach
  + TCP based
  + Unlikely to be blocked anywhere
  + No separate streaming server required
  + HTML5 video
  + MSE(Media Source Extensions)
  + HLS, MPEG-DASH
  + Javascript players
  + Most popular
  + No extra browser plugin needed
  + No Separated server required
  + http injecting(not used much)
  + latency higher
+ SRT
  + Secure Reliable Transport
  + UDP based
  + Faster then RTMP
  + Reliable on unpredicatable networks
  + almost no adoption
  + UDP not supported in browsers
  + Becoming popular
  + Reliable, low-latency

FFmpeg: The Swiss Army Knife of internet Streaming

Progressive Download

Singel-file Media

+ Not segmented
+ Easier to handle
+ Native browser support(HTML5 <video/> and <audio>)
+ Copy
+ Downlaod
+ Send to other services

Container formats

+ mp4
+ WebM
+ Ogg

How playback works

+ What is the file format? Do i know how to parse the file?
+ Is it audio or video or both?
+ What is the video width and height?
+ Codecs
+ If i have to seek to the 123rd second, where this file can i find the media data for that particular time?

Th index

+ Lookup table
+ Where to find media data of a time or frame
+ Difficult for the encoder to know the contents of the index beforehand
+ That is why it is usually written at the end

Structure of MP4

+ Similar to Apple Quicktime File Format(QT/MOV)
+ Hierarchical
+ Atom/Box

ftyp mdat moov ->non-fast-started
ftyp moov mdat ->fast-started mp4

http range request
partial content


Questions

+ How to check fast-started-ness?
+ How to fast-start?
  + Tools
  + ffmpeg

> ffmpeg -y -f lavfi -i testsrc=duration=5 test.mp4
> ffmpeg -v trace -i test.mp4 2>&1 | grep -e type:\'mdat\' -e type:\'moov\'

[in#0 @ 0xab8c18000] type:'mdat' parent:'root' sz: 24100 48 26395
[in#0 @ 0xab8c18000] type:'moov' parent:'root' sz: 2255 24148 26395

no fast-play

> ffmpeg -i test.mp4 -movflags +faststart -c copy test-fast-started.mp4

[in#0 @ 0x75cc18000] type:'moov' parent:'root' sz: 2255 40 26395
[in#0 @ 0x75cc18000] type:'mdat' parent:'root' sz: 24100 2303 26395

fast-play, moov first, mdat second


Adaptive Streaming

+ Multiple qualities(resolution/bitrate)
+ Player can adapt dynamically based on current conditions
+ Segmented

1280x720 ->4 Mbps
854x480 ->2 Mbps
426x240 ->500 kbps

Adaptive Resolution

HLS & DASH

HLS

+ HTTP Live Streaming
+ developed by apple
+ REleased in 2009
+ Most popular streaming format
+ Proprietary(专有的)

DASH

+ Dynamic Adaptive Streaming over HTTP
+ MPEG-DASH
+ First international standard in 2012
+ Collaboration of many companies

Browser Support

HLS

+ Safari:native
+ Others: MSE(Media Source Element)

DASH

+ MSE


Codec

HLS

+ H.264/AVC
+ H.265/HEVC

DASH

+ Codec agnostic
+ H.264, H.265, vp9 ...

Container

HLS

+ TS
+ fMP4


DASH

+ fMP4

Manifest

HLS

+ M3U8
  + Media playlist
  + Master playlist

DASH

+ MPD
  + XML


encoding Considerations

Frame types - I(iframe),P(predicted pictures),B

Group of Pictures - GOP

closed gop
open gop structure

segments in adaptive streaming

continuous stream

Segments: Switching and Duration

encoding efficiency: short segments
encoding efficiency: long segments

short segments => more I-frames => less compression 
long segments => fewer I-frames => Better compression 

segment Duration: Recommendations

+ Apple: 6 seconds
+ 2-4 seconds is usually good

codecs

| codec | Compatibility | HLS | DASH |
| --------------- | --------------- | --------------- | --------------- |
| H.264 | high | yes | yes |
| H.265 | Medium | yes | yes |
| VP9 | High | no | yes |

containers

ts(HLS v3)
fmp4(HLS v4+, DASH)

muxing audio and video

+ muxed together
+ separate audio, separate video


HLS or DASH?

| Format | Compatibility | Brower support |
| ------------- | -------------- | -------------- |
| HLS | High | JS+MSE/Safari |
| DASH | High | JS+MSE |


HLS and DASH

examples 

HLS, TS, A + V



HLS, TS

ffmpeg.exe -y -i ../nature.mp4 -to 10 \
-filter_complex "[0:v]fps=30,split=3[720_in][480_in][240_in];[720_in]scale=-2:720[720_out];[480_in]scale=-2:480[480_out];[240_in]scale=-2:240[240_out]" \
-map "[720_out]" -map "[480_out]" -map "[240_out]" -map 0:a -map 0:a -map 0:a \
-b:v:0 3500k -maxrate:v:0 3500k -bufsize:v:0 3500k \
-b:v:1 1690k -maxrate:v:1 1690k -bufsize:v:1 1690k \
-b:v:2 326k -maxrate:v:2 326k -bufsize:v:2 326k \
-b:a:0 128k \
-b:a:1 96k \
-b:a:2 64k \
-x264-params "keyint=60:min-keyint=60:scenecut=0" \
-var_stream_map "v:0,a:0,name:720p-4M v:1,a:1,name:480p-2M v:2,a:2,name:240p-500k" \
-hls_time 2 \
-hls_list_size 0 \
-hls_segment_filename adaptive-%v-%03d.ts \
-master_pl_name adaptive.m3u8 \
adaptive-%v.m3u8 

HLS, MP4

DASH, MP4

HLS + DASH, FMP4















