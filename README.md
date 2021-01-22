# Kakadu Documentation + Resources

* [Official Documentation for Kakadu Python Wrapper](https://image-processing.readthedocs.io/en/latest/introduction.html)
* [User Manual for Kakadu](https://kakadusoftware.com/wp-content/uploads/2014/06/kdu_show.pdf)
* [Take a look at the additional resources for pdfs and exe necessary to install](/additional-resources)


# Notes on ExifTool

The ExifTool can be obtained from [here](https://exiftool.org).The stand-alone
Windows executable does not require Perl. Just download and un-zip the archive 
then double-click on "exiftool(-k).exe" to read the application documentation,
drag-and-drop files and folders to view meta information, or rename to 
"exiftool.exe" for command-line use. Runs on all versions of Windows.

## License

This is free software; you can redistribute it and/or modify it under the same
terms as [Perl](https://dev.perl.org/licenses/) itself.

## Examples Usage of Kakadu tools

The kdu_compress utility accepts a long list of options that will not be explained here. The following will generate a JPEG2000 compressed JP2 file with roughly 20:1 compression and 5 images sizes. The rate option is used to control the amount of compression. The Clevels option is for controlling the number of image sizes.

```sh
kdu_compress -i filename.bmp -o filename.jp2 -Clevels=5 -rate 1.09
```