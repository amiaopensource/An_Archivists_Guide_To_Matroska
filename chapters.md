# Matroska Chapters

Create **chapters_test.mkv** with the following FFmpeg string. NOTE: This string will only work if **chapters_test.srt** is in the working directory.

```
ffmpeg -f lavfi -i "smptebars=s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0.1*sin(1000*2*PI*t):d=5:s=48000:c=stereo" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f lavfi -i "color=s=2x2:r=1:d=6,format=gray,geq=lum=4-N,datascope=s=24x12,crop=6:12:10:0,scale=iw*30:ih*30:flags=neighbor,pad=720:480:(720-iw)/2:(480-ih)/2,fps=25" -f lavfi -i "sine=r=48000:frequency=1:beep_factor=400:duration=5" -f lavfi -i "nullsrc=s=720x480:r=25:d=5,geq=random(1)/hypot(X-cos(N*0.07)*W/2-W/2\,Y-sin(N*0.09)*H/2-H/2)^2*1000000*sin(N*0.02):128:128,geq=r='X/W*r(X,Y)':g='(1-X/W)*g(X,Y)':b='(H-Y)/H*b(X,Y)'" -f lavfi -i "anoisesrc=colour=pink:d=5:r=48000,tremolo=f=0.1:d=0.9" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f srt -i chapters_test.srt  -filter_complex "[0:v][1:a][2:v][3:a][4:v][5:a][6:v][7:a][8:v][9:a]concat=n=5:v=1:a=1" -c:v ffv1 -c:a flac -c:s srt chapters_test.mkv
```

This file will be used throughout the Chapters exercise.

## MKXToolNix CLI

## MKXToolNix GUI

### Creating Chapters

Open MKVToolNix. Navigate to the *Multiplexer* sidebar tab. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters01.png "Multiplexer Tab")

Drop in **chapters_test.mkv**. This will populate the *Input* tab at the top of screen. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters02.png "Input tab")

In the *Source files:* section you should see only **chapters_test.srt**. If you see any
extra files remove them by right-clicking and selected *Remove File*. 

In the *Tracks, chapters, and tags:* section you should see a Video track, an Audio track, a Subtitles track, and some tags. 

Click on the *Output* tab at the top of screen.

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters03.png "Output tab")

In the *Chapters* section set *Generating chapters:* to "Chapters in fixed intervals" and *Interval:* to "5s". 

Rename the *Destination file:* to "chapters\_test\_5sec.mkv"

Press *Start multiplexing*. This will create the new file Matroska files with a new chapter every five seconds. 

Open **chapters\_test\_5sec.mkv** with VLC. Skip around the Chapters using *Playback -> Chapter*

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters04.png "Chapters in VLC")

Export the chapter info to an XML file using mkvextract

```
mkvextract chapters chapters_test_5sec.mkv > chapters_test_5sec.xml
```

Go back to MKVToolNix, Navigate to the *Chapter editor* sidebar tab.  Drop in **chapters_test_5sec.xml**. You should see five chapters 
appear in the window on the left-hand side. Clicking on any of the chapters will allow you edit information about the chapters.

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters05.png "Chapters Editor 5sec")

Edit the chapters in this window to make your own custom chapters for the file. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters06.png "Chapters Editor custom")

Once you're finished, save the file by clicking (at the top of the screen) *Chapter editor -> Save as XML file*. Save the file as **chapter\_test\_custom.xml**


![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters07.png "Save as XML file")

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters08.png "chapter_test_custom.xml")

Return to the *Multiplexer* sidebar tab. 

Drop in **chapters_test.mkv**

Navigate to the **Output** tab. 

Click the folder icon next to *Chapter file:* and select  **chapter\_test\_custom.xml**. 

Rename the *Destination file:* to "chapters\_test\_custom.mkv"

Press *Start multiplexing*. This will create the new file Matroska files with a new chapter with your custom chapter markings. Test it out in VLC!

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters09.png "Multiplex new MKV with custom chapters")


 














