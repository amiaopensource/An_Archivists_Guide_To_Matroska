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

mkvextract can be used to extract the metadata tags from a file into an XML document. The following command reads the file called `some_metadata.mkv` and extracts the metadata tags to a file called `some_tags.xml`.

```
mkvextract some_metadata.mkv tags some_tags.xml
```

## Adding tags to an existing file with mkvpropedit

To write the metadata from `some_tags.xml` into `some_metadata.mkv` then use mkvpropedit like this:

```
mkvpropedit some_metadata.mkv --tags global:some_tags.xml
```

## Activity

- Export Metadata from some_metadata.mkv to an xml file.
- Edit that xml file to add additional metadata
- Add the metadata from the xml file back into some_metadata.mkv

See Matroska's [tagging example pages](https://matroska.org/technical/specs/tagging/example-video.html) for ideas.
