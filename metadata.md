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
            <String>@therealrudy</String>
        </Simple>
    </Simple>
    <Targets />
</Tag>

```  
### Adding tags to an existing file with mkvpropedit

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

See Matroska's [tagging example pages](https://matroska.org/technical/specs/tagging/example-video.html) for ideas.
