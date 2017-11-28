# Matroska Chapters


```
ffmpeg -f lavfi -i "smptebars=s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0.1*sin(1000*2*PI*t):d=5:s=48000:c=stereo" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f lavfi -i "color=s=2x2:r=1:d=6,format=gray,geq=lum=4-N,datascope=s=24x12,crop=6:12:10:0,scale=iw*30:ih*30:flags=neighbor,pad=720:480:(720-iw)/2:(480-ih)/2,fps=25" -f lavfi -i "sine=r=48000:frequency=1:beep_factor=400:duration=5" -f lavfi -i "nullsrc=s=720x480:r=25:d=5,geq=random(1)/hypot(X-cos(N*0.07)*W/2-W/2\,Y-sin(N*0.09)*H/2-H/2)^2*1000000*sin(N*0.02):128:128,geq=r='X/W*r(X,Y)':g='(1-X/W)*g(X,Y)':b='(H-Y)/H*b(X,Y)'" -f lavfi -i "anoisesrc=colour=pink:d=5:r=48000,tremolo=f=0.1:d=0.9" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f srt -i chapters_test.srt  -filter_complex "[0:v][1:a][2:v][3:a][4:v][5:a][6:v][7:a][8:v][9:a]concat=n=5:v=1:a=1" -c:v ffv1 -c:a flac -c:s srt chapters_test.mkv
```


## MKXToolNix GUI

### Creating Chapters

Open MKVToolNix. Navigate to the *Multiplexer* tab. 


![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters01.png "Multiplexer Tab")


Drop in **chapters_test.mkv**

