# Matroska Chapters

First we need a file to work with. **chapters_test.mkv** exists in the github repo, but it's more fun to create it yourself! Create the file with the following FFmpeg string. 

NOTE: This string will only work if **chapters_test.srt** is in the working directory.

```
ffmpeg -f lavfi -i "smptebars=s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0.1*sin(1000*2*PI*t):d=5:s=48000:c=stereo" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f lavfi -i "color=s=2x2:r=1:d=6,format=gray,geq=lum=4-N,datascope=s=24x12,crop=6:12:10:0,scale=iw*30:ih*30:flags=neighbor,pad=720:480:(720-iw)/2:(480-ih)/2,fps=25" -f lavfi -i "sine=r=48000:frequency=1:beep_factor=400:duration=5" -f lavfi -i "nullsrc=s=720x480:r=25:d=5,geq=random(1)/hypot(X-cos(N*0.07)*W/2-W/2\,Y-sin(N*0.09)*H/2-H/2)^2*1000000*sin(N*0.02):128:128,geq=r='X/W*r(X,Y)':g='(1-X/W)*g(X,Y)':b='(H-Y)/H*b(X,Y)'" -f lavfi -i "anoisesrc=colour=pink:d=5:r=48000,tremolo=f=0.1:d=0.9" -f lavfi -i "color=color=black:s=720x480:r=25:d=5" -f lavfi -i "aevalsrc=0:d=5:s=48000:c=stereo" -f srt -i chapters_test.srt -filter_complex "[0:v][1:a][2:v][3:a][4:v][5:a][6:v][7:a][8:v][9:a]concat=n=5:v=1:a=1" -c:v ffv1 -c:a flac -c:s srt -y chapters_test.mkv
```

This file will be used throughout the Chapters exercise.

## MKXToolNix CLI

## MKXToolNix GUI

### Creating Chapters in Fixed Intervals 

This section will explain how to use the MKVToolNix GUI app to create, edit, and export chapter information. 

Open MKVToolNix. Navigate to the *Multiplexer* sidebar tab. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters01.png "Multiplexer Tab")

Drop in **chapters_test.mkv**. This will populate the *Input* tab at the top of screen. 

In the *Source files:* section you should see only **chapters_test.mkv**. If you see any
extra files remove them by right-clicking and selected *Remove File*. 

In the *Tracks, chapters, and tags:* section you should see a Video track, an Audio track, a Subtitles track, and some tags. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters02.png "Input tab")

Click on the *Output* tab at the top of screen.

In the *Chapters* section set *Generating chapters:* to "Chapters in fixed intervals" and *Interval:* to "5s". 

Rename the *Destination file:* to "chapters\_test\_5sec.mkv"

Press *Start multiplexing*. This will create the new file Matroska files with a new chapter every five seconds. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters03.png "Output tab")

Open **chapters\_test\_5sec.mkv** with VLC. Skip around the Chapters using *Playback -> Chapter*

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters04.png "Chapters in VLC")

### Exporting Chapters to XML Data 

Go back to MKVToolNix, Navigate to the *Chapter editor* sidebar tab. Drop in **chapters_test_5sec.mkv**. You should see five chapters 
appear in the window on the left-hand side. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters05.png "Chapters Editor 5sec")

Export this info to an XML file by clicking (at the top of the screen) *Chapter editor -> Save as XML file*. Save the file as **chapter\_test\_5sec.xml**

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters07.png "Save as XML file")

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters08.png "chapter_test_custom.xml")

Open the XML file in a text editor and take a look!

### Editing Chapter Data

In MKVToolNix, Navigate to the *Chapter editor* sidebar tab. Drop in **chapters_test_5sec.xml**. You should see the familiar five chapters.

Clicking on any of the chapters will allow you edit information about the chapters.

Edit the chapters in this window to make your own custom chapters for the file. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters06.png "Chapters Editor custom")

Once you're finished, Return to the *Multiplexer* sidebar tab. 

Drop in **chapters_test.mkv**

Navigate to the **Output** tab. 

Click the folder icon next to *Chapter file:* and select  **chapter\_test\_custom.xml**. 

Rename the *Destination file:* to "chapters\_test\_custom.mkv"

Press *Start multiplexing*. This will create the new file Matroska files with a new chapter with your custom chapter markings. Test it out in VLC!

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters09.png "Multiplex new MKV with custom chapters")


### Embedding Attachments

This section will explain how to use the MKVToolNix GUI app to embed attachments into the wrapper of an MKV file. This tool is very flexible, but there isn't 
much in the way of guidelines as to how to use it in archival environments. In this example we'll be embedding a PBCore file in the MKV. This is not meant to be 
an endorsement of the process, but an illustration of what's possible. 

Start off by finding the samplePBCore.xml file, or making your own using mediainfo

```
mediainfo -f --Output=PBCore chapters_test_5sec.mkv > sampePBCore.xml
```

Open MKVToolNix, Navigate to the *Multiplexer* sidebar tab. 

Drop in **chapter\_test\_custom.mkv**, and navigate to the  *Attachments* tab.

In this tab, drop in the samplePBCore.xml file. You should see it the *Attachment to add:* window. 

![alt text](https://github.com/amiaopensource/An_Archivists_Guide_To_Matroska/blob/master/Screenshots/Chapters09.png "Adding attachments")

Rename the *Destination file:* to "chapters\_test\_pbore.mkv" and press *Start multiplexing*.

Drop **chapters_test_pbcore.mkv** into MKVToolNix, and you should see the PBCore file in *Attachment from source file:*

### Exporting Attachments

The MKVToolNix GUI is not capable of exporting attachments, so you'll have to do it with some CLI apps. 

First, we need to find out the ID of the attachment is. In order to do this type the following command:

```
mkvmerge -i chapters_test_pbcore.mkv
```

You'll get the following output.

```
File 'chapters_test_pbcore.mkv': container: Matroska
Track ID 0: video (0x46465631 "FFV1")
Track ID 1: audio (FLAC)
Track ID 2: subtitles (SubRip/SRT)
Attachment ID 1: type 'text/xml', size 5337 bytes, file name 'samplePBCore.xml'
Chapters: 3 entries
Global tags: 1 entry
Tags for track ID 0: 1 entry
Tags for track ID 1: 1 entry
Tags for track ID 2: 1 entry
```

We're looking to find out what the *Attachment ID* of the PBcore file is. 

Looking at the output, we can see that the ID for samplePBCore.xml is *1*. 

We can now use mkvextract to extract the specific attachment using the following command

```
mkvextract attachments chapters_test_pbcore.mkv 1:samplePBCore_Extracted.xml
```

Open up both PBCore files and compare!












