# Matroska Metadata

Matroska tagging documentation is available at [matroska.org](https://matroska.org/technical/specs/tagging/index.html).

## Adding tags with FFmpeg

Just add use the `-metadata` in FFmpeg to add any metadata tag, replace the KEY with the name of the metadata value that you'd like to use and the VALUE with that metadata value.

-metadata KEY="VALUE"

Example:

```
ffmpeg -f lavfi -i testsrc -t 1 -metadata ARCHIVAL=yes -metadata PRESERVE_THIS=okay -metadata DESCRIPTION="Just color bars" some_metadata.mkv
```

You can also add metadata specifically to certain tracks. This says that the first video track (counting starts at zero) has a language code of 'eng'.

```
ffmpeg -f lavfi -i testsrc -t 1 -metadata:s:v:0 language=eng more_metadata.mkv
```

## Extracting tags from an existing file with mkvextract

mkvextract can be used to extract the metadata tags from a file into an XML document. The following command reads the file called `hamburger.mkv` and extracts the metadata tags to a file called `hamburger.xml`. (different syntax to do the same thing)

```
mkvextract hamburger.mkv tags hamburger.xml
```

or

```
mkvextract tags hamburger.mkv > hamburger.xml
```

Open **hamburger.xml** in your text editor. You should see a number of tags in the file, each nested with a `<tag>` element, looking something like this:

```
<Tag>
    <Simple>
        <Name>PRODUCER</Name>
        <String>Rudy C. Granados</String>
    </Simple>
    <Targets />
</Tag>
```  

For this exercise we're going to do two things:

(1): Update the description

(2): Add nested data to the Producer

### Update description

Change the `<String>` tag to a new description. Be as creative as you like!

### Add nested data

Update the PRODUCER tag to look like this. In this example, we're adding the tag TWITTER HANDLE and associating it with the PRODUCER tag.
```
<Tag>
    <Simple>
        <Name>PRODUCER</Name>
        <String>Rudy C. Granados</String>
        <Simple>
            <Name>TWITTER HANDLE</Name>
            <String>@rudist_rudy</String>
        </Simple>
    </Simple>
    <Targets />
</Tag>

```  
## Adding tags to an existing file with mkvpropedit

To write the metadata from `hamburger.xml` into `hamburger.mkv` then use mkvpropedit like this:

```
mkvpropedit hamburger.mkv --tags global:hamburger.xml
```

Run `ffprobe` on the file to see the results.

```
ffprobe -i hamburger.mkv
```

If you look at the metadata portion of the output you should see the following information

```
    PRODUCER                : Rudy C. Granados
    PRODUCER/TWITTER HANDLE : @therealrudy
```

## Changing technical metadata with mkvpropedit

mkvpropedit can be used to change the properties of an MKV file without having to rewrite the file entirely. The standard template for an mkvpropedit command looks something like this

```
mkvpropedit [input_file.mkv] --edit track:[target_track] --set [target_element]=[new_value]
```

To use this template, you'll need to replace everything in square brakets with your own values:

`[input_file.mkv]` = the path to the files you wish edit
`[target_track] ` = the track you wish to edit. This could be a video track, audio track, subtitle track, etc.
`[target_element]` = the element you wish to edit
`[new_value]` = the new value for the element that you're updating.

The syntax for `[target_track] ` is "v1" for "first video track". "a1" for "first audio track, "a2" for second audio track, etc...

The syntax for `[target_element]`, and `[new_value]` can be seen by typing `mkvpropedit -l`

### Change Aspect Ratio

The following command will change the aspect ratio of a the chapters_test.mkv to 16:9

```
mkvpropedit chapters_test.mkv --edit track:v1 --set display-width=16 --set display-height=9 --set display-unit=3
```

After running the command, open chapters_test.mkv. You should be able to immediately see the video display as 16:9 now.

You can also run mediainfo on the file, and see that it reports as 16:9

```
mediainfo chapters_test.mkv
```

To set the aspect ratio back to 4:3 use the following command

```
mkvpropedit chapters_test.mkv --edit track:v1 --set display-width=4 --set display-height=3 --set display-unit=3

```

See Matroska's [tagging example pages](https://matroska.org/technical/specs/tagging/example-video.html) for ideas.
