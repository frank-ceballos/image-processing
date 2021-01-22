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

# Notes on JP2 WSI

Below is an image showing the WSI compressiong parameters

![JP2 ](additional-resources/jp2-wsi.png)

## License

This is free software; you can redistribute it and/or modify it under the same
terms as [Perl](https://dev.perl.org/licenses/) itself.

# Examples Usage of Kakadu tools

## Exampe 1
The kdu_compress utility accepts a long list of options that will not be explained here. The following will generate a JPEG2000 compressed JP2 file with roughly 20:1 compression and 5 images sizes. The rate option is used to control the amount of compression. The Clevels option is for controlling the number of image sizes.

```sh
kdu_compress -i filename.bmp -o filename.jp2 -Clevels=5 -rate 1.09
```

David Taubman, creator/owner of Kakadu Software stated in the online discussion forum, "The JPEG2000 standard has no explicit definition for a quality factor (neither does the JPEG standard for that matter). As a general rule, the rate-distortion slope threshold often turns out to be a more reliable indicator of quality than the bit-rate. I would suggest compressing some images which are typical of your application to the point which you believe represents a sufficient quality, then looking at the slope thresholds reported by "kdu_compress" in its verbose (default) mode. You can then compress all your images, using the "-slope" option."

The original source for this example can be found [here](http://webservices.itcs.umich.edu/mediawiki/dlxs14/index.php/Image_Class_and_JPEG2000).

## Example 2
From the kakadu command line free tools:

```
kdu_compress -i in.tif -o out.jp2 Creversible=yes
```

The original source for this example can be found [here](http://gdcm.sourceforge.net/wiki/index.php/Tools/kdu_compress).
## Example 3
```
-------------------------------------------------------------------------------
Usage Examples for Demonstration Applications Supplied with Kakadu V8.0.5
-------------------------------------------------------------------------

To help you get started right away, we provide some useful examples
to demonstrate the use of the Kakadu example applications.  These are
far from exhaustive and the Kakadu software framework itself is intended
to be used in building more extensive applications than these
demonstration applications.  Nevertheless, the demonstration applications
are quite powerful.  Licensed versions of the Kakadu system ship with
some additional, much simpler demonstration applications for dydactic
purposes.

HTJ2K Note: We have placed all examples relating to the new High Throughput
JPEG 2000 (HTJ2K) standard at then end of each demo App's usage examples,
so as to help you find them.  You can also search for strings like
"=HT" or "SCP15"

kdu_compress
------------
  Note 1: you may find it very useful to examine the attributes used by
  the compressor by supplying a `-record' argument on the command
  line.  You may also find it very useful to open up a code-stream
  (optionally embedded inside a JP2 file) using "kdu_show" and to
  examine the properties (use menu or "p" accelerator) -- note that
  some of the attributes used by the compressor cannot be preserved
  in the code-stream (e.g., visual weights), so will show up only when
  you use `-record' with the compressor.

  Note 2: While kdu_compress is the most flexible of the image compression
  demo-apps, being designed to show off the largest number of Kakadu
  encoding capabilities, it is not the fastest.  If you are interested in
  measuring speed (or just processing things as quickly as possible), use
  "kdu_buffered_compress", which builds upon a higher level very powerful
  Kakadu API `kdu_stripe_compressor'.  That demo-app does not read as many
  different file formats, but it uses an optimized image data reading
  process.  You can compress sequences of images even more efficiently using
  "kdu_v_compress", placing the codestreams in a video container like MJ2
  or writing them as a (almost) raw sequence to an MJC file or to stdout.

  This demo app supports the following input image formats:
  PGM/PPM (up to 16 bits/channel); PBM (i.e., bi-level);
  PFM (floating point with 1 or 3 channels); BMP (i.e., the
  Windows upside down format); TIFF images (all precisions, number
  of channels, etc., out to floating point, including BigTIFF,
  but not compressed TIFF files unless you compile against LibTIFF);
  various raw sample formats.  Multiple input files can be supplied,
  comma-separated or using a special concatenated file syntax.

 a) kdu_compress -i image.pgm -o out.j2c -rate 1.0
    -- irreversible compression to 1 bit/sample.
 a1) kdu_compress -i image.pgm -o out.j2c Qfactor=85
    -- uses a quality factor (85% here) instead of bit-rate as the
       compression objective
    -- the quality factor is intended to have a broadly similar meaning to
       the one commonly used to compress JPEG images
    -- note that some visual weighting is automatically introduced, but goes
       away as the quality approaches 100%, at which point the PSNR of the
       compressed result should be about 3dB higher than the natural PSNR
       associated with digitizing original continuous image intensities to
       8 bit precision (adapts to the bit-depth of the input content).
 a2) kdu_compress -i image.pgm -o out.j2c Qfactor=85 -rate 1.0
    -- Combines the attributes of both of the above examples, compressing
       the image with visual weighting, to the quality associated with
       quality factor 85, unless this exceeds a compressed data-rate of
       1 bit/pixel, in which case the compressed representation is trimmed
       to that bit-rate in a rate-distortion optimal way.
 b) kdu_compress -i image.pgm -o out.j2c -rate 1.0,0.5,0.25
    -- irreversible compression to a 3 layer code-stream (3 embedded bit-rates)
 b1) kdu_compress -i image.pgm -o out.j2c -rate 1.0,0.5,0.25 Qfactor=85
    -- as in the above example, but limiting the highest quality that can
       be achieved to that associated with a quality factor of 85% and
       also introducing some visual weighting to the rate-distortion
       optimization objective associated with all 3 quality layers.
 b2) kdu_compress -i image.pgm -o out.j2c -rate 1.0,0.5,0.25 Qfactor=85 Ctype=N
    -- as in the above example, but sets the component-type to non-visual (N)
       so that there will be no visual weighting.  The rate-distortion
       optimization objective is then PSNR (i.e., minimization of mean
       squared error) for all quality layers.
 c) kdu_compress -i image.pgm -o out.j2c Creversible=yes -rate -,1,0.5,0.25
    -- reversible (lossless) compression with a progressive lossy to
       lossless code-stream having 4 layers.  Note the use of the dash, '-',
       to specify that the final layer should include all remaining
       compressed bits, not included in previous layers.  Specifying a
       large bit-rate for one of the layers does not have exactly the
       same effect and may leave the code-stream not quite lossless.  See
       usage statement for a more detailed explanation.
 d) kdu_compress -i red.pgm,green.pgm,blue.pgm -o out.j2c -rate 0.5
    -- irreversible colour compression (with visual weights) to 0.5 bit/pixel
    -- may use image.ppm or image.bmp if you want to start with a colour image
    -- ppm/pgm images with bit-depths up to 16 bits/channel are supported
    -- pfm (floating point) images are also supported
 d1) kdu_compress -i red.pgm,green.pgm,blue.pgm -o out.j2c -rate 0.5 Qfactor=85
    -- similar to other examples, this one imposes a quality factor constraint,
       in addition to the bit-rate constraint.
    -- while the quality factor is intended to have similar meaning to
       that used with JPEG, note that colour image compression with JPEG
       normally involves sub-sampling of the chrominance components
       produced after a conversion from RGB to YCbCr (the so-called 4:2:0
       colour representation), while Kakadu here is compressing the image
       with a full 4:4:4 colour representation.
    -- for 4:2:0 compression, as commonly used with JPEG, you can add the
       "-rgb_to_420" option to "kdu_compress", as demonstrated in later
       examples.
 e) kdu_compress -i image.pgm -o out.j2c Creversible=yes Clayers=9
                -rate 1.0,0.04 Stiles={711,393} Sorigin={39,71}
                Stile_origin{17,69} Cprecincts={128,128},{64,64}
                Corder=PCRL
    -- spatially progressive code-stream with 9 embedded quality layers,
       roughly logarithmically spaced between 0.04 and 1.0 bits per pixel,
       with some interesting canvas coordinates and weird tile sizes.
 f) kdu_compress -i image.pgm -o out.j2c Corder
    -- type this sort of thing when you can't remember the format or
       description of some element of the parameter specification language.
       In this case, you get an error message with an informative description
       of the "Corder" code-stream parameter attribute.
    -- you may find out all about the code-stream specification language
       by typing "kdu_compress -usage".
 g) kdu_compress -i image.bmp -o out.j2c -rate 0.5 -rotate 90
    -- compresses monochrome or colour bottom-up BMP file with 90 degree
       rotation.  Note that file organization geometry is folded into other
       geometric transformations, which are all performed without any
       buffering of uncompressed data.
 h) kdu_compress -i image.ppm -o out.j2c Stiles={171,191}
                 Clevels:T0C1=0 Cuse_sop:T4=yes Cycc:T2=no
    -- Use only 0 levels (instead of the default 5) of DWT for the second
       component (C1) of the first tile.  Put SOP markers in front of each
       packet of the fifth tile.  Turn off colour transformation (used by
       default for compatible 3-component images) in the third tile.
    -- Command lines used to specify complex code-stream parameter
       configurations can become very long.  As an alternative, you may
       place parameters into one or more switch files and load them from
       the command line using the "-s" option.
 i) kdu_compress -i image.pgm -o out.j2c -rate 1.0,0.3,0.07 Stiles={300,200}
                 Clayers=3 Clayers:T0=2 Clayers:T1=7 Cuse_sop=yes Cuse_eph=yes
    -- Rate allocation is performed across 3 quality layers.  Since the
       first tile is assigned only 2 layers, its quality will not improve
       beyond that associated with the second global bit-rate, 0.3 bps.
       The extra 4 layers for the second tile (T1) will receive empty
       packets without any SOP markers.  EPH markers are included with all
       packets, as mandated by the standard (see corrigendum).
 j) kdu_compress -i image.pgm -o out.j2c -rate 1.0,0.5,0.1,0.03
                 Stiles={300,200} Corder=LRCP
                 Porder:T1={0,0,2,10,10,LRCP} Porder:T1={0,0,4,10,10,PCRL}
                 -record log.txt
    -- Tile 1 (the second tile) gets two tile-parts.  The first tile-part of
       tile 1 includes the first 2 layers (0.1 bits per sample) and has a
       layer progressive order (LRCP).  The second tile-part contains the
       final two quality layers and has a resolution-progressive order (RLCP).
       The first tile-part of every tile appears first, followed by the extra
       tile-part of tile 1 (interleaved tile-parts).  Interesting things happen
       when you truncate the code-stream to a bit-rate below 1.0 -- you
       should be able to guess.
    -- The log file generated using "-record" is very useful for interpreting
       the results of complex command lines.  It uses Kakadu's uniform
       parameter language to report the code-stream parameter configuration.
 k) kdu_compress -i image.pgm -o out.bits -rate 1.0 Cprecincts={128,128}
                 Cuse_sop=yes Cuse_eph=yes "Cmodes=RESTART|ERTERM"
    -- Generates a code-stream with various error resilience features
       enabled.  Use "kdu_expand -resilient" with such code-streams for the
       best results in the event of transmission error.
 l) kdu_compress -i image.raw -o out.bits Nprecision=16 Nsigned=no
                 Sdims={1024,800} Qstep=0.0001 -rate 1.0
    -- Process a raw 16-bit image.
    -- Big-endian byte order is assumed for files with the ".raw" suffix,
       whereas little-endian byte order is assumed if the file has a ".rawl"
       suffix.  Pay special attention to this, since the native byte order
       varies from platform to platform -- we don't want our files to have
       platform-dependent interpretations now, do we!
    -- Note that for raw images you need to supply all of the dimensional
       information: image dimensions, bit-depth and whether the image
       samples are signed 2's complement or unsigned values; Kakadu's
       internal `Sprecision' and `Ssigned' attributes are derived from
       `Nprecision' and `Nsigned' supplied explicitly here.
    -- Note also that the irreversible processing path chooses
       a default set of quantization parameters based on a single scaling
       parameter (Qstep) -- you can specify individual subband quantization
       parameters if you really know what you are doing.  The Qstep value is
       interpreted relative to the nominal range of the data which is from
       -2^{B-1} to 2^{B-1}-1 where B is the bit-depth (Sprecision).  If your
       data is represented as 16-bit words, but all the information resides
       in the least significant 10 bits of these words, the default value
       of Qstep=1/256 may not be appropriate.  In this case, the best thing
       to do would be to specify the actual number of least significant
       bits which are being used (e.g., Sprecision=10 -- it assumes that
       the data is the least significant B bits of a ceil(B/8) byte
       word).  Alternatively, you may leave the most significant bits
       empty, but you should choose a smaller value for Qstep (as suggested
       by the example).  Remember that rate control is performed
       independently of quantization step size selection, except that if
       the quantization steps are too course, not enough bits will be
       produced by the entropy coder for the rate controller to achieve
       the target.  To see how many bits are being produced in any
       given case, run the compressor without a `-rate' argument.
 l1) kdu_compress -i image.raw -o out.bits Nprecision=16 Nsigned=no
                 Sdims={1024,800} Qfactor=90 Ctype=N -rate 1.0
    -- From Kakadu version 8.0.4, the `Qfactor' attribute provides a more
       intuitive way to set the `Qstep' value.  Here, we provide a very
       high quality factor (90%) and tell Kakadu not to apply any visual
       weighting (`Ctype' N means a non-visual image component).
    -- You could go all the way, specifying `Qfactor'=100, which will
       set up an extremely small Qstep value for you at the high
       sample bit-depth (16 bits) used in this example.  The 100% quality
       factor allows compressed quality to reach PSNRs of about 3dB
       higher than the PSNR naturally associated with digitizing continuous
       imagery to the precision P (16 bits here); this natural digitization
       PSNR is 10*log_10(12*2^{2P}).
 m) kdu_compress -i image_y.pgm,image_cb.pgm,image_cr.pgm -o out.jp2
                 -jp2_space sYCC CRGoffset={0,0},{0.25,0.25},{0.25,0.25}
                 -rate 1,0.5,0.2
    -- Compresses a YCbCr image directly, having chrominance components
       sub-sampled by a factor of 2 in each direction.  The CRGoffset
       argument aligns the chrominance samples in the middle of each
       2x2 block of luminance samples.  You may work with any sub-sampling
       factors you like, of course, and they may be different in each
       direction and for each component.  As a general rule, the mid-point
       registration of sub-sampled chrominance components requires
       CRGoffset values of 0.5-1/(2S), where S is the relevant
       sub-sampling factor.
          Identifies the colour space as sYCC through a containing JP2
       file's colour box so that the image can be correctly rendered
       (including all appropriate interpolation, component alignment and
       colour conversion operations) by the "kdu_show" application or any
       other conforming JP2 rendering application.
 m1) kdu_compress -i image_y.pgm,image_cb.pgm,image_cr.pgm -o out.jp2
                 -jp2_space sYCC CRGoffset={0,0},{0.25,0.25},{0.25,0.25}
                 -rate 1,0.5,0.2 -chroma_weights 1
    -- As above, but introduces visual weights that are appropriate for
       the YCbCr space, taking chroma sub-sampling into account.
 m1a) kdu_compress -i image_y.pgm,image_cb.pgm,image_cr.pgm -o out.jp2
                 -jp2_space sYCC CRGoffset={0,0},{0.25,0.25},{0.25,0.25}
                 -rate 1,0.5,0.2 Ctype=Y,Cb,Cr
    -- A better (more explicit) way to do the same thing as in example (m1).
 m2) kdu_compress -i red.pgm,green.pgm,blue.pgm -o out.jp2 -rgb_to_420
                  -rate 1,0.5,0.2
    -- Same as above, but the input RGB content is converted to YCbCr
       and the chrominance components are sub-sampled internally.  The
       "-rgb_to_420" option conveniently introduces the CRGoffset
       attributes and sYCC colour space metadata exactly as above,
       automatically applying an appropriate set of visual weights.
 m3) kdu_compress -i image.ppm -o out.jp2 -rgb_to_420 Clevels=7 Cvis=0.0001
                  Cblk={32,32} -rate 1,0.75,0.5,0.375,0.25
    -- Similar to above, but adds a visual masking model to better balance
       distortion across the image and spatial frequency subbands, using
       32x32 code-blocks instead of the default 64x64 code-blocks so as
       to give finer control over the spatial distribution of distortion.
       The Cvis option rarely does any harm to subjective image quality
       and often improves it substantially, especially when working with
       very large images containing substantial content diversity.
    -- This is a good test configuration to use for visual performance,
       although you should also explore Cvis=0.001.  Adding "-no_info",
       "-full" and "-tolerance 0" can help to get the most out of a given
       bit budget.
 m4) kdu_compress -i image.ppm -o out.jp2 -rgb_to_420 Qfactor=85
    -- pure Qfactor-based compression, very similar to what is done with
       the original JPEG algorithm, but with all the benefits of JPEG 2000.
 m5) kdu_compress -i image.ppm -o out.jp2 -rgb_to_420 Qfactor=85 -rate 1,0.5
    -- Qfactor-limited compression with explicit bit-rate constraints and
       two quality layers.
 n) kdu_compress -i image.pgm -o out.jp2 Creversible=yes -rate -,1,0.5
                 -jp2_space iccLUM,2.2,0.099
    -- Embeds the compressed image within a JP2 file having an embedded
       ICC profile identifying the image as having the tone reproduction
       curve defined by the NTSC standard (gamma curve for sRGB has
       parameters gamma=2.4 and beta=0.055 instead of 2.2 and 0.099).
 o) kdu_compress -i image.ppm -o out.jp2 -rate 2,1,0.5
                 -jp2_space iccRGB,3,0.16,0.9642,0,0,0,1,0,0,0,0.8249 Cycc=yes
    -- The embedded ICC profile inserted into the JP2 file describes the
       colour channels as G(X/X0), G(Y/Y0) and G(Z/Z0) where (X0,Y0,Z0)
       are the whitepoint of the D50 profile connection space and G() is
       the standard CIELab gamma function having parameters gamma=3.0 and
       beta=0.16.  The YCC transform applied to these colour channels for
       compression is not all that radically different from the linear
       opponent transform applied to the gamma corrected colour channels
       in the CIELab colour space.  It follows that this representation
       should have properties similar to Lab at D50 and can easily be
       converted (by means of a well conditioned linear transform) into
       a true D50 Lab space.
 p) kdu_compress -i image.ppm -o out.jp2 -rate -,0.05 Clayers=30
                 Creversible=yes Rshift=12 Rlevels=5 -roi {0.3,0.1},{0.5,0.5}
    -- Compresses a colour image losslessly using the max-shift ROI method
       to ensure that a square region of is assigned much higher priority
       in the layer generation process.  The region represents one quarter of
       the total number of image pixels and starts 30% of the way down and
       10% of the way across from the left of the image.  Reconstructing the
       initial layers (you can use kdu_show, kdu_expand or kdu_transcode to
       partially reconstructing or pair down the image) leaves an extremely
       low quality in the background (everything other than the region of
       interest) but a rapidly improving quality in the foreground as more
       and more layers arrive.  The foreground becomes lossless before
       the background improves substantially -- it eventually becomes lossless
       too.
 q) kdu_compress -i image.ppm -o out.jp2 -rate -,0.5 Clayers 20 Cblk={32,32}
                 Creversible=yes Rweight=7 Rlevels=5 -roi mask.pgm,0.5
    -- Another region of interest encoding example.  In this case the region
       is found from the mask image -- the foreground corresponds to the
       mask pixels whose values exceed 50% of the dynamic range (i.e., 128).
       The mask image is automatically scaled to fit the dimensions of each
       image component (scaling and region propogation are done incrementally
       so as to minimize memory consumption).  In this case, the max-shift
       method is not used. Instead, the distortion cost function which drives
       the PCRD-opt layer formation algorithm is modulated by the region
       characteristics.  The transition from background to foreground is
       softer than in the max-shift case and may be controlled by `Rweight'.
       Region definition is poorer than with the max-shift method, but a
       number of important disadvantages are avoided.  For more on this,
       consult the "kakadu.pdf" document.
 r) kdu_compress -i huge.pgm -o huge.jp2 -rate 1.5 Clayers=20 Creversible=yes
                 Clevels=8 Cprecincts={256,256},{256,256},{128,128}
                 Corder=RPCL ORGgen_plt=yes ORGtparts=R Cblk={32,32}
    -- I have used this exact command to successfully compress a very large
       geospatial image (> 500 MByte BMP file).  The entire image is compressed
       without any tiling whatsoever.  The compressed image may subsequently
       be viewed quickly and efficiently using "kdu_show", at any resolution.
       The key elements here are: 1) the generation of PLT marker segments
       (ORGgen_plt=yes); 2) the use of a packet sequence (RPCL) which places
       all packets of each precinct consecutively within the code-stream
       (otherwise, it is hard to efficiently represent or use the PLT
       marker information); and 3) the use of relatively small precincts.
       The additional "ORGtparts=R" attribute introduces tile-part headers
       immediately before each resolution level and locates the packet length
       information with the header of the tile-part to which the packets
       belong.  This has the effect of delaying the loading and parsing of
       packet length identifiers (hundreds of thousands of packets were
       generated in the 500 MByte image example) until an interactive
       viewer or browser requests the relevant resolution.
s) kdu_compress -i small.pgm -o small.jp2 -rate 1 Clayers 5 -no_info
   -- The `-no_info' option prevents Kakadu from including a comment (COM)
      marker segment in the code-stream to identify the rate-distortion slope
      and size associated with each quality layer.  This information is
      generated by default, starting from v3.3, since it allows rendering
      and serving applications to customize their behaviour to the properties
      of the image.  The only reason to turn off this feature is if you
      are processing very small images and are interested in minimizing the
      size of the code-stream.
t) kdu_compress -i massive.ppm -o massive.jp2 -rate -,0.001 Clayers=28
                Creversible=yes Clevels=8 Corder=PCRL ORGgen_plt=yes
                Cprecincts={256,256},{256,256},{128,128},{64,128},{32,128},
                           {16,128},{8,128},{4,128},{2,128} -flush_period 1024
   -- You might use this type of command to compress a really massive image,
      e.g. 64Kx64K or larger, without requiring the use of tiles.  The
      code-stream is incrementally flushed out using the `-flush_period'
      argument to indicate that an attempt should be made to apply incremental
      rate control procedures and flush as much of the generated data to the
      output file as possible, roughly every 1024 lines.  The result is that
      you will only need about 1000*L bytes of memory to perform all
      relevant processing and code-stream management, where L is the image
      width.  It follows that a computer with 256MBytes of RAM could
      losslessly an image measuring as much as 256Kx256K without
      resorting to vertical tiling.  The resulting code-stream can be
      efficiently served up to a remote client using `kdu_server'.
t1) kdu_compress -i enormous.tif -rate 1 Clayers=28 Clevels=12 Corder=RPCL
                 Cprecincts={256,256},{128,256},{64,256} -flush_period 1024
                 -mem -cpu 0
    -- In this example, an enormous image (e.g., 128K x 128K) might be
       compressed to a single tile, with incremental flushing and without
       strong limitations on the precinct sizes, so that code-blocks remain
       quite large in all resolutions and there are many resolution levels
       (there could be more).  This does not work for compressed data
       targets that have a linear organisation, because the codestream
       cannot be written in any legal order without having to buffer up
       at least enough compressed data to accommodate a full row of
       precincts at the lowest resolution level (more than 256000 lines
       would have to be processed before anything could be incrementally
       flushed in that case).  However, in the present example, the
       output file is omitted.  This causes "kdu_compress" to pass a
       special type of compressed data target to `kdu_codestream::create'
       that advertises the ability to accept structured codestream
       elements (headers and precincts) in any order.  Currently, that
       target is a null target, meaning that it discards all of its
       content; however, you could modify this to direct the data to a
       stuctured data-base, from which the content could later be
       re-ordered as a linear codestream.  Alternatively, you could
       store the structured elements within a file or data-base that
       is consistent with Kakadu's caching compressed data source
       model, allowing the content to be rendered, navigated and
       eventually even served directly from the structured cache
       reprsentation.
   -- For the moment, you might like to try the above demo to see how
      effective incremental flushing to a structured target can be.  You
      will find that the incremental flushing capability, combined with
      a structured cache target (such as the null target used here)
      can not only save a huge amount of memory, but also provide
      substantially higher overall throughputs when deployed on a
      platform that has many CPU cores.
u) kdu_compress -i im32.bmp -o im32.jp2 -jp2_alpha -jp2_box xml.box
   -- Demonstrates the fact that "kdu_compress" can read 32-bit BMP files
      and that you can tell it to regard the fourth component as an alpha
      channel, to be marked as such in the JP2 header.  The "kdu_show"
      application ignores alpha channels only because alpha blending is
      not uniformly supported across the various WIN32 platforms.  The
      Java demo application "KduRender.java" will use an image's alpha
      channel, if any, to customize the display.
   -- The example also demonstrates the inclusion of additional meta-data
      within the file.  Consult the usage statement for more on the structure
      of the files supplied with the `-jp2_box' argument.  To reveal the
      meta-data structure of a JP2 file, use "kdu_show"'s "meta-show"
      capability, accessed via the `m' accelerator or the view menu.
v) kdu_compress -i im.ppm -o im.jpx -jpx_space ROMMRGB
   -- demonstrates the generation of a true JPX file.
   -- demonstrates the fact that any of the JPX enumerated colour space
      descriptions can now be used; assumes, of course, that the input image
      does have a ROMM RGB colour representation (in this case).
   -- you can actually provide multiple colour spaces now, using `-jp2_space'
      and/or `-jpx_space', with the latter allowing you to provide
      precedence information to indicate preferences for readers which are
      able to interpret more than one of the representations.
w) kdu_compress -i frag1.pgm -o massive.jp2 Creversible=yes
                 Clevels=12 Stiles={32768,32768} Clayers=30
                 -rate -,0.0000001 Cprecincts={256,256},{256,256},{128,128}
                 Corder=RPCL ORGgen_plt=yes ORGtparts=R Cblk={32,32}
                 ORGgen_tlm=13 -frag 0,0,1,1 Sdims={1500000,2300000}
   kdu_compress -i frag2.pgm -o massive.jp2 Creversible=yes
                 Clevels=12 Stiles={32768,32768} Clayers=30
                 -rate -,0.0000001 Cprecincts={256,256},{256,256},{128,128}
                 Corder=RPCL ORGgen_plt=yes ORGtparts=R Cblk={32,32}
                 ORGgen_tlm=13 -frag 0,1,1,1
   kdu_compress -i frag3.pgm -o massive.jp2 Creversible=yes
                 Clevels=12 Stiles={32768,32768} Clayers=30
                 -rate -,0.0000001 Cprecincts={256,256},{256,256},{128,128}
                 Corder=RPCL ORGgen_plt=yes ORGtparts=R Cblk={32,32}
                 ORGgen_tlm=13 -frag 0,0,2,1
   ...
   -- demonstrates the compression of a massive image (about 3.5 Tera-pixels
      in this case) in fragments.  Each fragment represents a whole number of
      tiles (in this case only one tile, each of which contains 1 Giga-pixel)
      from the entire canavs.  The canvas dimensions must be explicitly
      given so that the fragmented generation process can work correctly.
   -- To view the codestream produced at any intermediate step, after
      compressing some initial number of fragments, you can use
      "kdu_expand" or "kdu_show".  Note, however, that while this will work
      with kakadu, you might not be able to view a partial codestream using
      other manufacturers' tools, since the codestream will not generally
      be legal until all fragments have been compressed.
   -- To understand more about fragmented compression, see the usage statement
      for the `-frag' argument in "kdu_compress" or, for a thorough
      picture, you can check out the definition of `kdu_compress::create'.
   -- In this example, the codestream generation machinery itself produces
      TLM (tile-part-length) marker segments.  This is done by selectively
      overwriting an initially empty sandpit for TLM marker segments in the
      main header.  TLM information makes it easier to efficiently access
      selected regions of a tiled image.
   -- As an alterative to providing separate input files for each source
      fragment, you can supply a single common input file for all fragments
      and use the "-icrop" argument to automatically crop out just the
      region of the image which you need to create each fragment.  The
      "-icrop" feature is not necessarily supported by all image file format
      reading tools used by the "kdu_compress" demo application, but it should
      be supported by the TIFF reading code, which also supports the new
      BigTIFF file format.
x) kdu_compress -i volume.rawl*100@524288 -o volume.jpx -jp2_space sLUM
                -jpx_layers * Clayers=16 Creversible=yes Sdims={512,512}
                Sprecision=12 Ssigned=no Cycc=no
   -- Compresses an image volume consisting of 100 slices, all of which are
      packed into a single raw file, containing 12-bit samples, in the
      least-significant bits of each 2-byte word with little-endian byte order
      (note the ".rawl" suffix means little-endian, while ".raw" means
      big-endian).
   -- The expression "*100@524288" means that the single file "volume.rawl"
      should be unpacked into 100 consecutive images, each separated by
      524288 bytes (this happens to be 512x512x2 bytes).  Of course, we
      could always provide 100 separate input files on the command-line but
      this is pretty tedious.
   -- The "-jpx_layers *" command instructs the compressor to create one
      JPX compositing layer for each image component (each slice of the
      volume).  This will prove particularly interesting when multi-component
      transforms are added (see examples Ai to Ak below).  Take a look at
      the usage statement for other ways to use the new "-jpx_layers" switch.
y) kdu_compress -i geo.tif -o geo.jp2 Creversible=yes Clayers=16 -num_threads 2
   -- Compress a GeoTIFF image, recording the geographical information tags
      in a GeoJP2 box within the resulting JP2 file.  Kakadu can natively
      read a wide range of exotic TIFF files, but not ones which contain
      compressed imagery.  For these, you need to compile against the public
      domain LIBTIFF library (see "Compilation_Instructions.txt").
   -- From version 5.1, Kakadu provides extensive support for multi-threaded
      processing, to leverage parallel processing resources (multiple
      CPU's, multi-core CPU's and/or hyperthreading CPU's).  In this example,
      the `-num_threads' argument is explicitly used to control threading.
      The application selects the number of threads to match the number of
      available CPU's by default, but it is not always possible to detect
      the number of CPU's on all platforms.  To force use of the single
      threaded processing model from previous versions of Kakadu, specify
      "-num_threads 0".  To use the multi-threading framework of v5.1 but
      populate the environment with only 1 thread, specify "-num_threads 1";
      in this latter case, there is still only one thread of execution in
      the program, but the order in which processing steps are performed
      is driven by Kakadu's thread scheduler, rather than the rigid order
      associated with function invocation.
z) kdu_compress -i frame.tif -o dci_frame.jp2 Sprofile=CINEMA4K
                Creslengths=1302083 Creslengths:C0=1302083,1041666
                Creslengths:C1=1302083,1041666 Creslengths:C2=1302083,1041666
   -- Compresses a 3-plane 12-bit per sample TIF image to a JP2 file whose
      embedded codestream is compliant with the 4K digital cinema profile,
      with rate constraints adjusted for a 24fps projection environment.
   -- This example demonstrates use of the "Creslengths" parameter
      attribute for constraining the compressed size associated with
      resolution-specific and/or component-specific subsets of the
      codestream.  You can combine Creslengths with -rate or -slope, so that
      Creslengths just acts as a guard to prevent violation of constraints
      under unusual circumstances.  This is important when generating
      Digital Cinema content.  The "Creslengths" attribute provides a rich
      set of potential constraints, well beyond what is required by Digital
      Cinema.  It allows you to bound the compressed size of any image
      resolution (globally), any image component at any resolution (globally),
      any resolution of any tile or any resolution of any tile-component
      (image component of a tile).  Moreover, it allows you to provide bounds
      (or omit bounds) for any or all of the quality layers you want to
      generate.
   -- You should note that `Creslengths' constrains only the total number
      of bytes found in JPEG2000 packets (packet bodies and packet headers).
      It does not include the main header or tile-part header sizes.  For
      a typical digital cinema codestream, the main header is around 200
      bytes in size and each tile-part header occupies 14 bytes.  You can
      find detailed information about the header sizes by subtracting the
      values returned by `kdu_codestream::get_total_bytes' and
      `kdu_codestream::get_packet_bytes'.
   -- Although `Creslengths' provides absolute constraints on the sizes
      of various subsets of the codestream, it is strongly recommended
      that you also provide an overall constraint on the generated
      frame sizes via a "-rate" argument to "kdu_compress" -- or by
      explicitly setting the size limit in calls to the
      `kdu_codestream::flush' API function.  Doing this generally increases
      the efficiency of the rate control processing machinery and also
      ensures that the overall codestream size constraint accounts for the
      codestream main header and tile-part headers -- or you can subtract
      these small values from the constraints of interest.
z1) kdu_compress -i frame.tif -o dci_frame.jp2 Sprofile=CINEMA2K
                 Creslengths=1041666 Creslengths:C0=833333
                 Creslengths:C1=375000 Cagglengths:C2=1
    -- Similar to the above example, except that the supplied constraints
       are targeting a high frame-rate 2K digital cinema profile at 60
       frames/second, in which the overall bit-rate is constrained to
       500MB/s, the luminance channel (component 0) is constrained to 400MB/s
       and the combined data rates of the chrominance channels are
       constrained to 180MB/s.  The aggregate constraint is specified
       with image component 1's `Creslengths' attribute, while the
       `Cagglengths' attribute for image component 2 identifies component 1
       as its aggregation target.
z2) kdu_compress -i frame.tif -o dci_frame.jp2 Sprofile=CINEMA2S
                 Creslengths=651041,0,1302083
                 Creslengths:C0=520833,0,1041666
                 Creslengths:C1=520833,0,1041666
                 Creslengths:C2=520833,0,1041666
    -- Similar to example (z), this one generates a codestream according to
       the "Scalable 2K" Digital Cinema profile, targeting 48fps operation.
       In this case, there are 2 quality layers (you can specify this
       explicitly, but the CINEMA2S profile sets it up automatically).  The
       overall (and per-component) size constraints for the first quality
       layer correspond exactly to those of the CINEMA2K profile, while the
       size constraints for the overall codestream are twice as large,
       allowing the 2K cinema frames to be played at 24fps with comparable
       bit-rate to regular CINEMA2K at 24fps.
    -- As explained with example (z), it is recommended that you also
       use the "-rate" option to supply the target bit-rates for each
       quality layer, which in this case would be
       8*651041/(W*H) and 8*1302083/(W*H), respectively.  Doing this
       generally results in faster rate control processing and also
       ensures that the overall codestream sizes account for the
       codestream main header and tile-part headers.
z3) kdu_compress -i frame.tif -o dci_frame.jp2 Sprofile=CINEMA4S
                 Creslengths=1302083,0,2604116
                 Creslengths:C0=1302083,1041666,0,2083332
                 Creslengths:C1=1302083,1041666,0,2083332
                 Creslengths:C2=1302083,1041666,0,2083332
    -- Similar to example (z), this one generates a codestream that
       conforms to the "Scalable 4K" Digital Cinema profile CINEMA4S,
       for a frame rate of 24fps.
    -- There are two quality layers, the first of which is constrained
       in accordance to the regular 4K cinema profile CINEMA4K.
    -- As for the above examples, use of the "-rate" option is recommended,
       in addition to `Creslengths', primarily because it tends to increase
       the efficiency of the rate control machinery.  However the "-rate"
       parameter's argments are expressed in bits/pixel, which depends on
       the actual frame sizes used.

kdu_compress advanced Part-2 Features
-------------------------------------
    These additional examples look a lot more complex than the ones above,
    because they exercise rich features from Part-2 of the JPEG2000 standard.
    The signalling syntax becomes complex and may be difficult to fully
    understand without carefully reading the usage statements printed by
    "kdu_compress -usage", possibly in conjunction with IS15444-2 itself.
    In the specific applications which require these options, you would
    probably configure the relevant codestream parameter attributes directly
    from the application using the binary set/get methods offered by
    `kdu_params', rather than parsing complex text expressions from the
    command-line, as given here.  Nevertheless, everything can be
    prototyped using command-line arguments.

Aa) kdu_compress -i image.pgm -o image.jpx
                 Cdecomp=B(V--:H--:-),B(V--:H--:-),B(-:-:-)
    -- Uses Part-2 arbitrary decomposition styles (ADS) features to describe
       a packet wavelet transform structure, in which the highest two
       resolution levels of HL (horizontally high-pass) and LH (vertically
       high-pass) subbands are further subdivided vertically (HL) and
       horizontally (LH) respectively.  Subsequent DWT levels use the
       regular Mallat decomposition structure of Part-1.
    -- The decomposition structure given here is usually a little more
       efficient than the standard Mallat structure from Part-1.  This
       structure is also compatible with compressed-domain flipping
       functionalities which Kakadu uses to implement efficient rotation
       (for transcoding or rendering).
    -- Much richer splitting structures can be described using the `Cdecomp'
       syntax, but compressed domain flipping becomes fundamentally impossible
       if any final subband involves more than one high-pass filtering
       step in either direction.

Ab) kdu_compress -i image.ppm -o image.jpx
                 Cdecomp=B(BBBBB:BBBBB:B----),B(B----:B----:B----),B(-:-:-)
    -- Similar to example Aa), except that the primary (HL, LH and HH)
       subbands produced by the first two DWT levels are each subjected to
       a variety of further splitting operations.  In this case, the highest
       frequency primary HL and LH subbands are each split horizontally and
       vertically into 4 secondary subbands, and these are each split again
       into 4 tertiary subbands.  The highest frequency primary HH subband
       is split into just 4 secondary subbands, leaving a total of 36
       subbands in the highest resolution level.  In the second DWT level,
       the primary HL, LH and HH subbands are each split horizontally and
       vertically, for a total of 12 subbands.  All subsequent DWT levels
       follow the usual Mallat decomposition structure.

Ac) kdu_compress -i y.pgm,cb.pgm,cr.pgm -o image.jpx
                 Cdecomp:C1=V(-),B(-:-:-) Cdecomp:C2=V(-),B(-:-:-)
    -- Uses Part-2 downsampling factor styles (DFS) features to describe
       a transform in which the first DWT level splits the Cb and Cr image
       components (2'nd and 3'rd components, as supplied by "cb.pgm" and
       "cr.pgm") only in the vertical direction.  Subsequence DWT levels
       use full horizontal and vertical splitting (a la Part-1) for all
       image components.
    -- This sort of thing can be useful for applications in which the
       chrominance components have previously been subsampled horizontally
       (e.g., a 4:2:2 video frame).  In particular, it ensures that whenever
       the image is reconstructed at resolutions (e.g., at half or
       quarter resolution for the luminance), the chrominance components
       can be reconstructed at exactly the same size as the luminance
       component.

Ad) kdu_compress -i image.pgm -o image.jpx  Catk=2 Kkernels:I2=I5X3
      or, equivalently,
    kdu_compress -i image.pgm -o image.jpx  Catk=2
                 Kextension:I2=SYM Kreversible:I2=no
                 Ksteps:I2={2,0,0,0},{2,-1,0,0}
                 Kcoeffs:I2=-0.5,-0.5,0.25,0.25
    -- Uses Part-2 arbitrary transform kernel (ATK) features to describe
       an irreversible version of the spline 5/3 DWT kernel -- Part-1
       uses the reversible version of this kernel for its reversible
       compression path, but does not provide an irreversible version.
    -- The `Kkernels' attribute provides a convenient way to set up the
       other ATK parameters for common Part2 wavelet kernels.  These
       parameters are: `Kextension', `Ksymmetric', `Kreversible', `Ksteps'
       and `Kcoeffs'.
    -- If you wish to configure your own wavelet transforms, beyond those
       offered via the simple `Kkernels' attribute, you should carefully
       review the `Ksteps' and `Kcoeffs' parameter attribute syntax and
       interpretation, as explained in  the usage statement printed by
       "kdu_compress -usage" -- the same information is found in the
       source code and the "Properties" menu item within "kdu_show".
    -- Note that the `Catk' attribute identifies the kernel to be used
       via its instance index (2 in this case).  The kernel is then
       given by the `Kextension', `Kreversible', `Ksteps' and `Kcoeffs'
       attributes with this instance index (:I2), or more simply by a
       `Kkernels' attribute with the same instance index.

Ae) kdu_compress -i image.ppm -o image.jpx  Catk=2 Kkernels:I2=R2X2
       or, equivalently,
    kdu_compress -i image.ppm -o image.jpx  Catk=2
                 Kextension:I2=CON Kreversible:I2=yes
                 Ksteps:I2={1,0,0,0},{1,0,1,1}
                 Kcoeffs:I2=-1.0,0.5
    -- Another example of Part-2 arbitrary transform kernel (ATK) features,
       this time specifying the well-known Haar (2x2) transform kernel, for
       lossless processing; the reversible Haar DWT is also known as the
       "S-transform" in the literature.

Af) kdu_compress -i image.bmp -o image.j2c Catk=2
        Kextension:I2=SYM Kreversible:I2=yes
        Ksteps:I2={4,-1,4,8},{4,-2,4,8}
        Kcoeffs:I2=0.0625,-0.5625,-0.5625,0.0625,-0.0625,0.3125,0.3125,-0.0625
    -- Another example of Part-2 arbitrary transform kernel (ATK) features,
       this time specifying a reversible 13x7 kernel (13-tap symmetric low-pass
       analysis filter, 7-tap symmetric high-pass analysis filter) with two
       lifting steps.

Ag) kdu_compress -i image.ppm -o image.jpx  -jp2_space sRGB  Mcomponents=3
                 Sprecision=8,8,8  Ssigned=no,yes,yes  Mmatrix_size:I7=9
                 Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
                 Mvector_size:I1=3  Mvector_coeffs:I1=128,128,128
                 Mstage_inputs:I16={0,2}  Mstage_outputs:I16={0,2}
                 Mstage_collections:I16={3,3}
                 Mstage_xforms:I16={MATRIX,7,1,0,0} Mnum_stages=1 Mstages=16
    -- Compresses an RGB colour image using the conventional RGB to YCbCr
       transform to approximately decorrelate the colour channels, implemented
       here as a Part-2 multi-component transform.  The colour transform is
       actually identical to the Part-1 ICT (Irreversible Colour Transform),
       but this example is provided mainly to demonstrate the use of the
       multi-component transform.
    -- To decode the above parameter attributes, note that:
       a) There is only one multi-component transform stage, whose instance
          index is 16 (this is the I16 suffix found on the descriptive
          attributes for this stage).  The value 16 is entirely arbitrary.  I
          picked it to make things interesting.  There can, in general, be
          any number of transform stages.
       b) The single transform stage consists of only one transform block,
          defined by the `Mstage_xforms:I16' attribute -- there can be
          any number of transform blocks, in general.
       c) This block takes 3 input components and produces 3 output
          components, as indicated by the `Mstage_collections:I16' attribute.
       d) The stage inputs and stage outputs are not permuted in this example;
          they are enumerated as 0-2 in each case, as given by the
          `Mstage_inputs:I16' and `Mstage_outputs:I16' attributes.
       e) The transform block itself is implemented using an irreversible
          matrix decorrelation operator.  More specifically, the transform
          block belongs to the class of matrix decorrelation operators
          (1'st field of `Mstage_xforms:I16' record is "MATRIX"), with
          matrix coefficients taken from the `Mmatrix_size' and
          `Mmatrix_coeffs' attributes with instance index 7 (2'nd field of
          `Mstage_xforms:I16' is 7), using irreversible processing
          (4'th field of `Mstage_xforms:I16' is 0 -- irreversible).  Block
          outputs are added to the offset vector whose instance index is 1
          (3'rd field of `Mstage_xforms:I16' is 1), as given by the
          `Mvector_size:I1' and `Mvector_coeffs:I1' attributes.
       f) The mapping from YCbCr to RGB is performed using the 3x3 matrix,
          whose coefficients appear in raster order within the
          `Mmatrix_coeffs:I1' attribute.
       g) Since a multi-component transform is being used, the precision
          and signed/unsigned properties of the final decompressed (or
          original compressed) image components are identified by the
          `Mprecision' and `Msigned' attributes (8-bit unsigned image
          samples in this case), while their number is given by `Mcomponents'.
          The actual values of `Mprecision' and `Msigned' are not explicitly
          specified by compressors anymore, but they are derived internally
          from the `Nprecision' and `Nsigned' values.  In this example,
          the kdu_compress app sets Nprecision and Nsigned based on the
          information it finds in the input file's header, so they need
          not be specified on the command-line.
       h) The `Sprecision' and `Ssigned' attributes record the precision
          and signed/unsigned characteristics of what we call the codestream
          components -- i.e., the components which are obtained by block
          decoding and spatial inverse wavelet transformation.  In this
          case, these are the Y, Cb and Cr components.  The RGB to YCbCr
          transform has the property that these are also 8-bit quantities
          (no range expansion), with Cb and Cr holding signed quantities
          and Y (luminance) unsigned.

Ah) kdu_compress -i image.bmp -o image.jpx  -jp2_space sRGB  Mcomponents=4
                 Sprecision=8,8,8  Ssigned=no,yes,yes  Mmatrix_size:I7=9
                 Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
                 Mvector_size:I1=3  Mvector_coeffs:I1=128,128,128
                 Mvector_size:I2=1  Mvector_coeffs:I2=128
                 Mstage_inputs:I16={0,2},{0,0}  Mstage_outputs:I16={0,3}
                 Mstage_collections:I16={3,3},{1,1}
                 Mstage_xforms:I16={MATRIX,7,1,0,0},{MATRIX,0,2,0,0}
                 Mnum_stages=1 Mstages=16
    -- Same as example Af), except that the multi-component transform defines
       an extra output component, which is created by a second transform
       block in the single multi-component transform stage.
          This extra transform block is described by the second record in
       each of `Mstage_collections' and `Mstage_xforms'; it takes only 1 input
       and 1 output and uses a null-transform (2'nd field in the second record
       of `Mstage_xforms:I16' is 0).  This means that the extra transform
       block simply passes its input through to its output, adding the
       offset described by `Mvector_size:I2' and `Mvector_coeffs:I2' (3'rd
       field of the second recrod in `Mstage_xforms:I16' is 2).
          The bottom line is that the 4'th output component is simply a
       replica of the 1'st raw codestream component -- the Y (luminance)
       component.  In order, the output components are R, G, B and Y.
    -- This example shows how multi-component transforms can have more
       output components than the number of codestream components -- i.e.
       the components which are actually encoded.  In fact, they can also
       have fewer components.  When confronted with this situation, the
       "kdu_compress" example associates the input image file's N components
       (N=3 here) with the first N output image components, and then figures
       out how to work back through the multi-component transform network,
       inverting or partially inverting an appropriate subset of the
       transform blocks so as to obtain the codestream components which
       must be encoded.  If there is a way of doing this, Kakadu should
       be able to find it.

Ai) kdu_compress -i image.ppm -o image.jpx  -jp2_space sRGB
                 Mcomponents=3  Creversible=yes
                 Sprecision=8,8,8  Ssigned=no,yes,yes  Mmatrix_size:I7=12
                 Mmatrix_coeffs:I7=1,1,4,0,1,-1,1,0,-1,0,0,1
                 Mvector_size:I1=3  Mvector_coeffs:I1=128,128,128
                 Mstage_inputs:I25={1,1},{2,2},{0,0}
                 Mstage_outputs:I25={2,2},{0,0},{1,1}
                 Mstage_collections:I25={3,3}
                 Mstage_xforms:I25={MATRIX,7,1,1,0}
                 Mnum_stages=1  Mstages=25
    -- Same as example Af), except that processing is performed reversibly
       and the Part-1 RCT (reversible colour transform) is implemented as a
       multi-component transform to demonstrate reversible matrix
       decorrelation transforms.
    -- To understand the reversible decorrelation transform block, observe
       firstly that the coefficients from `Mmatrix_coeffs:I7' belong to
       the following 4x3 array:
                        | 1   1   4 |
                    M = | 0   1  -1 |
                        | 1   0  -1 |
                        | 0   0   1 |
       Let I0, I1 and I2 denote the inputs to this transform block.  The
       reversible transform operator transforms these inputs into outputs
       via the following steps (one step per row in the matrix, M):
          i)   I2 <- I2 - round[(1*I0 +  1*I1) / 4]  = I2 - round((I0+I1)/4)
          ii)  I1 <- I1 - round[(0*I0 + -1*I2) / 1]  = I1 + I2
          iii) I0 <- I0 - round[(0*I0 + -1*I2) / 1]  = I0 + I2
          iV)  I2 <- I2 - round[(0*I0 +  0*I1) / 1]  = I2
       Noting that `Mstage_inputs:I25' associates the block inputs with
       the raw codestream components I0 -> C1=Db, I1 -> C2=Dr, I2 -> C0=Y,
       and `Mstage_outputs:I25' associates the block outputs with stage
       output components I0 -> M2=B, I1 -> M0=R, I2 -> M1=G, the above
       steps can be written as
          i)   G <- Y - round((Db + Dr)/4)
          ii)  R <- Dr + G
          iii) B <- Db + G
          iV)  G <- G
       which is exactly the Part-1 RCT transform mapping YDbDr to RGB -- of
       course, the fourth step does nothing here, but reversible
       multi-component decorrelation transforms require this final step.
    -- For a complete description of reversible multi-component decorrelation
       transforms, consult Part-2 of the JPEG2000 standard, or the interface
       description for Kakadu function `kdu_tile::get_mct_rxform_info'.

Aj) kdu_compress -i catscan.rawl*35@524288 -o catscan.jpx -jpx_layers *
                 -jpx_space sLUM Creversible=yes Sdims={512,512} Clayers=16
                 Mcomponents=35  Nsigned=no  Nprecision=12
                 Sprecision=12,12,12,12,12,13  Ssigned=no,no,no,no,no,yes
                 Mvector_size:I4=35 Mvector_coeffs:I4=2048
                 Mstage_inputs:I25={0,34}  Mstage_outputs:I25={0,34}
                 Mstage_collections:I25={35,35}
                 Mstage_xforms:I25={DWT,1,4,3,0}
                 Mnum_stages=1  Mstages=25
    -- Compresses a medical volume consisting of 35 slices, each 512x512,
       represented in raw little-endian format with 12-bits per sample,
       packed into 2 bytes per sample.  This example follows example (x)
       above, but adds a multi-component transform, which is implemented
       using a 3 level DWT, based on the 5/3 reversible kernel (the kernel-id
       is 1, which is found in the second field of the `Mstage_xforms' record.
    -- To decode the above parameter attributes, note that:
       a) There is only one multi-component transform stage, whose instance
          index is 25 (this is the I25 suffix found on the descriptive
          attributes for this stage).  The value 25 is entirely arbitrary.  I
          picked it to make things interesting.  There can, in general, be
          any number of transform stages.
       b) The single transform stage consists of only one transform block,
          defined by the `Mstage_xforms:I25' attribute -- there can be
          any number of transform blocks, in general.
       c) This block takes 35 input components and produces 35 output
          components, as indicated by the `Mstage_collections:I25' attribute.
       d) The stage inputs and stage outputs are not permuted in this example;
          they are enumerated as 0-34 in each case, as given by the
          `Mstage_inputs:I25' and `Mstage_outputs:I25' attributes.
       e) The transform block itself is implemented using a DWT, whose kernel
          ID is 1 (this is the Part-1 5/3 reversible DWT kernel).  Block
          outputs are added to the offset vector whose instance index is 4
          (as given by `Mvector_size:I4' and `Mvector_coeffs:I4') and the
          DWT has 3 levels.  The final field in the `Mstage_xforms' record
          is set to 0, meaning that the canvas origin for the multi-component
          DWT is to be taken as 0.
       f) Since a multi-component transform is being used, the precision
          and signed/unsigned properties of the final decompressed (or
          original compressed) image components are given by `Mprecision'
          and `Msigned', while their number is given by `Mcomponents', but
          Mprecision and Msigned should not be specified explicitly; instead,
          they are specified via `Nprecision' and `Nsigned' as seen above.
          The reason for this is that there can, in general, be an
          additional non-linear transform between the MCT output components
      	  (with Mxxx attributes) and the final component outputs (with Nxxx
      	  attributes) that may modify the precision/signed properties.	The
      	  internal machinery can derive the Mxxx attributes from the Nxxx
          attributes, but not the other way around.
       g) The `Sprecision' and `Ssigned' attributes record the precision
          and signed/unsigned characteristics of what we call the codestream
          components -- i.e., the components which are obtained by block
          decoding and spatial inverse wavelet transformation.  In this
          case, the first 5 are low-pass subband components, at the bottom
          of the DWT tree; the next 4 are high-pass subband components
          from level 3; then come 9 high-pass components from level 2 of
          the DWT; and finally the 17 high-pass components belonging to
          the first DWT level.  DWT normalization conventions for both
          reversible and irreversible multi-component transforms dictate
          that all high-pass subbands have a passband gain of 2, while
          low-pass subbands have a passband gain of 1.  This is why all
          but the first 5 `Sprecision' values have an extra bit -- remember
          that missing entries in the `Sprecision' and `Ssigned' arrays
          are obtained by replicating the last supplied value.

Ak) kdu_compress -i catscan.rawl*35@524288 -o catscan.jpx -jpx_layers *
                 -jpx_space sLUM Sdims={512,512} Clayers=14 -rate 70
                 Mcomponents=35  Nsigned=no  Nprecision=12
                 Sprecision=12,12,12,12,12,13  Ssigned=no,no,no,no,no,yes
                 Kkernels:I2=I2X2 Mvector_size:I4=35 Mvector_coeffs:I4=2048
                 Mstage_inputs:I25={0,34}  Mstage_outputs:I25={0,34}
                 Mstage_collections:I25={35,35}
                 Mstage_xforms:I25={DWT,2,4,3,0}
                 Mnum_stages=1  Mstages=25
    -- Same as example Ai), except in this case the compression processes
       are irreversible, and an irreversible Haar wavelet transform is
       used, identified via the Kkernels attribute, having instance
       index 2 (i.e., ":I2").  The Haar transform has 2-tap low- and high-pass
       filters.
    -- Note that "kdu_compress" consistently expresses bit-rate in terms
       of bits-per-pixel.  In this case, each pixel is associated with 35
       image planes, so "-rate 70" sets the maximum bit-rate to 2 bits
       per sample.

Al) kdu_compress -i confocal.ppm*12@786597 -o confocal.jpx -jpx_layers *
                 -jpx_space sRGB Cblk={32,32} Cprecincts={64,64}
                 ORGgen_plt=yes Corder=RPCL Clayers=12 -rate 24
                 Mcomponents=36 Sprecision=8,8,8,9,9,9,9,9,9,9,9,9,8
                 Ssigned=no,no,no,yes  Kkernels:I2=I2X2 Mmatrix_size:I7=9
                 Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
                 Mvector_size:I7=3  Mvector_coeffs:I7=128,128,128
                 Mstage_inputs:I25={0,35}  Mstage_outputs:I25={0,35}
                 Mstage_collections:I25={12,12},{24,24}
                 Mstage_xforms:I25={DWT,2,0,2,0},{MATRIX,0,0,0,0}
                 Mstage_inputs:I26={0,0},{12,13},{1,1},{14,15},{2,2},{16,17},
                                  {3,3},{18,19},{4,4},{20,21},{5,5},{22,23},
                                  {6,6},{24,25},{7,7},{26,27},{8,8},{28,29},
                                  {9,9},{30,31},{10,10},{32,33},{11,11},{34,35}
                 Mstage_outputs:I26={0,35}
                 Mstage_collections:I26={3,3},{3,3},{3,3},{3,3},{3,3},{3,3},
                                        {3,3},{3,3},{3,3},{3,3},{3,3},{3,3}
                 Mstage_xforms:I26={MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                                   {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                                   {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                                   {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                                   {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                                   {MATRIX,7,7,0,0},{MATRIX,7,7,0,0}
                 Mnum_stages=2  Mstages=25,26
    -- This real doozy of an example can be used to compress a sequence
       of 12 related colour images; these might be colour scans from a
       confocal microscope at consecutive focal depths, for example.  The
       original 12 colour images are found in a single file, "confocal.ppm",
       which is actually a concatenation of 12 PPM files, each of size
       786597 bytes.  12 JPX compositing layers will be created, each
       having the sRGB colour space.  In the example, two multi-component
       transform stages are used.  These stages are most easily understood
       by working backwards from the second stage.
        * The second stage has 12 transform blocks, each of which implements
          the conventional YCbCr to RGB transform, producing 12 RGB triplets
          (with appropriate offsets to make unsigned data) from the 36 input
          components to the stage.  The luminance inputs to these 12
          transform blocks are derived from outputs 0 through 11 from the
          first stage.  The chrominance inputs are derived from outputs
          12 through 35 (in pairs) from the first stage.
        * The first stage has 2 transform blocks.  The first is a DWT block
          with 2 levels, which implements the irreversible Haar (2x2)
          transform.  It synthesizes the 12 luminance components from its
          12 subband inputs, the first 3 of which are low-pass luminance
          subbands, followed by 3 high-pass luminance subbands from the
          lowest DWT level and then 6 high-pass luminance subbands from
          the first DWT level.  The chrominance components are passed
          straight through the first stage its NULL transform block.
    -- All in all, then, this example employs the conventional YCbCr
       transform to exploit correlation amongst the colour channels in
       each image, while it uses a 2 level Haar wavelet transform to
       exploit correlation amongst the luminance channels of successive
       images.
    -- Try creating an image like this and viewing it with "kdu_show".  You
       will also find you can serve it up beautifully using "kdu_server" for
       terrific remote browsing experience.

Ba) kdu_compress -i image.tif -o out.jpx -rate 3 Cmodes=BYPASS|BYPASS_E2
    -- Demonstrates use of the new "fast mode" that is the subject of
       Ammendment 4 to IS15444-2.  The fast mode is actually 3 modes that
       control the point at which the BYPASS coding option is introduced into
       each code-block.  These three modes are correspond to combinations of
       the BYPASS_E1 and BYPASS_E2 flags, in which at least one of these
       flags is present and the BYPASS flag is also supplied (otherwise the
       other mode flags will have no effect).
    -- In our experience, the most useful option is probably BYPASS_E2, since
       it yields substantial speedups for both compression and decompression
       while usually incurring very little loss in compression efficiency.
    -- You should be aware of the fact that the "fast mode" requires the
       compressed codestream to be marked as a Part-2 codestream, which
       means that it must either be written as a raw codestream or embedded
       in a JPX file.  It cannot be embedded in JP2 or MJ2 files, since
       these allow only Part-1 codestreams.

Ca) kdu_compress -i image.tif -o out.jpx -fprec 16F5 Creversible=yes Clayers=20
    -- Demonstrates the lossless compression of floating point source data
       (IEEE half-floats in this case).
    -- The source image in this case might already have a floating point
       representation (TIFF files can store floating point samples), but it
       might hold integer-valued samples.  In all cases, the source sample
       values are converted to half-floats.
    -- The "-fprec" argument has many uses and has been part of the
       kdu_compress application for many years.  Its name is intended to
       suggest "forced precision".  Common uses of this argument involve only
       the forced precision, with an optional M or L suffix to indicate
       whether the precision conversions are to be achieved by aligning
       MSB's or LSB's.
    -- In this example, the "F" character in the "-fprec" argument indicates
       "floating-point" and the suffix of 5 indicates that the floating-point
       representation should involve 5 exponent bits.  The MSB of all float
       formats is the sign bit, even though it is not used for unsigned
       original content, which leaves 10 mantissa bits in the LSB's in
       this case.  This is exactly the half-float representation.
    -- Note that the floating-point "-fprec" expression does not just
       convert source samples to floats.  It causes the bit-patterns of
       these floating point representations to be passed through to the
       compression machinery as if they were integers (16-bit integers in
       this case).  It also causes a non-linear point transform (NLT) to be
       defined at the code-stream level with type code NLType=UMAG (if the
       source samples are unsigned) or NLType=SMAG (if the source samples
       are signed).  These both map to the NLT sign-magnitude point
       transform which is important for efficient compression.  The
       kdu_compress application also then adds a Pixel Format (pxfm) box
       to the output JPX file, which identifies the exact format of the
       floating point representation.
    -- Note that in this case, true lossless compression of the half-floats
       is performed, but of course there are many quality layers so you
       can extract reduced quality renditions in the usual way, serve them
       via JPIP or whatever you like.  By contrast, simply reading half-floats
       and converting them to integers for regular compression would not
       generally allow truly lossless recovery.
    -- This example can be understood as an introduction to HDR compression,
       since true floating-point compression, with the aid of the NLType=UMAG
       or SMAG options and a Pixel Format box, is especially suitable for
       the compression of HDR (High Dynamic Range) imagery.
    -- It is worth noting that the "out.jpx" file produced here, or by any
       of the examples below, can be opened using "kdu_show" or rendered
       using "kdu_render", producing exactly the rendered imagery that one
       should expect -- i.e., the UMAG/SMAG point transform and Pixel Format
       box will be used to correctly interpret decompressed results.
    -- Note that the source floating point values may be signed or unsigned.
       If they are unsigned, the sign bit is effectively not used (clipping
       occurs to prevent negative numbers being used).  Most applications
       are expected to use signed representations.  Where unsigned
       floating point data is compressed, the codestream's `Ssigned' attribute
       will be YES (i.e., true) and the sign bit is important.  However,
       you should be aware that wherever -ve to +ve or +ve to -ve transitions
       in the source imagery occur abruptly, true float compression can
       produce unpleasant ringing effects when the compressed imagery is
       viewed at reduced resolutions.  This is because the small amount of
       ringing in the wavelet transform basis functions gets massively
       amplified in locations where +ve to -ve or -ve to +ve transitions
       occur, except where the corresponding floating point exponents are
       very small (i.e., the transitions occur in regions of very low
       absolute value).  This is an inescapable consequence of the floating
       point representation, but its severity can be diminished by choosing
       the smallest exponent bit-depth that you can get away with in the
       floating point representation.  If you have signed short float
       source data, but you know that you do not need the full 5 bits of
       exponent precision for your application, we recommend reducing the
       number of exponent bits to 4 or (preferably) 3.  A 16F3 representation
       is still very useful for compressing high dynamic range content, since
       it allows compressed values to exceed the nominal dynamic range by
       a factor of 16, while fully preserving the original mantissa of an
       IEEE half-float for values as small as 1/32 of the nominal dynamic
       range.
Cb) kdu_compress -i image.tif -o out.jpx -fprec 28F8 Creversible=yes Clayers=20
    -- Similar to the above, but compresses 28-bit floating point values with
       an 8-bit exponent.  These are essentially IEEE single precision floats
       but with 4 LSB's dropped from the mantissa.  Again, true lossless
       compression is possible.
    -- As noted above, the use of large exponents can adversely impact the
       visual quality of reduced resolution renderings of the content if
       your original floating point data was signed, with +ve <--> -ve
       transitions of large amplitude.  We caution against the use of
       signed representations if possible, except where they are used to
       preserve small -ve excursions in otherwise positive data.  We also
       recommend using reduced precision exponents, wherever possible.
Cc) kdu_compress -i image.tif -o out.jpx -fprec 32F8 Qstep=0.00001 -rate 4
    -- Similar to the above, but irreversible (lossy) compression is
       performed, using the CDF 9/7 DWT instead of the integer LeGall 5/3 DWT.
    -- The content here is compressed directly as true single precision IEEE
       floats, but note that the compression cannot generally be numerically
       lossless.
    -- You need to be careful of irreversible compression of true floats
       (specified via "-fprec <P>F<E>" patterns).  In this case the floating
       point bit patterns that are subjected to sign-magnitude to 2's
       complement point transformation and the irreversible transformation,
       quantization and coding, involve a leading sign bit, 8 exponent bits
       and then 23 mantissa bits.  What this means is that the most
       significant mantissa bit appears in the 10'th bit position, so to
       achieve small quantization errors a very small base quantization step
       size is required (hence the small Qstep value used here).  You can
       play around with examples like these without the "-rate" constraint,
       to see how small the quantization step size needs to be to get high
       quality outputs.
    -- Beyond the quantization effects, the limited (although very high)
       numerical precision of the internal implementation introduces its
       own small quantization errors which can subject the least significant
       bits of the original floating point mantissa to numerical noise.
    -- Taking these things into account, we suggest that you use as few
       exponent bits as you can get away with for an application.  Although
       it is tempting to use the full 8 exponent bits of an IEEE single
       precision floating-point source representation, and it will work,
       we strongly advise against the use of such large exponents unless
       they are absolutely critical.
Cd) kdu_compress -i image.tif -o out.jpx -fprec 24F4 Qstep=0.00001
    -rate 4 Clayers=20
    -- Taking into account the advice given in example (Cc), this can be a
       good way to compress the information found in a high dynamic
       range image, allowing for very low relative quantization errors.
       The representation involves 19 mantissa bits, which is about as
       many as are likely to be largely free from numerical
       processing noise in the high precision irreversible processing
       pipeline offered by Kakadu.
    -- The nominal maximum intensity for images compressed as true floats
       is 1.0.  The minimum normalized exponent associated with the
       representation used here is -7, while the maximum exponent is 7,
       without encountering infinities and NaN's.  This means that the
       representation is accurate to one part in 2^20 for intensities as
       small as 2^{-7}.  For lower intensities, the mantissa becomes
       denormalized, so that the smallest non-zero magnitude is
       2^{-8}*2*{-19} = 2^{-27}.  Meanwhile, the largest representable
       magnitude is almost 2*8 times the nominal maximum amplitude of 1.0,
       allowing for rather extreme super-luminous values and an overall
       dynamic range of 2^35, which is considerably better than anything
       Kakadu can give you with regular linear compression.  The
       representation is roughly logarithmic over the range 2^{-8} to 2^8.
       Certainly, you can get much larger dynamic range with 5 exponent
       bits, but you should always aim to use as few exponent bits as
       you can reasonably get away with for your application.
Ce) kdu_compress -i image.tif -o out.jpx
    NLTmake={LOG,0.01,0,256},{IGAMMA,2.4,0.055,256}
    -- Demonstrates the use of other types of non-linear point transform (NLT).
    -- The NLTmake attribute provides a great way to construct these
       transforms, since it allows you to build them from cascades of
       forward/inverse gamma operators and forward/inverse log-like functions.
    -- The sequence of steps expressed via the NLTMake parameters, is
       ordered from the perspective of the decompressor.  For a compressor's
       perspective, the first step is to apply an inverse gamma operator,
       inverting a gamma function with gamma=2.4 and beta=0.055 (this is the
       standard sRGB gamma curve).  The second step is to apply a
       log-like transformation, converting the linear data obtained after
       inverting the SRGB gamma function into a log representation
       via |y| = A*(|x|/0.01) if |x| <= 0.01, else |y| = A*(1+log(x/0.01)),
       where A = 1 / (1-log(0.01)) ensures that x \in [0,1] is mapped
       reversibly to y \in [0,1].
    -- Log-like transforms are widely used in scientific imaging
       applications.
    -- One nice property of the log-like representation is that the
       samples that scaling the input samples by a alpha is equivalent to
       adding A*log(alpha) to the samples that are actually subjected to
       spatial DWT, quantization and coding operations, except where
       the original or scaled input samples were smaller than the 0.01
       threshold (you can use a different one) that separates the linear
       and logarithmic regimes in the transformation.  This property
       (scaling converts to addition) is beneficial because the scaling
       factor moves entirely into the LL band after wavelet transformation,
       having no impact on the way in which detail bands are compressed.
       The same is true for scaling factors which vary spatially, but
       only very slowly.
Cf) kdu_compress -i 01.tif,02.tif,03.tif,04.tif,05.tif,06.tif,
                    07.tif,08.tif,09.tiff,10.tif,11.tif,12.tif
      -o out.jpx -jpx_layers \* -jpx_space sRGB Mcomponents=36
      Sprecision=8,8,8,9,9,9,9,9,9,9,9,9,
                 8,8,8,9,9,9,9,9,9,9,9,9,
                 8,8,8,9,9,9,9,9,9,9,9,9
      Ssigned=no,no,no,yes Kkernels:I2=I2X2 Mmatrix_size:I7=9
      Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
      Mvector_size:I7=3 Mvector_coeffs:I7=128,128,128
      Mstage_inputs:I25={0,35} Mstage_outputs:I25={0,35}
      Mstage_collections:I25={12,12},{12,12},{12,12}
      Mstage_xforms:I25={DWT,2,0,2,0},{DWT,2,0,2,0},{DWT,2,0,2,0}
      Mstage_inputs:I26={0,0},{12,12},{24,24},{1,1},{13,13},{25,25},{2,2},
                        {14,14},{26,26},{3,3},{15,15},{27,27},{4,4},{16,16},
                        {28,28},{5,5},{17,17},{29,29},{6,6},{18,18},{30,30},
                        {7,7},{19,19},{31,31},{8,8},{20,20},{32,32},{9,9},
                        {21,21},{33,33},{10,10},{22,22},{34,34},{11,11},
                        {23,23},{35,35}
      Mstage_outputs:I26={0,35}
      Mstage_collections:I26={3,3},{3,3},{3,3},{3,3},{3,3},{3,3},{3,3},{3,3},
                             {3,3},{3,3},{3,3},{3,3}
      Mstage_xforms:I26={MATRIX,7,7,0,0},{MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                        {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                        {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},{MATRIX,7,7,0,0},
                        {MATRIX,7,7,0,0},{MATRIX,7,7,0,0},{MATRIX,7,7,0,0}
      Mnum_stages=2  Mstages=25,26 Qstep=0.001 -rate 6,3,1.5,0.75,0.375
      NLTmake={LOG,0.01,0,256},{IGAMMA,2.4,0.055,256}
    -- This is a real doozy of an example.  It uses both Part-2
       multi-component transforsm and Part-2 non-linear point transforms.
    -- The way to understand it is by recognizing that during decompression
       the non-linear point transform occurs last.  So during compression,
       the first thing that happens is that standard sRGB gamma function is
       inverted (producing linear RGB) and then a log-like transform is
       applied, exactly as described in the previous example.  After this,
       the multi-component transform applies a 2 level Haar wavelet transform
       across corresponding colour planes of the 12 colour images, and also
       applies a decorrelating (RGB to YCbCr type) colour transform to
       produce codestream components that are then subjected to spatial 
       DWT (default irreversible CDF 9/7 used here), followed by quantization
       and coding.  During decompression all of this is reversed.
    -- As mentioned above the Non-Linear Point transform here has the
       property that if the linear inensities for an image can be understood
       as the product of a detailed image and a slowly varying scaling image,
       the log-like transform converts this to the sum of the logs of the
       detailed and slowly varying scaling image.  Now suppose all 12 input
       images are ver similar, apart from slowly varying illumination changes.
       Then the multi-component Haar transfom will eliminate the detailed
       imagery from all but its LL componen and the spatial DWT will
       eliminate the slowly varying scale factor component from all but
       its lowest frequency spatial subbands, leaving the key information
       for the complete set of 12 images compacted into a small number of
       subbands.  Essentially, where a collection of images have significant
       changes in illumination conditions, you should find that
       multi-component transforms that compress the images as a volume can
       be more efficient when the source data is mapped to a log-like
       representation.  This is a reason to be interested in custom
       non-linear point transforms such as that used here.

kdu_compress advanced Part-15 (HTJ2K) Features
----------------------------------------------
    These additional examples relate to Part-15 of the JPEG 2000 standard,
    also known as HTJ2K (High Throughput JPEG 2000), or simply JPH.  The
    examples here will grow considerably in the coming months, but first
    we just provide some very simple examples to get you going.
Ha) kdu_compress -i image.tif -o out.jph Creversible=yes
    -- Losslessly compresses the image to a JPH file.
       Use of the fast HT Block coder is automatically introduced here
       due to the use of a JPH file target.  JPH files are almost identical
       to JP2 files, but have a different brand.
    -- Direct HTJ2K encoding is definitely faster than encoding non-HT
       codestreams.  In the example here, lossless encoding of an 8-bit
       per channel source is around 10 times faster, but it can be much
       faster than this again.
    -- The "kdu_compress" demo-app is not capable of demonstrating the true
       throughput achievable by HTJ2K because it reads the image from file,
       using separate "fread" calls for each line, and uses plain
       sample-by-sample conversion operations to transform the source
       samples into the desired internal format.  For a better demonstration
       of very high throughput, the "kdu_buffered_compress",
       "kdu_v_compress" and "kdu_vex_compress" examples are preferred,
       since these read imagery in larger chunks and use vectorized
       sample data conversion operations, via the high level
       `kdu_stripe_compressor' API that automates these processes.  Even
       then, it is extremely unlikely that you can read data fast enough
       from even a very fast SSD to avoid disk I/O being the bottleneck.
       For a high throughput evaluation, you are referred to the advanced
       "kdu_v_compress" usage examples that use a "-frame_reps" argument
       to ensure that data is compressed from memory buffers with minimal
       disk I/O.
Hb) kdu_compress -i image.tif -o out.j2c Creversible=yes Scap=P15
    -- Like the above example, but produces a raw codestream.  By default,
       raw codestreams use the standard Part-1 block coding algorithm, but
       by specifing the P15 flag in the `Scap' attribute, we force all the
       defaults to change to those most appropriate for Part-15.
Hc) kdu_compress -i image.tif -o out.j2c Creversible=yes Cmodes=HT
    -- Generates the same results as the above example.  In this case, we
       explicitly specify use of the HT block coding algorithm of
       Part-15, and this results in automatic configuration of the `P15'
       flag for CAP (capabilities) and so forth.
Hd) kdu_compress -i image.tif -o tmp.jph -rate 2
    -- Similar to (Ha), but specifies a bit-rate target.  Pretty much all
       parameter attributes and command-line arguments work the same way
       for Part-15 as they do for Part-1.
    -- While this examples demonstrate that HTJ2K encoding is much faster
       than regular J2K encoding (assuming a processor for which
       optimizations are available -- e.g., Intel with AVX2), it does
       not demonstrate the maximum possible throughput increase.
       You should receive a printed note (if you did not specify "-quiet")
       suggesting that you use either the "-bstats" argument or specify
       the `Cplex' parameter attribute, both of which act to constrain
       encoding complexity, using dynamically estimated scene complexity
       statistics.  This is only relevant when there is an explicit
       bit-rate target ("-rate") and only one quality layer (Clayers=1).
    -- Nevertheless, You should find that HTJ2K encoding with rate control,
       as above, for a source with 8-bit/channel precision, is around 4 to
       5 times faster than regular J2K encoding to the same conditions.
Hd1) kdu_compress -i image.tif -o tmp.jph Qfactor=85
    -- Demonstrates Q-factor based compression with HTJ2K.
    -- As noted in earlier examples, the JPEG 2000 quality factor is
       intended to have similar meaning to the quality factor commonly
       used to control JPEG, but note that here we are compressing colour
       imagery without any sub-sampling (4:4:4 rather than the 4:2:0
       sub-sampled representation most commonly used with JPEG).
    -- Using a `Qfactor' to control HTJ2K compression, without a separate
       bit-rate constraint, leads to the highest possible encoding
       throughput.
Hd2) kdu_compress -i image.tif -o tmp.jph Qfactor=85 -rgb_to_420
    -- Same as example (Hd1) but uses the 4:2:0 colour representation
       that is most commonly employed with traditional JPEG compression.
       4:2:0 tends to be a little more efficient, in the sense of
       minimizing visual distortion for a given bit-rate, but at very
       high qualities, the loss of chrominance resolution associated
       with 4:2:0 becomes more obvious, depending on the content.  This
       is why professional photographers tend to prefer 4:4:4.
He) kdu_compress -i image.ppm -o tmp.j2c -rate 2 Cmodes=HT Cmodes:C1=0
    -- Demonstrates the generation of a Part-15 codestream in which the
       HT block coder is used for 2 of the image components, while
       component 1 (first chrominance component in this case) uses the
       Part-1 block coding algorithm.
    -- It is really worth opening this file using kdu_show, if you have it,
       using the File->Properties menue to see what parameter attributes
       have been generated for this codestream.  Alternatively, pass the
       "-record" option to kdu_expand.
Hf) kdu_compress -i image.ppm -o tmp.jph -rate 2 Clayers=8
    -- This demonstrates that HTJ2K codestreams can have multiple quality
       layers, even if they exclusively use the HT block coding algorithm,
       which is not significantly embedded -- i.e., not really quality
       scalable.
    -- Quality layer boundaries and all rate control here are done in the
       same way as they are for non-HT codestreams that use the original
       JPEG 2000 block coding algorithm.  Quality layer boundaries are
       correctly recorded in the codestream's packet headers, but the
       content is not actually quality scalable.  This means that if you
       try to decode with a reduced number of quality layers (try it in
       "kdu_show" using the "<" accelerator), most or all of the code-block
       data decodes to 0, which can produce some rather weird artifacts,
       but you are not expected to do this.  The reason for preserving
       quality layer boundaries in HTJ2K codestream that use only the
       non-scalable HT block coder, is to facilitate subsequent transcoding.
    -- Take a look at the advanced "kdu_transcode" usage examples for HTJ2K,
       which show how you can transcode the HT-only output to a codestream
       that uses the original (non-HT) block coding algorithm of JPEG 2000
       and recovers all quality layer boundaries perfectly.  Another
       transcoding example shows how the codesteram can efficiently be
       transcoded to one at lower quality by discarding quality layers
       during transcoding, while retaining the HT-only block coding
       algorithm -- this is much better than completely decoding the HTJ2K
       image and re-encoding at a lower quality, since the rate control
       process has been done once for all quality layers and the resulting
       layer boundaries, recorded in the codestream, are then re-used each
       time transcoding to a different quality is required.
Hg) kdu_compress -i image.ppm -o tmp.jph -rate 4 Cplex={6,EST,0.25,0}
    -- This example shows you how to make the encoding go faster, using
       the "Cplex" (read as "Complexity Constraints") parameter attribute.
       This attribute provides various methods for managing the set of
       coding passes that are generated by the block encoder, but the
       most powerful and versatile is the "EST" (read as "Estimated")
       method.  Basically, as subband samples are generated by DWT
       analysis, they are used to compute statistics that are forwarded
       to the core codestream management machinery for the determination
       of suitable complexity constraints, based on the target compressed
       size -- specified here via "-rate".
    -- The first parameter to the "Cplex" attribute identifies the
       maximum number of coding passes to perform for each code-block,
       for which 6 is a good value.  The HT block coder produces coding
       passes that are organized into so-called HT-Sets, each of which
       has 3 passes, so it makes sense to request a whole number of
       HT-Sets.  You can get away with a single set (3 passes), but will
       lose some coding efficiency; there is pretty much no value in
       encoding more than 2 HT-Sets, so 6 is the natural value for this
       parameter.  Values that are not multiples of 3 could be useful
       for fine tuning the trade-off between computation and coding
       efficiency.  For example, a value of 4 can prove useful, but it
       will not be a lot faster than 6.  A value of 1 can be supplied
       here to see just how conservative the complexity constraint
       generation mechanism is - you should get a result which is
       distinctly smaller than the target compressed size when only
       1 pass is generated.
    -- The third parameter indicates how conservative the complexity
       constraint generation machinery should be in determining where
       the coarsest coding pass should be.  This parameter takes values
       in the range 0 to 1, where 0 leaves the least slack and 1 leaves
       the most slack between the target compressed size and the
       size associated with keeping only the first (coarsest) coding
       pass from every code-block.  Even the value 0 should still be
       conservative, but less so.  A value of 0.25 is a good choice in
       general, but if you do intend to generate only one HT-Set
       (i.e., just 3 passes instead of 6), we recommend a smaller value
       for the third parameter, such as 0.05.  If you are prepared to
       generate 3 HT-Sets (quite a lot), then it makes sense to choose
       a larger value for the third parameter, such as 0.75, to extract
       some small benefit from the extra coding passes you are
       generating -- the idea is to ensure that the generated coding
       passes nicely straddle the optimal truncation point that cannot
       be selected until the codestream content is flushed.
    -- The fourth parameter allows you to insert a controlled delay
       between the point at which subband samples (and hence there
       statistics) become available and the point at which complexity
       constraint decisions are made for the affected code-blocks.
       The value 0 here implies no delay, which means that there is no
       additional memory introduced into the data processing pipeline,
       but complexity constraint decisions rely more heavily upon
       forecasting the statistics of future subband samples that have
       not yet been seen.  Despite this, using the minimal memory
       configuration here still usually produces very good results in our
       experience.  See below for more examples, though.
    -- If you did not specify "-quiet", you should have received an
       advisory note recommending that you use the "-bstats" option,
       providing background scene complexity statistics to make the Cplex-EST
       algorithm much more robust.  See below for examples of this.
    -- NOTE that the "kdu_compress" demo-app is throughput limited by
       its line-by-line approach to reading image samples from file
       via "fread".  It is very likely that the throughput of these
       examples is actually constrained by file I/O, rather than
       processing, even if you have an ultra-fast SSD, since "fread"
       throughput is really quite limited.  For a better test of
       throughput, you are recommended to use the "kdu_buffered_compress"
       demo-app -- see the corresponding usage examples for that
       application. 
Hh) kdu_compress -i image.ppm -o tmp.jph -rate 4 Cplex={6,EST,0.25,-1}
    -- Similar to above, but demonstrates a maximum memory configuration.
       As noted above, the last argument to the "Cplex" attribute controls
       the amount of delay (or buffering) between subband sample generation
       and the point at which complexity constraints are generated for the
       corresponding code-blocks, which must happen before the encoding
       jobs are scheduled to run.  Supplying a negative value for this
       parameter provides a convenient way to specify large delays.  The
       value -1 means that the effective delay is 1 line less than the
       full tile height, which is essentially the largest sensible delay.
       A value like "-128" might make more sense with the default code-block
       height of 64, noting that code-blocks of height 64 in the highest
       resolution level span 128 image lines, so -128 means that the
       encoding can commence once there is only one row of code-blocks'
       subband samples left to produce.
    -- Note that the "Cplex" attribute can be tile-specific and the
       EST algorithm performs its estimates on a tile-by-tile basis.
       However, if there are multiple tiles, statistics from previously
       processed tiles are automatically shared with subsequent tiles
       to improve their forecasts.  You could, for example, specify a
       larger delay value (4'th parameter) for the first row of tiles
       than subsequent rows of tiles, so as to avoid the need to ever
       forecast statistics based on limited observations.  In practice,
       however, we discourage the use of tiles in general with
       JPEG 2000, since they are not necessary unless one intends to
       compress enormous images running into the tens or hundreds of
       Giga-pixels, or Tera-pixels.
    -- Finally, we note that the delay is internally capped at a large
       yet not infinite value (e.g., 64K image lines).
Hi) kdu_compress -i image.ppm -o tmp.jph -rate 4 -bstats stats.txt
    -- Does the same thing as example (Hg), automatically setting the
       recommended "Cplex={6,EST,0.25,0}" option for you, but uses the
       background scene complexity statistics found in file "stats.txt"
       to make complexity forecasts in this minimum-memory mode of the
       Cplex-EST algorithm more robust to spatial complexity variations in
       the image -- much more robust!
    -- To create the background statistics file "stats.txt" here, you
       can use almost exactly the same command, applying it to any number
       of training images, with a slightly different format for the
       "-bstats" parameter string.  Specifically, to collect training
       statistics, do the following:
          kdu_compress -i im1.ppm -rate 4 -bstats -,stats.txt
          kdu_compress -i im2.ppm -rate 4 -bstats stats.txt,stats.txt
          kdu_compress -i im3.ppm -rate 4 -bstats stats.txt,stats.txt
          ...
       Note that the collected statistics do not depend on the "-rate"
       parameter, and they do not even depend strongly on the source image
       sample bit-depths, but they do depend upon the coding and
       quantization parameters, as well as the number of image (colour)
       components.  You will get warning messages if the statistics are
       incompatible or partially incompatible with your coding configuration.
    -- While training background statistics might seem like an unfortunate
       complication, we note that the Cplex-EST forecast methodology is only
       weakly dependent on these background statistics, so that you can
       use statistics trained with very different images and should still
       find it beneficial.  Just a single training image may well be
       enough.
Hi1) kdu_compress -i image.ppm -o tmp.jph -rate 4 -bstats stats.txt Qfactor=90
    -- Just like example (Hi), but adds a `Qfactor' type constraint to image
       quality.
    -- This example really puts lots of things together at once:
       Quantization parameters are chosen based on the quality factor;
       Visual weights are introduced automatically in a way that is sensitive
       also to the quality factor (less aggressive at very high qualities);
       Overall compressed size is also constrained based by the "-rate"
       constraint, to 4 bits/pixel; and Cplex-EST complexity constraints also
       apply, using background statistics to maximize the robustness of the
       complexity-constrained process that is installed to meet the
       -rate constraint in a rate-distortion optimal way.
Hj) kdu_compress -i image.ppm -o tmp.jph -rate 4 Cplex={3,EST,0.05,0}
    -- Demonstrates a suitable configuration for complexity-constrained
       encoding with just one HT-Set -- see example (Hg) for an
       explanation of these parameter values.
Hk) kdu_compress -i image.ppm -o cbr_out.j2c -rate 2 Corder=PCRL Clevels=5
                 Cprecincts={8,8192},{4,8192},{2,8192} Cblk={4,1024}
                 Cdecomp=B(-:-:-),B(-:-:-),H(-)
                 Qstep=0.0001 Catk=2 Kkernels:I2=I5X3 -no_weights
                 Scbr={1,10} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 8
    -- Demonstrates ultra-low latency image compression.  Of course, the compressed
       target here is a file, but it could be a network interface or something
       else that is able to take advantage of low latency.  The fundamental
       end-to-end latency for this compression setup is just 24 image lines,
       including the latency associated with a constant bit-rate communication
       channel, forward and inverse wavelet transform and the pre-buffering
       required for the Cplex-EST complexity-constraint generation algorithm
       employed here.  The actual latency will be larger due to computation
       delays and operating-system induced latencies that can be considerable,
       but this application really can flush compressed data to the output every
       8 input lines, after a well defined initial delay.
    -- You can eliminate the "-flush_period" option if you like, and the
       codestream flushing process will be deferred to the very end of the image,
       but the codestream that results will be the same, since the `Scbr' option
       enforces the deployment of a rate-control algorithm that always operates
       causally, flush-set by flush-set, no matter when it is deployed.  This
       means that you can also specify a larger "-flush_period" such as 64, to
       run the background flushing job less often, while still generating a
       codestream that is formally compatible with the fundamental latency of
       24 lines, even if the actual latency is higher.  This is because the
       JPEG 2000 rate control process is decoupled from the transformation and
       encoding processes, rather than operating in a tight feedback loop as
       typical video coders do.  In fact the encoder and decoder can flexibly
       adjust their latencies to suit a particular implementation platform,
       right down to the point where the fundamental latency is almost
       achieved, without needing to negotiate any change in the codestream
       structure.
    -- To keep the latency low, two Part-2 features have been employed: the
       5/3 irreversible DWT is introduced via the `Kkernels' and `Catk'
       attributes; and a non-uniform downsampling style has been introduced
       via the `Cdecomp' attribute, so that there are only 2 vertical wavelet
       decomposition levels but 5 horizontal decomposition levels.
    -- For low latency, the spatially progressive codestream sequence idetified
       as PCRL is employed.
    -- The HT block encoder is used for maximum throughput, while the Cplex-EST
       complexity constraint algorithm guarantees that no more than 6 coding
       passes (2 HT-Sets) will be produced by the block encoder, for any
       code-block, prior to the PCRD-opt stage, which executes every 8 lines -- a
       so-called "flush-set".
    -- Notice that the high-memory form of the Cplex-EST algorithm is used (last
       parameter to `Cplex' is -1, not 0).  This is desirable for low latency
       compression, because the Cplex-EST algorithm operates independently on
       each flush-set in the presence of the `Scbr' option, and here the flush-sets
       correspond to just 8 image lines, so that high-memory does not imply very
       much memory (or delay) at all.  You can achieve lower practical latency
       levels with the low-memory Cplex-EST form (last parameter = 0), because
       this allows block encoding to start earlier, but the latency benefit is
       only modest for configurations like this one.  The main benefit of the
       high-memory form of Cplex-EST here is that the coding passes actually
       generated are deduced in a deterministic way from the image sample
       statistics, regardless of the multi-threading behaviour -- this is not
       true of the low-memory form, which otherwise still performs very well.
    -- The "-no_weights" option ensures optimization for MSE (or PSNR), but you
       can leave this off to get a visually optimized result.
    -- Note that this configuration is even more interesting with the
       `kdu_buffered_compress' demo-app, which allows you to hit higher
       througputs and eliminate the impact of source image file reading time
       on throughput measurements.  It is also very interesting for low-latency
       video compression applications, using "kdu_v_compress" or "kdu_vcom_fast".
Hl) kdu_compress -i image.ppm -o cbr_out.j2c -rate 2 Corder=PCRL Clevels=5
                 Cprecincts={32,8192},{16,8192},{8,8192},{4,8192} Cblk={16,256}
                 Cdecomp=B(-:-:-),B(-:-:-),B(-:-:-),H(-) Qstep=0.0001 -no_weights
                 Scbr={1,34} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 32
    -- Very similar to the last example, except that the longer (and usually
       more efficient) 9/7 DWT kernel is used from JPEG 2000 Part-1 (Kakadu's
       default irreversible transform) and there are 3 levels of vertical
       wavelet decomposition (still 5 levels of horizontal decomposition).
    -- The fundamental end-to-end latency in this case is 108 image lines and
       the flush-set size is 32 lines.
    -- This configuration is strongly recommended for applications that require
       low latency, but do not need the ridiculously low latency of the
       configuration in example (Hk).
Hm) kdu_compress -i image.ppm -rgb_to_420 -o cbr_out.jpx -rate 2 Corder=PCRL
                 Clevels=4 Clevels:C0=5
                 Cprecincts={16,8192},{8,8192},{4,8192} Cblk={16,256}
                 Cprecincts:C0={32,8192},{16,8192},{8,8192},{4,8192}
                 Cdecomp=B(-:-:-),B(-:-:-),H(-)
                 Cdecomp:C0=B(-:-:-),B(-:-:-),B(-:-:-),H(-)
                 Qstep=0.0001 -no_weights
                 Scbr={1,34} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 32
    -- Same as the last example, but adjusted for use with 4:2:0 imagery,
       in which hte chrominance channels are sub-sampled by 2 in each direction,
       relative to the luminance channel.  In this case, 420 content is
       constructed automatically from the RGB source image via the "-rgb_to_420"
       option, but you can also just feed luminance and chrominance components
       as separate image files to the "-i" argument -- there are many other
       examples of this above.
    -- Notice that the wavelet decomposition structure for the luminance
       component (C0) add one extra level of decomposition (vertical and
       horizontal) and this levels subband samples are packated into precincts
       that have twice as many lines.  This ensures that all flush-sets represent
       32 luminance lines and 16 chrominance lines each, maintaining an end-to-end
       latency of 108 luminance lines.
    -- The output codestream is embedded in a JPX file here, for convenience of
       rendering, since the JPX file can capture the rendering intent (YCbCr 4:2:0
       content needs to be upsampled and colour converted during rendering).  A
       plain JPH file cannot be used here, because features of JPEG 2000 Part-2
       have been employed, along with those of Part-1 (core) and Part-15 (HTJ2K).
Hn) kdu_compress -i image.ppm -rgb_to_420 -o cbr_out.jpx -rate 2 Corder=PCRL
                 Clevels=5
                 Cprecincts={32,8192},{16,8192},{8,8192},{4,8192} Cblk={16,256}
                 Cdecomp=-(),B(-:-:-),B(-:-:-),H(-)
                 Cdecomp:C0=B(-:-:-),B(-:-:-),B(-:-:-),H(-)
                 Qstep=0.0001 -no_weights
                 Scbr={1,34} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 32
    -- This example is extremely similar to the last one, except that all image
       components now have 5 levels of wavelet decomposition, with the same
       precinct structure, but the first level for the chrominance components
       is a degenerate one (signature "-()" in the `Cdecomp' attribute), which
       does no downsampling and produces no detail subbands.
    -- The difference between the codestreams produced by this example and the
       previous can be experienced during reduced resolution decoding (e.g., 
       using "kdu_expand -reduce 1 ...").  Discarding a resolution level from
       the codestream produced here leaves all 3 components with the same
       dimensions (i.e., half resolution but 4:4:4 sampling).  On the other
       hand, discarding a resolution level from the codestream produced by the
       previous exmaple preserves the original 4:2:0 sampling arrangement, with
       all component dimensions halved.  The present example is almost certainly
       preferable, because it allows 3 levels of resolution scaling, before the
       aspect ratio of the reconstructed image changes due to the use of
       horizontal-only wavelet decomposition at the lowest resolutions.

kdu_buffered_compress
---------------------
  This application offers many of the same features as "kdu_compress".  The
  primary difference is that this application buffers stripes of the input
  image(s) that it reads in memory and passes these stripes to the higher
  level `kdu_stripe_compressor' API, which also takes care of all format
  and buffer reorganization processing.  By contrast, "kdu_compress"
  reads, deinterleaves (if necessary) and converts image samples line by
  line into one of the four fundamental internal representations and passes
  them to the lower level `kdu_multi_analysis' API.

  The `kdu_stripe_compressor' API is strongly recommended for most users,
  since it incorporates highly efficient SIMD data conversion functions for
  X86-family processors that can leverage SSSE3 instructions, but also
  AVX2 and FMA instructions.  In the past, the lower level approach tended
  to offer the greatest opportunities for optimization, but it is most
  likely these days that the `kdu_stripe_compressor' will give you the
  best performance, not only because it already contains vectorized sample
  data tranfer operations, but also because it embodies a very sophisticated
  work flow for processing tiled images that may take some time to reproduce
  using the lower level API's directly.
  
  To keep the code substantially less convoluted than "kdu_compress", this
  demo app offers support for a smaller set of input file formats,
  does not offer JPX file writing or control over colour spaces, and
  does not offer some of the more exotic features of "kdu_compress" such
  as fragmented compression.  Input file formats supported are 8-bit PGM,
  8-bit PPM, BMP, and raw files with up to 16 bits/sample.

 a) kdu_buffered_compress -i image.ppm -o out.jp2 -rate 1
 b) kdu_buffered_compress -i image.ppm -o out.jp2 -rate 1 -stats -quiet
    -- Suppress annoying warnings, but report key statistics such as
       actual compressed bit-rate, working memory and distortion-length
       slope threshold (can be passed to -slope').
 c) kdu_buffered_compress -i image.ppm -o out.jp2 -slope 42755 -stats
    -- Verify that the slope can be used to control compressed size, which
       is ultimately more efficient.  Slope tends to correlate better with
       image quality than does compressed bit-rate.
 d) kdu_buffered_compress -i image.ppm -vrep 8 -slope 42755 -cpu -stats
    -- Automatically replicate the image 8 times vertically, compressing
       the result as one much bigger image.  This is a great way to
       measure throughput performance, because the image is buffered
       up once, after which compression proceeds from memory, eliminating
       file reading as a potential bottleneck.
    -- Note that no output file is provided, which automatically selects
       the internal "null" compressed file target (like /dev/null).
 e) kdu_buffered_compress -i image.ppm -vrep 8 -rate 1 -cpu -stats
       Clevels=10 Cprecincts={256,256},{128,256},{128,128} -flush_period 1024
    -- The "null" compressed data target also happens to support the
       "structured cache" data target interface, which allows codestream
       elements to be written in any order.  This is optimal for incremental
       flushing.  Compare the memory consumption and also overall throughput
       of this example with that in (d) to see the benefits of incremental
       codestream flushing in this context.
 f) kdu_buffered_compress -i image.ppm -o tmp.jp2 -precise Clevels=10 -rate 1
    -- Force the use of an internal floating point representation for the
       data even though the source samples are 8 bits deep.  This can
       be important for huge images with many decomposition levels.
    -- Try using "-precise" with the above examples to see how fast
       Kakadu can perform high precision compression.
    -- If your source data has moderate to high precision and you want to
       process using the lower precision data path, you can select the
       "-fastest" option and see what impact this has.
 g) ... most of the advanced coding options available to "kdu_compress"
    are also available in "kdu_buffered_compress", including Part-2
    features such as multi-component transforms.  However, because this
    app does not write JPX files, and Part-2 codestreams cannot be
    legally embedded in a JP2 file, you will have to write a raw
    codestream (".j2c") when testing such features.  Alternatively, you
    will find it is very easy to add JPX file format support to this
    app -- just follow the steps taken by "kdu_compress" to use
    `jpx_target' instead of `jp2_target'.
 h) kdu_buffered_compress -i red.rawl,green.rawl,blue.rawl -little_endian
    Sprecision=12 Sdims={20480,20480} Ssigned=no -o out.jp2 Corder=RPCL
    ORGtparts=R Cprecincts={256,256} Cuse_precincts=yes Stiles={512,512}
    ORGgen_tlm=8 ORGtpart_interrupts=2 -flush_period 1024
    -tile_concurrency 8 -rate 0.8 -fastest -cpu -quiet
    -- This is a real example that prompted some of the enhancements
       introduced to the core system and the `kdu_stripe_compressor'
       implementation in KDU-7.4.  The generated codestream contains 1600
       tiles, each of size 512x512 (very small in the grand scheme of things).
       To achieve close to 100% utilization of CPU resources on highly
       parallel processing platforms, we have to be careful to keep multiple
       tiles open concurrently, to pass stripes whose height is equal to the
       tile height into the `kdu_stripe_compressor' workhorse.  This happens
       because the default value for `-max_height' is 1024.  If you have
       larger tiles and the codestream still contains a very large number
       of tiles, you might like to experiment with increasing `-max_height'
       to the height of these larger tiles.
    -- The `-tile_concurrency' argument specifies the number of tiles that
       are kept concurrently active, which is more than the single tile
       processing engine that was instantiated by previous (prior to KDU-7.4)
       incarnations of the `kdu_stripe_compressor' object and smaller (in
       this case) than the number of tiles spanned by a single image row.
       This allows for good processor cache utilization and also good thread
       concurrency with little or no idle time experienced by any thread.
    -- In this example, the double buffering height used for the
       multi-threaded DWT processing machinery is automatically selected so
       as to allow each active tile processing engine to buffer up all
       samples in the tile, so that the data can easily be pushed into
       all concurrently active tile processing engines without waiting for
       processing to complete within any of them.  If you are trying to
       replicate the same behaviour in your own implementation, take note
       of the fact that the `env_dbuf_height' argument to the
       `kdu_stripe_compressor::start' function is usually best passed
       as -1, which causes the `kdu_stripe_decompressor' object to select
       good values automatically -- that is what is happening here.
    -- You should find that examples like this are able to push the disk I/O
       capabilities of your platform right to the limit (even with SSD's) so
       that performance is usually limited by disk I/O rather than anything
       else.  Most operating systems cache recent files in memory so you
       may need to run the demo multiple times to discover the true
       throughput of the application.
    -- The "-fastest" argument has been added here for good measure since
       12-bit precision imagery will be compressed using the high precision
       (floating point) data path by default, yet in most cases (especially
       at this bit-rate) the accuracy offered by the fixed-point 16-bit
       data path is more than sufficient for such data.  Feel free to drop
       this option and see what difference it makes (usually not all that
       much, but this will depend on the memory bandwidth of your system
       amongst other things).
    -- Notice that the example does incremental background flushing of
       generated codestream content, which is dimensioned to allow
       codestream content to be flushed after every pair of tile rows have
       been processed.  With only a very small drop in throughput, you can
       increase the "-flush_period" value to four tile heights (2048) in
       order to get more effective rate control, or you can reduce the
       flush period to a single tile height (512) at the risk of less
       effective rate control.  If the incremental flush period becomes too
       large, the overall processing time will increase because there will
       be more data to flush at the end after all imagery has been pushed
       into the processing machinery, and this final flush is single threaded,
       so it's best to keep the `-flush_period' to a modest fraction of the
       overall image height.
    -- You will note that ORGgen_tlm takes the value 8, whereas the maximum
       number of tile-parts into which each tile would naturally be divided
       in accordance with ORGtparts=R should be 6, given that the packet
       progression sequence is resolution dominant (RPCL) and the number of
       distinct resolutions is 6 (because Clevels=5).  The reason for allowing
       2 extra tile-parts in the ORGgen_tlm specification is that incremental
       flushing (-flush_period), in combination with the highly efficient
       multi-threaded processing engine being invoked here, may lead to a
       situation in which some parts of a tile are available while others are
       not yet available, at the point when a background flush actually
       occurs.  Incremental flushing always comes with the possibility that
       a tile may need to be split into parts.  Since it is hard to predict
       exactly how many parts there will be, we add an allowance for these
       (2 in this case) and provide the special `ORGtpart_interrupts'
       attribute (also set to 2) which limits the number of extra tile-parts
       that can be introduced by incremental flushing.  This guarantees that
       the compression will always succeed, but it may cause the flushing
       process to proceed in a non-optimal manner if `ORGtpart_interrupts' is
       set too low.  For reference, you can try setting this parameter to 0
       (and `ORGgen_tlm' equal to 6) and see what happens.  On a sufficiently
       parallel processing platform, you should find that you sometimes get
       warning messages suggesting that your `ORGtpart_interrupts' value is
       smaller than desired, but nonetheless generating a correct result.
       If you try the same thing with `ORGgen_tlm' equal to the minimum value
       of 6 but omitting the `ORGtpart_interrupts' attribute, you should find
       that you occasionally get an error message related to the generation of
       too many tile-parts.  Even without any `ORGgen_tlm' attribute, it
       can happen that incremental flushing might naturally try to generate
       more than the absolute limit of 255 tile-parts for a tile, except that
       the default value of 200 for `ORGtpart_interrupts' should normally
       prevent this, unless you have a weird `ORGtparts' specification.
       The `ORGtpart_interrupts' attribute is new to KDU-7.4, prior to which
       incremental flushing could have generated excessive tile-part
       boundaries that were hard to control.

kdu_buffered_compress advanced Part-15 (HTJ2K) Features
-------------------------------------------------------
    These additional examples relate to Part-15 of the JPEG 2000 standard,
    also known as HTJ2K (High Throughput JPEG 2000), or simply JPH.
    Most of the HTJ2K examples to get you started appear under "kdu_compress",
    but the HT Block coder is so fast that the line-by-line image reading
    paradigm in "kdu_compress" usually becomes the bottleneck, so that
    you cannot properly measure throughput this that application.  Here
    we give you some ideas for measuring throughput.
Ha) kdu_buffered_compress -i big_image.ppm -vrep 8 Qstep=0.001
    -rate 3 -cpu -stats Cmodes=HT
    -- This example aims to eliminate file reading I/O as a bottleneck
       in the measurement of throughput for HTJ2K encoding.  A single
       big image is read into memory and then effectively concatenated
       with itself 8 times over to make an image that is much taller.
    -- Notice that there is no output file.  The compression, rate
       control and codestream generation all takes place, but
       nothing needs to be written to disk.
    -- Running this example with a 13Kx13K RGB source image on a
       late 2016 15" Macbook Pro (2.7GHz 4-core i7 Skylake CPU) yielded a
       throughput of 820 Mega-samples/s.
Hb) kdu_buffered_compress -i big_image.ppm -vrep 8 Qstep=0.001
    -rate 3 -cpu -stats Cmodes=HT
    Corder=PCRL -flush_period 1024
    Cprecincts={256,1024},{256,1024},{256,1024},{128,1024},{64,1024},{32,1024}
    -- Building on the last example, this one avoids deferring the
       post-compression rate-distortion optimization step to the end,
       where it has to run single-threaded.  Instead, the rate control
       and codestream generation processes are performed incrementally,
       roughly every 1000 image lines, so they can mostly take place in
       the background while other threads are busy working to compress
       new content.
    -- Running this example with the same 13Kx13K RGB source image on a
       4-core Skylake i7 Desktop machine with 3.4GHz base clock yielded a
       throughput of 906 Mega-samples/s.
Hc) kdu_buffered_compress -i big_image.ppm -vrep 8 Qstep=0.001
    -rate 3 -cpu -stats Cmodes=HT
    Cplex={6,EST,0.25,0} Corder=PCRL -flush_period 1024
    Cprecincts={256,1024},{256,1024},{256,1024},{128,1024},{64,1024},{32,1024}
    -- This example demonstrates the way to get the highest throughput
       during HTJ2K encoding of a single image.  The key here is the
       "Cplex" coding parameter attribute -- it is a pseudo-attribute
       in that there it does not affect parameters recorded in the
       codestream header.  The Cplex attribute (and in particular its
       EST complexity constraint method) is explained in the "Hxx
       series examples for "kdu_compress" above). 
    -- Running this example with the same 13Kx13K RGB source image on a
       4-core Skylake i7 Desktop machine with 3.4GHz base clock yielded a
       throughput of 1.71 Gsamples/s.
    -- For reference, on the same platform with "Cmodes=0" (i.e., using
       the original JPEG 2000 block coding algorithm), the throughput
       is 162.5 Mega-samples/s with regular Kakadu (not the speed-pack
       version).  This means that there can be more than a 10x end-to-end
       speedup for HTJ2K encoding over regular Kakadu-based JPEG 2000
       encoding at 3 bits/pixel, with full rate control.  Of course, larger
       speedups can be expected at higher bit-rates, with truly enormous
       speedups for lossless compression.
    -- As with "kdu_compress", supplying "Cplex" alone like this will result
       in a printed note recommending that you use the "-bstats" option
       instead, in order to make the Cplex-EST forecasting strategy for
       the complexity of unseen subband samples more robust.  See next
       example.
Hd) kdu_buffered_compress -i big_image.ppm -vrep 8 Qstep=0.001
    -rate 3 -cpu -stats Cmodes=HT
    -bstats stats.txt Corder=PCRL -flush_period 1024
    Cprecincts={256,1024},{256,1024},{256,1024},{128,1024},{64,1024},{32,1024}
    -- Same as example (Hc), except that background scene complexity
       statistics are imported from file "stats.txt" to make the Cplex-EST
       complexity constraint algorithm more robust, without reducing its
       throughput in any significant way.
    -- As exampled in "kdu_compress" example (Hi), you can collect
       background statistics for the "stats.txt" file in a simple way,
       using either kdu_compress or kdu_buffered_compress.  For example,
       you can use the following:
          kdu_buffered_compress -i im1.ppm -rate 4 -bstats -,stats.txt
          kdu_buffered_compress -i im2.ppm -rate 4 -bstats stats.txt,stats.txt
          kdu_buffered_compress -i im3.ppm -rate 4 -bstats stats.txt,stats.txt
          ...

kdu_maketlm
-----------
 a) kdu_maketlm input.j2c output.j2c
 b) kdu_maketlm input.jp2 output.jp2
    -- You can add TLM marker segments to an existing raw code-stream file
       or wrapped JP2 file.  This can be useful for random access into large
       compressed images which have been tiled; it is of marginal value when
       an untiled image has multiple tile-parts.
    -- Starting from v4.3, TLM information can be included directly by the
       codestream generation machinery, which saves resource-hungry file
       reading and re-writing operations.  Note, however, that the
       "kdu_maketlm" facility can often provide a more efficient TLM
       representation, or find a legal TLM representation where none can
       be determined ahead of time by the codestream generation machinery.

kdu_v_compress
--------------
    Accepts similar arguments to `kdu_compress', but (nominally) for video.

    The input format must be one of the following:
      VIX file (wide range of precisions, colour, sub-sampling, frame rates);
          Read the usage statement to find a detailed description of the VIX
          raw video file format, which is superior to YUV since it has a
          header.
      YUV (precisions out to 16 bits per channel, and even RGB-containing
          YUV files, discovered by parsing the filename -- should work for
          most sensibly constructed file names).
      TIFF file (precisions out to 16 bits per channel) with 1 to 4
          components, untiled and uncompressed.
      Sequences of the above files can be automatically concatenated where
      their filenames contain a numeric component immediately preceding the
      file extension -- see usage statement.
    
    The output format must be one of the following:
      1) an MJ2 file (*.mj2) -- conforming to the Motion JPEG2000 standard
      2) a JPB file (*.jpb)  -- elementary broadcast stream, as specified in
                                Annex M of IS15444-1.
      3) a JPX file (*.jpx)  -- uses Compositing Layer Extensions boxes and
                                Multiple Codestream boxes, as defined in
                                IS15444-2/AMD3, adding some sample metadata
      4) an MJC file (*.mjc) -- a simple non-standard compressed video format,
                                developed for illustration purposes, or for
                                piping to other applications.
      5) no output file at all -- all compression is done and codestreams
         are generated for each video frame, but the final file writing step
         is skipped.  The advantage of this is that it allows you to get a
         clearer picture of how fast the compression will be in an application
         where the compressed data is stored in memory, transferred over a
         network, etc.  When compressed results are written to disk, the
         reading of new input data and writing of compressed output data to
         the same physical device can substantially increase I/O latencies,
         depending on the machine.

 a) kdu_v_compress -i in.vix -o out.mj2 -rate 2
    -- Compress to a Motion JPEG2000 file, with a bit-rate of 2 bits per pixel
       enforced over each individual frame (not including file format wrappers)
       and reports the per-frame CPU processing time.
 b) kdu_v_compress -i in.vix -o out.mj2 -rate 2,1,0.5 -accurate
    -- See the effects of slope prediction on compressor processing time.
 c) kdu_v_compress -i in.vix -o out.mj2 -rate 2 -frame_reps 4
    -- The "-frame_reps" argument causes each frame to be compressed multiple
       times, using exactly the same parameters, except that on all but the
       first time, the compressed data is flushed to a null target that simply
       discards the data.  The output file still contains one compressed frame
       for each source frame, but throughput statistics report the actual
       amount of work done (number of frame compressions performed and
       associated CPU time).  This allows you to estimate the throughput that
       would be achieved if source frames were already available in memory
       and compressed data were passed in memory to another module -- i.e.,
       avoiding any bottlenecks associated with disk I/O.
 d) kdu_v_compress -i in.vix -o out.mj2 -rate 2 -frame_reps 4 -quiet -cpu
    -- As above, but suppresses progress reports and other informative print
       statements (-quiet) except that overall throughput informatin is
       printed at the very end (-cpu).
 e) kdu_v_compress -i 1920x540x30x420.yuv -o stream.jpb -fields normal \
                   -frate 1001,30000 -jpb_data 3,200 -rate 1.5
    -- Generates an elementary broadcast stream for an interlaced YUV
       file (1080i, 4:2:0), specifying CCIR-709 colour and a maximum
       bit-rate compatible with the Level-1 broadcast profile.  For other
       profiles, use the `Sprofile' attribute.  For encoding at close to
       the limiting bit-rate for a profile, you are recommended to also
       specify `Creslengths'.
 f) kdu_v_compress -i in444.vix -o out.mjc Corder=PCRL Clevels=5
                    "Cprecincts={8,8192},{4,8192},{2,8192},{1,8192}"
                    "Cdecomp=B(-:-:-),B(-:-:-),B(-:-:-),H(-),H(-),H(-)"
                    "Cblk={4,1024}" Catk=2 Kkernels:I2=I5X3
                    -rate 2 Scbr=\{1,10\} -cbr_stats
    -- Demonstrates CBR (constant-bit-rate) compression of a video,
       writing results in the MJC file format.  CBR compression is
       requested via the `Scbr' option, which has a number of effects:
       A) In this application, if the output is an MJC file, its header
          is written with the special "CBR flag", which indicates that all
          compressed video frames have exactly the same size, which is
          written as part of the header.  After the header, the MJC file
          is nothing other than a concatenation of the codestreams
          associated with each successive frame, possibly with padding
          bytes inserted between the EOC marker of one codestream and the
          SOC marker of the next, so as to guarantee that codestreams
          are separated by the advertised constant number of bytes.
       B) Within each codestream , content is flushed in such a way as
          to guarantee compatibility with a low latency constant bit-rate
          communication channel.  Codestream flushing proceeds in small
          "flush sets", each of which has its own tight rate control
          machinery.  The total number of bytes produced by any flush
          set is guaranteed not to overflow a bit-buffer, which drains
          at a constant rate (the CBR channel rate).  Underflow is
          also guaranteed not to occur.  In this example, the bit-buffer
          size is equivalent to 10 lines of a video frame and each
          flush-set effectively represents 8 lines of the video frame,
          due to the choice of an especially low latency wavelet
          transform.
    -- The Part-2 ATK and DFS features are used here to define a
       non-Mallat DWT structure based on the irreversible 5/3 LeGall
       wavelet kernel.  The DWT structure consists of 3 regular
       decomposition levels (3 vertical and 3 horizontal), followed
       by 2 horizontal-only deocmposition levels.  It can be shown
       that the overall end-to-end latency associated with this
       transform structure, combined with the 10 lines of communication
       delay associated with the bit-buffer, is exactly 30 lines of
       the video frame.
    -- In this example, the codestream content is actually flushed only
       at the end of each video frame, which is not itself a low latency
       mechanism, but the `Scbr' option forces the flushing to be done
       in a way that is completely equivalent to what a hardware 
       implementation with the 30 line end-to-end latency would do.
       The next example shows you how to actually achieve very low
       latency in software, by adding the "-flush_period" option, but
       of course no software solution can realize latencies of only
       a few video, since operating system scheduling jitter is typically
       on the order of 1ms or even a few milliseconds.
    -- The "-cbr_stats" option provides useful summary statistics for
       the CBR flushing process -- the "-cbr_trace" option can be used
       to provide a much more detailed frame-by-frame report.
    -- The configuration here can be used with 4:4:4 video frame and
       also with 4:2:2 video frames, or indeed any format with
       horizontally sub-sampled chrominance components, so long as
       they are not vertically sub-sampled.  The next example shows
       you how to achieve the same ultra low latency with 4:2:0 content
       where chrominance components are sub-sampled by 2 in both the
       vertical and horizontal directions.
    -- Mote that the output format is not required to be MJC; MJ2 and
       JPX files can also be written, but in this case the written
       codestreams might occasionally be a few bytes shorter than the
       maximum size (the CBR size), and those formats add other metadata
       whose size cannot be accounted for by the CBR flushing algorithm.
    -- You can pass the MJC file produced here to kdu_v_expand, or convert
       it to an MJ2 or JPX file using "kdu_merge" -- see usage statements
       for that application.  However, the ultimate intent in a low latency
       video communication application is that the generated codestream
       content emitted via the abstract `kdu_compressed_target' interface
       internally would be passed directly to a communication channel, with
       the equivalent of "kdu_v_expand" or "kdu_vex_fast" reading from the
       other end of the channel.
 g) kdu_v_compress -i in420.vix -o out.mjc Corder=PCRL Clevels=5
                    "Cprecincts={8,8192},{4,8192},{2,8192},{1,8192}"
                    "Cprecincts:C1={4,8192},{2,8192},{1,8192}"
                    "Cprecincts:C2={4,8192},{2,8192},{1,8192}"
                    "Cdecomp=B(-:-:-),B(-:-:-),B(-:-:-),H(-),H(-),H(-)"
                    "Cdecomp:C1=B(-:-:-),B(-:-:-),H(-),H(-),H(-)"
                    "Cdecomp:C2=B(-:-:-),B(-:-:-),H(-),H(-),H(-)"
                    "Cblk={4,1024}" Catk=2 Kkernels:I2=I5X3
                    -rate 4 Scbr=\{1,10\} Qstep=0.001 -flush_period 64
    -- Essentially the same as the previous example, except for the
       following:
       A) "-flush_period" is used to force the codestream flushing
          algorithm to run in the background with a period of 64 video
          lines.  Each time it runs, the available content is partitioned
          into flush-sets which have a period of 8 video lines, and the
          CBR constraints are applied while preparing and pushing out
          content for each flush set.  The "-flush_period" could be set
          as small as 8, but this would incur a larger overhead in
          the launching and management of the background flushing jobs,
          which is unwarranted considering the granularity with which
          operating systems can be expected to schedule threads.
       B) The wavelet transform structure configured here is suitable for
          CBR flushing of 4:2:0 content, or any video content in which
          the chrominance components are vertically sub-sampled by 2
          relative to the luminance component.  Specifically, the
          chrominance components are configured to use a wavelet
          transform with only 2 vertical decomposition levels, while the
          luminance component uses 3 vertical decomposition levels, so
          that all components incur essentially the delay.
    -- Note that the use of "-flush_period" here typically increases
       throughput, despite the overhead of frequently re-entrant codestream
       flushing jobs.  This is because the low latency properties of the
       transform, code-block dimensions and flushing operations allows all
       working memory to reside on-chip, almost entirely within L2 and L1
       cache memories.
 h) kdu_v_compress -i s0035.tiff+399 -o - -in_prec 10M -frate 1,24
                   -rate 2 > out.mjc
    -- Compresses up to 400 TIFF files, "seq00035.tiff" through to
       "seq00434.tiff".
    -- The "-in_prec" argument allows you to specify how many of the
       sample bits recovered for each channel are valid, and whether they
       are in the MSB or LSB bit positions of each TIFF sample word.  This
       is useful, because it is common to pack 10 or 12-bit/channel imagery
       into 16-bit/channel TIFF files -- an unfortunate practice that has
       arisen due to the fact that TIFF readers rarely support the full
       range of precisions that can actually be declared in TIFF tags.
    -- The resulting raw codestreams are written to "stdout" following the
       MJC file format, which consists of a 12-byte header, followed by
       a concatenated sequence of codestreams, each prepended by a 4-byte
       bigendian length field.
    -- This form of command can be used to pipe a sequence of compressed
       codestreams to other applications that might package the codestreams
       into custom containers.

kdu_v_compress advanced Part-15 (HTJ2K) Features
------------------------------------------------
    These additional examples relate to Part-15 of the JPEG 2000 standard,
    also known as HTJ2K (High Throughput JPEG 2000), or simply JPH.  The
    examples here will grow considerably in the coming months, but first
    we just provide some very simple examples to get you going.

Ha) kdu_v_compress -i vid4K.vix -o out.mj2 Qstep=0.01 Cmodes=HT
    -- This is a really simple example, without any rate control, which
       allows the highest possible HTJ2K encoding throughput.  The compressed
       quality here is imply controlled by the quantization step sizes,
       which are most eastily specified via the single parameter `Qstep',
       which expands out into separate step sizes for every subband that
       are adapted to minimize overall mean squared error.
    -- In a real application, of course, you can change the Qstep value
       from frame to frame -- J2K supports finer grain (precinct-level)
       quantization step control, but the feature is hardly every used and
       not currently implemented in Kakadu.
    -- Be aware that the encoding itself here is too fast to observe.
       The throughput in this example will be limited by disk reading
       (and even writing) speed.  To get a better idea how fast things
       actually are, you can use the "-frame_reps" argument to force the
       encoder to process frames that are read from disk many times.
       In practice, of course, you would be thinking of using the
       methods in the demo-app to compress directly from frames in
       memory rather than from disk.

Hb) kdu_v_compress -i vid4K.vix -o out.mj2 Creversible=yes Cmodes=HT
    -- Same as above, but does lossless compression.
    -- Be aware that even with the "-frame_reps" argument used to avoid
       bottlenecking on disk reading, you will still need a fast SSD to
       absorb the output compressed data fast enough.  For example, on
       a 4 core, i7 Skylake CPU, Kakadu can encode 4K 4:4:4 12 bit/channel
       content losslessly at more than 65 frames per second, typically
       producing well over a Gigabyte/second just at the output, let alone
       the rate at which data must be read.

Hc) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=RPCL Clevels=6 Cblk={32,128}
                   -rate 2 Qfix16=FREE Cmodes=HT -fastest -double_buffering 16
                   -proc_limit 6,1,100 Kkernels:I2=I5X3 Catk=2 -frame_reps 32
    -- This example illustrates one way to do fully rate-controlled video
       encoding with deterministic bounds on encoding complexity (6 HT passes
       per code-block in this case).
    -- We use a light weight (5/3 irreversible) DWT (a Part-2 feature) with
       slightly rectangular code-blocks (a little faster and lower in memory
       consumption than the default), with visual optimization (turn off
       with -no_weights for MSE optimization).
    -- The "-proc_limit" option is important here, since it limits the
       number of HT sets to 2 per frame (each HT Set consists of an HT
       Cleanup and HT SigProp and an HT MagRef pass), using statistics
       from previous frames to decide where to put them.
    -- The "-frame_reps" option is only for speed testing, since reading
       frames from file (using "fread") is too slow and becomes the
       bottleneck in the video compression application.  The number of
       processed coding passes does not depend upon this, so speed
       estimates should reflect what can be achieved with an in-memory
       video source (e.g., coming directly from a camera).
    -- As an example on a late 2016 15" Macbook Pro (Skylake 4-core i7 CPU
       with 2.7GHz base clock), this example is able to encode full 4K RGB
       4:4:4 content with 12 bits/channel at 75 frames/second.
    -- While this option works well, it has a couple of non-idealities,
       that are all corrected with the "Cplex" mechanism illustrated
       below.  These non-idealities are:
       a) The first frame is compressed with essentially no constraint on
          encoding complexity, to avoid a cold start that may have lower
          quality;
       b) If there are large changes in the scene complexity from frame to
          frame, the "-proc_limit" approach can significantly misjudge the
          best set of coding passes to perform, resulting in reduced
          image quality, e.g., in the first frame or two after a scene change.
       
Hd) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=RPCL Clevels=6 Cblk={32,128}
                   -rate 2 Qfix16=FREE Cmodes=HT -fastest -double_buffering 16
                   Cplex={6,EST,0.25,0} Kkernels:I2=I5X3 Catk=2 -frame_reps 32
    -- This example replaces the "-proc" method of complexity control with
       the "Cplex" method.  The "-proc" method uses the rate control outcome
       from previous frames to determine good sets of coding passes to
       generate in a current frame (we are generating only 6 HT coding passes
       for each code-block, but can choose which bit-plane to start from).
       The "Cplex" method, however, processes each frame completely
       independently, relying upon statistics collected from the subband
       samples as they are generated.
    -- You should find that this example can also run a little faster than
       the one above that uses "-proc".  Both should produce very nearly
       the same image quality, but the "-proc" method will suffer if
       the local image statistics vary strongly from frame to frame.
    -- As an example on a late 2016 15" Macbook Pro (Skylake 4-core i7 CPU
       with 2.7GHz base clock), this example is able to encode full 4K RGB
       4:4:4 content with 12 bits/channel at 78 frames/second.
    -- For reference, with Cmodes=0 (regular JPEG 2000 encoding) on the same
       platform, the throughput with regular Kakadu (not the speed-pack),
       is 8.3 frames/second.  This means that HTJ2K provides an end-to-end
       throughput improvement of almost 9.4x at 2 bits/pixel with tight
       rate control.  Of course, at higher bit-rates, or using quantization
       based rate control, the throughput increase can be much larger again.
    -- The Cplex method, by itself, can suffer a little if image statistics
       vary very strongly between the top and bottom of the frame, but the
       impact is usually only small.  However, this slight weakness can be
       avoided in three ways:
       a) You can introduce delay into the "Cplex" method, as documented in
          the "Hxx" series of examples for "kdu_compress".  In particular,
          you can use "Cplex={6,EST,0.25,-1}" to introduce the maximum
          possible delay, but it will consume a lot more memory and so
          there will be greater demand on the processor's external memory bus.
       b) The "Cplex" method can use statistics from previous frames as a
          rough guide to avoid making unreasonable forecasts about future
          scene complexity when it has only seen a small amount of content
          from the top of a current video frame.  This works extremely well,
          even under conditions of strong inter-frame and spatial complexity
          variation.  So well, in fact, that the "kdu_v_compress" and
          "kdu_vcom_fast" demo applications always use this feature when
          the "Cplex-EST" method is specified, directly or indirectly.  In
          this case, only the first frame of the video sequence suffers from
          the small possible quality degradations mentioned above, that
          can arise from large statistical variations from the top to the
          bottom of the image.
       c) The "Cplex" method can use background statistics that provide a
          stationary model (not dependent on the data being compressed)
          that serves to make forecasts more robust.  This feature is
          activated via the "-bstats" argument.  You can use methods (b)
          and (c) together, so that the background statistics make the
          "Cplex-EST" algorithm more robust for the first video frame,
          while inter-frame statistics improve the robustness for all
          other frames -- it does not matter much how far apart the
          frames are.  This is the approach that this demo-app employs
          automatically if "-bstats" is specified, as shown in the next
          example.
He) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=RPCL Clevels=6 Cblk={32,128}
                   -rate 2 Qstep=0.001 Qfix16=FREE Cmodes=HT -fastest
                   -double_buffering 16 -bstats bstats.txt
                   Kkernels:I2=I5X3 Catk=2 -frame_reps 32
    -- Just like the last example, and just as fast, but with the "-bstats"
       argument, which implicitly installs the recommended minimum memory
       "Cplex-EST" strategy "Cplex={6,EST,0.25,0}", although you can also
       specify the `Cplex' attribute yourself explicitly, to control the
       level of complexity and other attributes.
    -- The "-bstats" argument here behaves exactly the same way as it does
       when used with "kdu_compress" or "kdu_buffered_compress", and indeed
       you can collect the statistics from images processed with those
       demo-apps, or you can collect the statistics from videos with this
       demo-app or "kdu_vcom_fast" -- they are interchangeable.  The
       main difference between this example, and the previous one, is that
       the "-bstats" option will avoid any small degradations in image
       quality for the first frame of the sequence, that might result
       from imagery with strong spatial variations in scene complexity.
       This is mostly of interest if you are using this demo-app to
       compress source videos that contain only one frame.
    -- Here we also specify "Qstep=0.001" to provide a finer set of
       quantization step sizes, capable of generating extremely high
       quality compressed content.  It is worth noting that the "-bstats"
       statistics depend on the quantization parameters, to it is a good
       idea to collect statistics with the finer set of quantization step
       sizes.
    -- You can collect statistics using this demo-app, or any of the
       primary Kakadu compression demo-apps.  To do it with this demo-app,
       for the coding parameters employed here, use something like the
       following, noting that the "-rate" value itself has no impact on
       the collected statistics -- you just need to specify some "-rate".
          kdu_v_compress -i vid1.vix Corder=RPCL Clevels=6 Cblk={32,128}
                         -rate 2 Qstep=0.001 Qfix16=FREE Cmodes=HT -fastest
                         Kkernels:I2=I5X3 Catk=2 -bstats -,bstats.txt
          kdu_v_compress -i vid2.vix Corder=RPCL Clevels=6 Cblk={32,128}
                         -rate 2 Qstep=0.001 Qfix16=FREE Cmodes=HT -fastest
                         Kkernels:I2=I5X3 Catk=2 -bstats bstats.txt,bstats.txt
          kdu_v_compress -i vid3.vix Corder=RPCL Clevels=6 Cblk={32,128}
                         -rate 2 Qstep=0.001 Qfix16=FREE Cmodes=HT -fastest
                         Kkernels:I2=I5X3 Catk=2 -bstats bstats.txt,bstats.txt
          ...
       In practice, the compression is only very weakly dependent on the
       statistics, so you don't need much training (a single video with
       some representative content should be more than enough).
Hf) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=PCRL -rate 2 Clevels=5
                   Cprecincts={8,8192},{4,8192},{2,8192} Cblk={4,1024}
                   Cdecomp=B(-:-:-),B(-:-:-),H(-),H(-),H(-),H(-)
                   Qstep=0.0001  Catk=2 Kkernels:I2=I5X3 Scbr={1,10} Cmodes=HT
                   Cplex={6,EST,0.25,-1}
    -- Demonstrates low-latency compression with a constant-bit-rate channel
       model, with 2 vertical and 5 vertical levels of wavelet transform
       using irreversible 5x3 subband filters and flush-sets of 8 lines each.
    -- The precincts force code-blocks in the second vertical transform level
       to have size 1024x2, while code-blocks in the first level have size
       4x1024, so that a new set of code-blocks from every subband appears
       every 8 image lines.
    -- The leaky-bucket buffer model holds 10 lines worth of compressed data
       at the constant bit-rate corresponding to 2 bits/pixel (-rate 2); it
       is filled once per flush set, to a fulness of between 8 and 10 lines
       by the rate control algorithm, and drains at a constant rate, so that
       neither underflow nor overflow every happens.
    -- The Cplex-EST algorithm is used to constraint complexity to at most
       two HT-Sets per code-block (6 coding passes).  A high memory form of
       the algorithm is used (last Cplex parameter is -1) to avoid any need
       for statistical forecasting within the Cplex-EST procedure -- it means
       that the code-blocks for each flush-set are not scheduled for encoding
       until all subband samples for the flush-set have been generated by the
       wavelet transform and analyzed to determine the most appropriate
       bit-planes to use for the two generated HT-Sets in each code-block.
    -- The fundamental end-to-end latency for this configuration, including
       channel transport and decoding, from the time one line enters the
       encoder to the time the line appears at the output of the decopressor
       is 24 image lines.  This does not allow for additional time taken in
       computation.  As explained with the `kdu_codestream::get_cbr_model'
       function, a practical hardware implementation should allow for an
       additional 8 lines of latency to perform the block encoding and an
       additional 4 lines of latency to complete all block decoding
       operations, assuming that all code-block processors operate at a
       constant throughput and are fully occupied, and that nothing can be
       assumed about the distribution of compressed bits between low and
       high frequency subband samples.  Lower latencies are possible with
       very careful design.
    -- It is worth noting that the low-latency CBR flushing model accounts
       for all codestream headers and allows for a carefully calculated
       transmission start point -- the delay between arrival of the first
       image line and the start of transmission for that frame.  This start
       delay is the same for all codestreams (all frames), but the very
       last flush-set of each codestream may be smaller than its maximum
       size.  When this happens, dead time exists between the end of one
       codestream and the start of the next codestreams, so the cumulative
       length of all codestreams will be slightly smaller than expected when
       they are written to the MJ2 file format, as is done here.  You can
       instead write them to the MJC file format, which in CBR mode consists
       of a concatenated sequence of fixed-length codestreams, including
       whatever padding is required after the EOC (End of Codestream) marker
       to ensure an entirely constant data rate.
    -- We point out here that although everything here can be done with
       either the original JPEG 2000 block coding algorithm or the HT
       block coding algorithm, HTJ2K codestreams are by far the most
       suitable for low latency compression.  The reason for this is that
       the HT block encoder and decoder are very fast and can run with
       deterministically bounded throughput, which is extremely important for
       low latency applications where the number of code-blocks available for
       parallel processing is inherently limited.
    -- Finally we point out that the example here actually flushes all the
       coded content at the end of the codestream, rather than incrementally,
       although the flushing model processes flush-sets one by one,
       sequentially without any look ahead, so it is entirely equivalent to
       flushing the content immediately after the code-blocks of each
       flush-set have been generated.  For a true low latency software
       deployment, you should add the "-flush_period" option.  For example,
       you can specify "-flush_period 8", but this does not guarantee that
       flushing happens immediately after each flush-set of code-blocks has
       been generated.  In Kakadu's multi-threaded model, flushing occurs in
       a high priority background job, but thread scheduling delays inevitably
       mean that multiple flush-sets might be ready by the time the job
       gets around to processing them, so that they may appear in a somewhat
       bursty nature at the `kdu_compressed_target' interface that collects
       compressed data.  This sort of thing is the main difference between
       hardware and software deployments, at least for non-real-time
       operating systems.
Hg) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=PCRL -rate 2 Clevels=5
                  Cprecincts={8,8192},{4,8192},{2,8192} Cblk={4,1024}
                  Cdecomp=B(-:-:-),B(-:-:-),H(-)
                  Qstep=0.0001 Scbr={1,10} Cmodes=HT
                  Cplex={6,EST,0.25,-1}
    -- This is exactly the same as the previous example (Hf), except that
       we use the irreversible 9x7 wavelet transform (the default DWT from
       JPEG 2000 Part-1), which has higher coding efficiency than the 5x3
       transform on natural photographic content, but a higher latency.
    -- The fundamental end-to-end latency here is 36 lines, and the
       additional latency attributed to computation is still 12 lines for
       a straightforward implementation.

Hi) kdu_v_compress -i vid4K.vix -o out.mj2 Corder=PCRL -rate 2 Clevels=5
                  Cprecincts={16,8192},{8,8192},{4,8192},{2,8192} Cblk={8,512}
                  Cdecomp=B(-:-:-),B(-:-:-),B(-:-:-),H(-),H(-),H(-)
                  Qstep=0.0001 Scbr={1,18} Cmodes=HT
                  Cplex={6,EST,0.25,0} -decoupled_stats
    -- This is similar to the above example (Hg) except that it uses 3 vertical
       levels of wavelet transform, for improved coding efficiency while
       roughly doubling the latency.  Flush-sets here have 16 lines instead
       of 8 lines, and the leaky-buffer model is sized at 18 lines, so that
       each flush-set must fill the buffer to between 16 and 18 lines to
       avoid overflow or underflow.
    -- Another feature of this example is the use of the low-memory form
       of the Cplex-EST complexity constraint algorithm (last parameter of
       `Cplex' is 0 rather than -1).  This does not save a huge amount of
       memory since Cplex-EST runs on a flush-set basis and the flush-sets
       are small, but it means that block encoding in each subband can
       commence as soon as the relevant code-block's samples have been
       produced by the wavelet transform, rather than waiting until all
       subband samples for the entire flush-set have been produced.  This
       does not alter the fundamental latency, but it reduces the
       computation-induced latency, since computation for the higher resolution
       code-blocks (the bigger ones) can occur within the fundamental
       wavelet analysis delay window.  In practice, we find that the low
       memory form of Cplex-EST performs almost identically to the high
       memory form, with a typical drop of about 0.02 dB in PSNR (i.e.,
       negligible loss of image quality), even on content with highly
       non-uniform scene complexity distribution over the frame.  The
       "-decoupled_stats" option prevents the use of any information from
       previous frames in the video when forecasting the complexity of
       subband samples from a flush-set that have not yet been produced;
       however, you can always add statistical sharing by removing this
       argument or you can introduce a background statistical model with the
       "-bstats" option.  These are unlikely to make much difference unless
       you work with many more levels of wavelet transform and much larger
       flush-sets.
    -- The fundamental latency for this configuration is 76 lines end-to-end,
       while the additional latency due to computation is less than 20 lines
       in a simple hardware implementation.  In practice, use of the
       low memory Cplex-EST configuration here allows for even smaller
       computation induced delays.

Hj) kdu_v_compress -i vid_3840x2160_60_10b_420_000.yuv -o out.mj2 -rate 2
                  Corder=PCRL Clevels=4 Clevels:C0=5
                  Cprecincts={16,8192},{8,8192},{4,8192} Cblk={16,256}
                  Cprecincts:C0={32,8192},{16,8192},{8,8192},{4,8192}
                  Cdecomp=B(-:-:-),B(-:-:-),H(-)
                  Cdecomp:C0=B(-:-:-),B(-:-:-),B(-:-:-),H(-)
                  Qstep=0.0001 -no_weights
                  Scbr={1,34} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 32
    -- Same as "kdu_compress" example (Hm), demonstrating low-latency HTJ2K
       compression of 4:2:0 content with deterministically managed encoding
       complexity via the Cplex-EST algorithm.
    -- Fundamental end-to-end latency here is 108 luminance lines.
Hn) kdu_v_compress -i vid_3840x2160_60_10b_420_000.yuv -o out.mj2 -rate 2
                   Corder=PCRL Clevels=5
                   Cprecincts={32,8192},{16,8192},{8,8192},{4,8192} Cblk={16,256}
                   Cdecomp=-(),B(-:-:-),B(-:-:-),H(-)
                   Cdecomp:C0=B(-:-:-),B(-:-:-),B(-:-:-),H(-)
                   Qstep=0.0001 -no_weights
                   Scbr={1,34} Cmodes=HT Cplex={6,EST,0.25,-1} -flush_period 32
    -- Same as "kdu_compress" example (Hn), with the same application and
       latency as example (Hj) above, except that there are 3 natural
       resolution scales, accessible (for example) via the "-reduce" argument
       to "kdu_expand", rather than just 2 natural resolution scales; this
       is achieved by using an empty first level of decomposition for the
       chrominance components, so that discarding resolution levels leaves
       the original 4:2:0 content with a 4:4:4 sampling arrangement.

kdu_vcom_fast
-------------
    This is the last of the compression demo apps.  It provides largely the
    same set of options as "kdu_v_compress" and its purpose is also to compress
    a sequence of video frames.  The main difference from "kdu_v_compress"
    is that this application can instantiate multiple independent frame
    processing engines, each of which can be heavily multi-threaded.  The
    work flow within each frame processing engine is very similar indeed to
    "kdu_v_compress", except that frames are read by a separate high priority
    thread and transferred to the processing engines, while kdu_v_compress
    schedules a background processing job within its single thread-group to
    do the file reading.

    With this application you can explore the impact of different numbers
    of frame processing engines and different numbers of threads per engine
    on throughput and delay.  The delay for a video compression application
    is essentially equal to the number of independent frame processing
    engines.  On machines with a very large number of CPUs, it is usually
    necessary to instantiate a small number of engines (e.g., 2 or 4) to
    obtain close to maximum throughput.  This demo app also allows you to
    bind the threads used for individual processing engines to specific
    groups of logical CPUs (affinity control).

    This application accepts a much smaller range of input formats, just
    to avoid duplicating too much code from kdu_v_compress.  If necessary,
    convert your YUV files to VIX by using kdu_v_compress with the
    "Creversible=yes" option, followed by kdu_v_expand.  Then you can
    try out kdu_vcom_fast on the VIX files.

 a) kdu_vcom_fast -i in.vix -o out.mj2 -rate 2 -stats
    -- Compress to a Motion JPEG2000 file, with a bit-rate of 2 bits per pixel
       enforced over each individual frame (not including file format wrappers)
       and reports the per-frame CPU processing time and compression stats.
 b) kdu_vcom_fast -i in.vix -o out.jpx -jpx_prefix cover.jpx -rate 2 -stats
    -- As above, but writes a JPX animation, whose first frame is the "cover
       image" supplied via "cover.jpx".
    -- Note that the cover image must have a composition box; it can be
       generated using the "kdu_merge" tool using something like
       "kdu_merge -i input.jp2 -o cover.jpx -composit 1@60*0".
    -- One reason for writing a JPX animation rather than an MJ2 file is that
       JPX files can carry codestreams that use any of the Part-2 features,
       such as advanced multi-component transforms (e.g., hyperspectral
       video compressed using an inter-component KLT or DWT), the ultra-fast
       block coding modes accessed via `Cmodes' options like `BYPASS_E2'.
 c) kdu_vcom_fast -i 4K.vix -o 4K_24.mj2 ORGgen_tlm=3 \
    Corder=CPRL Cblk={32,32} Clevels=6 \
    Cprecincts={256,256},{256,256},{256,256},{256,256},{256,256},{256,256},{128,128} \
    Sprofile=CINEMA4K Creslengths=1302083 Creslengths:C0=1302083,1041666 \
    Creslengths:C1=1302083,1041666 Creslengths:C2=1302083,1041666 -rate 1.48 \
    -stats -frames 500 -loop
    -- This one generates codestreams compatible with the 4K digital cinema
       standard, assuming the input VIX file contains 12-bit/sample XYZ
       sample values that do not exceed the 4K cinema standard's maximum
       dimensions.
    -- Additionally, the source file is read over and over again in a "-loop"
       as required to ensure that 500 frames are compressed.
 d) kdu_vcom_fast -i in.vix -o out.mj2 -rate 2 -frame_reps 4
    -- Repeatedly compress each frame 4 times, discarding all but one of the
       four codestreams produced by each codestream.  This provides a useful
       means of testing the maximum throughput of the compression process
       in cases where you might otherwise be limited by the rate at which the
       disk can read from the input VIX file.
 e) kdu_vcom_fast -i in.vix -o out.mj2 -rate 2 -engine_threads 8 8
    -- Overrides the default assignment of threads to processing engines to
       ask for two engines, each with a thread-pool of 8 threads to do its
       processing (16 threads in all).  This would be appropriate for a
       platform with 16 logical CPUs.
 f) kdu_vcom_fast -i in.vix -o out.mj2 -rate 2 \
    -engine_threads 8:(0,1,2,3,4,5,6,7) 8:(8,9,10,11,12,13,14,15)
    -- Similar to the above example, but instantiates 2 frame processing
       engines, each with 8 threads, binding the first engine to be
       scheduled on logical CPUs 0 to 7 and the second engine to be scheduled
       on logical CPUs 8 to 15.
    -- Binding threads to logical CPUs is primarily of interest when the
       CPUs reside in multiple packages/dies.  This allows you to ensure that
       each engine runs entirely within a single package, which leads to
       fewer inter-package memory transactions.
 g) kdu_vcom_fast -i in.vix -o out.mj2 -rate 2 \
    -engine_threads 8:0(0,1,2,3,4,5,6,7) 8:1(0,1,2,3,4,5,6,7)
    -- Similar to the above but this one is suitable for a Windows platform
       whose logical CPUs have been organized into two separate processor
       groups (0 and 1) by the system administrator.  Windows will not
       normally allow a task/process to run in more than one processor group;
       however, the CPU affinity notation here requests the first engine's
       threads to be assigned to logical CPUs 0 to 7 of group 0 and the
       second to be assigned to logical CPUs 0 to 7 of group 1.

kdu_vcom_fast advanced Part-15 (HTJ2K) Features
-----------------------------------------------
    These additional examples relate to Part-15 of the JPEG 2000 standard,
    also known as HTJ2K (High Throughput JPEG 2000), or simply JPH.

Ha) kdu_vcom_fast -i vid4K.vix -o out.mj2 Corder=RPCL Clevels=6 Cblk={32,128}
                  -rate 2 Qfix16=FREE Cmodes=HT -fastest -double_buffering 16
                  Cplex={6,EST,0.25,0} Kkernels:I2=I5X3 Catk=2
    -- Essentiall the same as example (Hd) for kdu_v_compress.
    -- The only difference here is that without additional arguments, on a
       typical 4-core/8-thread CPU, this example will create two processing
       engines, so the "Cplex" option may result in slightly lower quality
       than full HT encoding (with "Cplex") for two initial frames of the
       video, rather than just one in the "kdu_v_compress" case.
Hb) kdu_vcom_fast -i vid4K.vix -o out.mj2 Corder=RPCL Clevels=6 Cblk={32,128}
                   -rate 2 Qstep=0.001 Qfix16=FREE Cmodes=HT -fastest
                   -double_buffering 16 -bstats bstats.txt
                   Kkernels:I2=I5X3 Catk=2
    -- Essentially the same as example (He).  The main difference from example
       (Ha) above is that the background statistics provided via the "-bstats"
       argument prevent any quality degradation due to strong variations in
       scene complexity within the first two frames of the video sequence
       (if there are two processing engines), rather than just one, in the
       case of "kdu_v_compress".  So the "-bstats" option is slightly more
       valable for "kdu_vcom_fast" than it is for "kdu_v_compress".

kdu_merge
---------
 a) kdu_merge -i im1.jp2,im2.jp2 -o merge.jpx
    -- probably the simplest example of this useful tool.  Creates a
       single JPX file with two compositing layers, corresponding to the
       two input images.  Try opening `merge.jpx' in "kdu_show" and using
       the "enter" and "backspace" keys to step through the compositing
       layers
 b) kdu_merge -i video.mj2 -o video.jpx
    -- Assigns each codestream of the input MJ2 file to a separate compositing
       layer in the output JPX file.  Try stepping through the video frames
       in "kdu_show".
 c) kdu_merge -i video.mj2 -o video.jpx -composit 300@24.0*0+1
    -- Same as above, but adds a composition box, containing instructions to
       play through the first 300 images (or as many as there are) at a
       rate of 24 frames per second.
    -- The expression, "0+1" means that the first frame correspondings to
       compositing layer 0 (the first one) and that each successive frame
       is obtained by incrementing the compositing layer index by 1.
 d) kdu_merge -i background.jp2,video.mj2 -o out.jpx
              -composit 0@0*0 150@24*1+2@(0.5,0.5,1.71),2+2@(2.3,3.2,1)
    -- Demonstrates a persistent background (0 for the iteration count makes
       it persistent), on top of which we write 150 frames (to be played at
       24 frames per second), each consisting of 2 compositing layers,
       overlayed at different positions and scales.  The first frame
       overlays compositing layers 1 and 2 (0 is the background), after
       which each new frame is obtained by adding 2 to the compositing
       layer indices used in the previous frames.  The odd-indexed
       compositing layers are scaled by 1.71 and positioned half their scaled
       with to the right and half their scaled height below the origin
       of the compositing canvas.  The others are scaled by 1 and positioned
       2.3 times their width to the right and 3.2 times their height below
       the origin.
    -- The kdu_merge utility also supports cropping of layers prior to
       composition and scaling.
 e) kdu_merge -i im1.jp2,im2,jp2,alpha.jp2 -o out.jpx
              -jpx_layers 2:0 sRGB,alpha,1:0/0,1:0/1,1:0/2,3:0/3
                              sRGB,alpha,1:0/0,1:0/1,1:0/2,3:0/0
              -composit 0@(0,0,2),1@(0.5,0.5,1),2:(0.3,0.3,0.4,0.4)@(1.2,1.2,1)
    -- This demonstrates the creation of a single complex image from 3
       original images.  im1.jp2 and im2.jp2 contain the colour imagery,
       while alpha.jp2 is an image with 4 components, which we selectively
       associate with the other images as alpha blending channels.
       * Three custom compositing layers are created using the `-jpx_layers'
         command.  The first just consists of the first compositing layer
         from the second image file (note that file numbers all start from 1
         while everything else starts fro 0) -- of course, JP2 files have
         only one compositing layer.  The second custom compositing layer
         has four channels (3 sRGB channels and 1 alpha channel), extracted
         from image components 0-2 of codestream 0 in file 1 and image
         component 3 (the 4'th one) of codestream 0 in file 3 (the alpha
         image).  The relevant codestream colour transforms are applied
         automatically during the rendering process, so that even though the
         components have been compressed using the codestream ICT, they may
         be treated as RGB components.  The third compositing layer is
         similar to the second, but it uses the second component of
         the alpha image for its alpha blending.
       * One composited image is created by combining the 3 layers.  The
         first layer is scaled by 2 and placed at the origin of the
         composition canvas.  The second layer is placed over this, scaled
         by 1 and shifted by half its height and width, below and to the
         right of the composition canvas.  The third layer is placed on top
         after first cropping it (removing 30% of its width and height from
         the left, and preserving 40% of its original with and height) and
         then shifted it by 1.2 times its cropped height and width.
    -- It is worth noting that the final image does not contain multiple
       copies of any of the original imagery; each original image codestream
       is copied once into the merged image and then referenced from
       custom compositing layer header boxes, which are in turn referenced
       from the composition box.  This avoids inefficiencies in the file
       representation and also avoids computational inefficiencies during
       rendering.  Each codestream is opened only once within "kdu_show"
       (actually inside `kdu_region_compositor') but may be used by
       multiple rendering contexts.  One interesting side effect of this is
       that if you attach a metadata label to one of the codestreams in
       the merged file it will appear in all elements of the composited
       result which use that codestream.  You can attach such metadata
       labels using the metadata editing facilities of "kdu_show".
 f) kdu_merge -i im1.jpx,im2.jpx,im3.jpx -o album.jpx -album2
    -- Make a "photo album" containing the supplied input images (keeps all
       their individual metadata, correctly cross-referenced to the images
       from which it came) and generates new template metadata entries that
       can easily be edited from kdu_winshow or kdu_macshow to build
       descriptions of the content.
 g) kdu_merge -i im1.jpx,im2.jpx,im3.jpx -o album.jpx -album2 10 -links
    -- As in (f), but the period between frames (during animated playback)
       is set to 10 seconds, and individual photos are not copied into the
       album.  Instead they are simply referenced by fragment table boxes
       (ftbl) in the merged JPX file.  This allows you to present imagery in
       lots of different ways without actually copying it into each
       presentation.  Linked codestreams are properly supported by all Kakadu
       objects and demo apps, including client-server communications using
       "kdu_server".
 h) kdu_merge -i im1.jp2,im2.jp2,im3.jp2 -o video.mj2 -mj2_tracks P:0-2@30
    -- Merges three still images into a single Motion JPEG2000 video track,
       with a nominal play-back frame rate of 30 frames/second.
 i) kdu_merge -i im1.jpx,im2.jpx,... -o video.mj2 -mj2_tracks P:0-@30,1-1@0.5
    -- As above, but merges the compositing layers from all of the input
       files, with a final frame (having 2 seconds duration -- 0.5 frames/s)
       repeating the second actual compositing layer in the input
       collection.
 j) kdu_merge -i vid1.mj2:1,vid1.mj2:0,vid2.mj2 -o out.mj2
    -- Merges the second video track encountered in "vid1.mj2" with
       the first video track encountered in "vid1.mj2" and the first
       video track encountered in "vid2.mj2".  In this case, there is no
       need to explicitly include a -mj2_tracks argument, since timing
       information can be taken from the input video sources.  The
       tracks must be all either progressive or interlaced.
 k) kdu_merge -i im1.j2c,im2,j2c,im3.j2c -o out.jpx -raw_proto proto.jp2
    -- Merges three raw codestream files (not embedded inside any JP2
       file wrapper) into a single JPX file which will have three
       compositing layers (one for each source codestream), taking the
       rendering information (colour description, channel assignment,
       rendering resolution and potentially a colour palette) from the
       `proto.jp2' file, which acts as a prototype.  You can use any JP2 or
       JPX file as a prototype, allowing you to add potentially very
       complex rendering descriptions to the raw codestreams.  You can merge
       any number of raw codestreams in one go, since the app only
       opens raw codestreams one at a time.  All other input sources are
       kept open from the time the command line is read until the output file
       has been generated, limiting the number of input files to around 500
       on some operating systems, but this limit does not apply to raw
       codestreams.  You can use similar commands to create JPX files which
       link (rather than embed) raw codestream files or to create MJ2 files
       from raw codestreams.
 l) kdu_merge -i in1.jpx,in2.jpx -o out.jpx -jpx_meta_swap 2 -jpx_layers 1:0
    -- Creates a JPX output file containing a single compositing layer, whose
       codestream and rendering information are taken from the first
       compositing layer of `in.jpx', but whose auxiliary metadata (everything
       managed by `jpx_meta_manager', such as labels, ROI descriptions, xml,
       etc.) are imported from `in2.jpx'.  This provides a useful way of
       modifying the imagery associated with an existing file, without
       changing its metadata, or vice-versa.  The `jpx_meta_swap' feature is
       more powerful than you might guess from this simple example.  You can
       use it to merge the auxiliary metadata from any set of files with the
       imagery from any (potentially different) set of files, or to clear
       metadata; all codestream and compositing layers in the swapped
       metadata will be fixed up to point to the correct entities in the
       generated output file.
 m) kdu_merge -i im01.j2c+29 -o out.mj2 -raw_proto p.jp2 -mj2_tracks P:0-@30
    -- Merges 30 raw codestream files, with names im01.j2c through im30.j2c,
       into a single MJ2 file, with one video track and a playback rate of
       30 frames/second, taking the colour specifications from "p.jp2".
       This example demonstrates the [+<extend>] suffix which may be appended
       to any file name supplied with the "-i" argument to expand it into
       multiple filenames, all of which differ only through successive
       incrementing of the numerical suffix found within the supplied filename.
 n) kdu_merge -i video.mj2 -o out.jpx -containers 1-1*299 -composit 1@30*0
              -jpx_track 1:1 299@30*0+1
    -- Merges the frames from an MJ2 file into a JPX file that uses containers
       to efficiently describe the large number of identical compositing
       layers that arise.  JPX containers describe a repeated sequence of
       compositing layers, optionally with associated codestream headers;
       moreover containers can embed one or more presentation tracks to
       apply to the relevant compositing layers (without containers, JPX
       files have at most one presentation track, defined by the composition
       box).  A file with containers, must contain at least one top-level
       compositing layer and one top-level codestream, plus one top-level
       composition box, so the container cannot embed compositing layer 0
       here and we must provide a "-composit" argument for the top-level
       layer.
    -- This example uses one container only, which has one "base compositing
       layer" (1-1 means layer 1 to layer 1) that is repeated 299 times
       (there are 300 frames in this example video).  The container is
       automatically equipped with the relevant codestream headers to be
       repeated as well.
    -- In this example, only one presentation track is defined for the
       container via `-jpx_track'.  It is possible to define multiple
       containers, with or without presentation tracks, and it is possible
       to define multiple presentation tracks for any given container.  In
       this case the "1:1: in "-jpx_track" means a track is being defined
       for the first container and it applies to 1 base layer within the
       container, along with all its repretitions.
    -- Note that "kdu_merge" automatically adds simple labels to the
       generated  metadata.  The main purpose of this is to help you to
       see how to annotate presentation tracks through container-embedded
       metadata.  You can view, edit/delete and resave this information using
       the "kdu_show" demo app ("kdu_macshow" or "kdu_winshow").
 o) kdu_merge -i video.mj2 -o out.jpx -containers 1-1*0 -composit 1@30*0
              -jpx_track 1:1 @30*0+1
    -- This is just a more flexible way of encoding the previous example.
       In this case, the container has an indefinite number of repetitions
       (i.e., "*0"), that will ultimately be determined by the number of
       available codestreams.  The presentation track is also defined by an
       indefinitely repeating frame, with a frame rate of 30fps ("@30" has no
       prefix to set the number of repetitions).
    -- In a custom application, these features are very useful for
       live video, since they allow an application to define the metadata
       up front and then pass an indefinite sequence of codestreams to the
       file writer.
 p) kdu_merge -i E1.jp2,O1.jp2,E2.jp2,O2.jp2,E3.jp2,O3.jp2,...
              -o out.jpx -containers 2-3*0 -composit 2@0*0+1
              -jpx_track 1:1 @30*0+1 -jpx_track 1:1 @30*0+1
    -- This example is similar to the above, but the idea is that the inputs
       consist of an alternating sequence of even and odd fields from an
       interlaced video (just an example -- there is nothing specific here
       about interlaced video itself).
    -- The top-level composition box defines two frames (the first two images)
       which are "PAUSE" frames, because they have a zero-valued FPS ("2@0").
    -- There is one container, which contains two base compositing layers
       (and implicitly two base codestreams that are required by these
       compositing layers) and is repeated indefinitely.
    -- Two separate presentation tracks are defined for the container.  The
       first track presents even fields E2,E3,E4,... at a frame rate of 30fps.
       The second track presents odd fields O2,O3,O4,... at a frame rate of
       30fps.
    -- Some simple labels are automatically added to the metadata to clarify
       the structure of the content and help you to get started with
       annotating the content in "kdu_show"; you can of course delete these
       and resave the file from within "kdu_show".
    -- Interlaced video is not a very serious application for containers and
       presentation tracks, but it makes for a simple example.  A more
       serious example would be hyperspectral video, consisting of a sequence
       of multi-component transformed codestreams, each offering a large
       number of interesting output component definitions (combining the
       hyperspectral planes in interesting ways).  Containers and tracks can
       then be defined to efficiently capture the various presentation options,
       so that a separate animated presentation track is provided for each
       interesting combination of the available codestream output components
       (e.g., a panchromatic track, one or more visible RGB tracks, various
       pseudo-colour tracks fromed from combinations of visible, infrared
       and/or ultraviolet components, and so forth).
 q) kdu_merge -i in.mjc -o out.mj2 -mj2_tracks P:0-
    -- Demonstrates that "kdu_merge" can accept the input video streams
       in the simple "MJC" file format, which is basically a trivial
       header followed by a concatenatation of codestreams, one per
       video frame.  Converting these to MJ2 here enables them to be
       conveniently browsed using the "kdu_show" applications (Windows or
       MAC).
    -- Note that the "kdu_merge" application can only use MJC files that
       have the special "CBR flag" in their header, indicating that each
       codestream has exactly the same size -- this allows the codestreams
       of interest to be extracted from the file by seeking to well-defined
       addresses.   The CBR flag is written by "kdu_v_compress" when it
       is used with the `Scbr' option, as demonstrated in the usage examples
       (f) and (g) for that application.

kdu_expand
----------
 In many ways this is the dual of kdu_compress.  It can write most of the
 file formats that kdu_compress can read, with the sole exception of the
 relatively uncommon PBM (bi-level) image file format that is used
 sometimes to store fax documents.  In particular, it can write
 low and high precision PGM/PPM, floating point PFM, BMP, TIFF (all
 precisions) and a range of raw sample data formats.

 Note 1: Like "kdu_compress", this is not the fastest of the image
 decoding applications, but it writes the widest range of image file
 formats.  If you are interested in measuring decode throughput, you
 should at least drop the "-o " argument (no output file), but
 preferably use the "kdu_buffered_expand" demo-app instead.

 a) kdu_expand -i in.j2c -o out.pgm
    -- decompress input code-stream (or first image component thereof).
 b) kdu_expand -i in.j2c -o out.pgm -rate 0.7
    -- read only the initial portion of the code-stream, corresponding to
       an overall bit-rate of 0.7 bits/sample.  It is generally preferrable
       to use the transcoder to generate a reduced rate code-stream first,
       but direct truncation works very well so long as the code-stream has
       a layer-progressive organization with only one tile (unless
       interleaved tile-parts are used).
 c) kdu_expand -i in.j2c -o out.pgm -region {0.3,0.2},{0.6,0.4} -rotate 90
    -- decompress a limited region of the original image (starts 30% down
       and 20% in from left, extends for 60% of the original height and
       40% of the original width).  Concurrently rotates decompressed
       image by 90 degrees clockwise (no extra memory or computational
       resources required for rotation).
    -- Note that the whole code-stream if often not loaded when a region
       of interest is specified, as may be determined by observing the
       reported bit-rate.  This is particularly true of code-streams with
       multiple tiles or spatially progressive packet sequencing.
 d) kdu_expand -i in.j2c -o out.pgm -fussy
    -- most careful to check for conformance with standard.  Checks for
       appearance of marker codes in the wrong places and so forth.
 e) kdu_expand -i in.j2c -o out.pgm -resilient
    -- similar to fussy, but should not fail if a problem is encountered
       (except when problem concerns main or tile headers -- these can all
       be put up front) -- recovers from and/or conceals errors to the
       best of its ability.
 f) kdu_expand -i in.j2c -o out.pgm -reduce 2
    -- discard 2 resolution levels to generate an image whose dimensions
       are each divided by 4.
 g) kdu_expand -i in.j2c -o out.pgm -record log.txt
    -- generate a log file containing all parameter attributes associated
       with the compressed code-stream.  Any or all of these may be
       supplied to "kdu_compress" (often via a switch file).
    -- note that the log file may be incomplete if you instruct
       the decompressor to decompress only a limited region of interest
       so that one or more tiles may never be parsed.
 h) kdu_expand -i in.j2c -cpu 0
    -- measure end-to-end processing time, excluding only the writing of
       the decompressed file (specifying an output file will cause the
       measurement to be excessively influenced by the I/O associated
       with file writing)
 i) kdu_expand -i in.j2c -o out.pgm -precise
    -- force the use of higher precision numerics than are probably
       required (the implementation makes its own decisions based on
       the output bit-depth).  The same argument, supplied to the compressor
       can also have some minor beneficial effect.  Use the `-precise'
       argument during compression and decompression to get reference
       compression performance figures.
 j) kdu_expand -i in.jp2 -o out.ppm
    -- decompress a colour image wrapped up inside a JP2 file.  Note that
       sub-sampled colour components will not be interpolated nor will
       any colour appearance transform be applied to the data.  However,
       palette indices will be de-palettized.  This is probably the most
       appropriate behaviour for an application which decompresses to a
       file output.  Renderers, such as "kdu_show" should do much more.
 k) kdu_expand -i huge.jp2 -o out.ppm -region {0.5,0.3},{0.1,0.15}
               -no_seek -cpu 0
    -- You could try applying this to a huge compressed image, generated in
       a manner similar to that of "kdu_compress" Example (r).  By default,
       the decompressor will efficiently seek over all the elements of
       the code-stream which are not required to reconstruct the small
       subset of the entire image being requested here.  Specifying `-no_seek'
       enables you to disable seekability for the compressed data source,
       forcing linear parsing of the code-stream until all required
       data has been collected.  You might like to use this to compare the
       time taken to decompress an image region with and without parsing.
 l) kdu_expand -i video.jpx -o frame.ppm -jpx_layer 2
    -- Decompresses the first codestream (in many cases, there will be only
       one) used by compositing layer 2 (the 3'rd compositing layer).
 m) kdu_expand -i video.jpx -o out.pgm -raw_components 5 -skip_components 2
    -- Decompresses the 3'rd component of the 6'th codestream in the file.
    -- If any colour transforms (or other multi-component transforms) are
       involved, this may result in the decompression of a larger number of
       raw codestream components, so that the colour/multi-component transform
       can be inverted to recover the required component.  If, instead, you
       want the raw codestream component prior to any colour/multi-component
       transform inversion, you should also specify the
       `-codestream_components' command-line argument.
 n) kdu_expand -i geo.jp2 -o geo.tif -num_threads 2
    -- Decompresses a JP2 file, writing the result in the TIFF format, while
       attempting to record useful JP2 boxes in TIFF tags.  This is only a
       demonstration, rather than a comprehensive attempt to convert all
       possible boxes to tags.  However, one useful box which is converted
       (if present) is the GeoJP2 box, which may be used to store geographical
       information.
    -- See "kdu_compress" example (y) for a discussion of the "-num_threads"
       argument.
 m) kdu_expand -i in.jp2 -o out.tif -stats -reduce 2
    -- The `-stats' option causes the application to report statistics on
       the amount of compressed data which has been parsed, in each successive
       quality layer, at the resolution of interes (in this case, one quarter
       the resolution of the original image, due to the "-reduce 2" option).
       The application also reports the number of additional bytes which were
       parsed from each higher resolution than that required for decompression
       (in this case, there are two higher resolution levels, due to the
       "-reduce 2" option).  This depends upon codestream organization and
       whether or not the compressed data in the codestream was randomly
       accessible.
 n1) kdu_expand -i in.jp2 -o out.tif -fprec 16
     -- The "-fprec" argument stands for "force precision".  The argument can
        be used in a variety of different ways.  In this case, the
        output file is written with 16-bit sample values, regardless of
        the precision of the originally compressed samples.
     -- In this example, the original sample bits find themselves in the
        least significant bit positions of each 16-bit word, which will
        produce a very dark (or even black) image if the original samples
        had a much lower precision (e.g. 8 bits /sample).
 n2) kdu_expand -i in.jp2 -o out.tif -fprec 16M
     -- As above, but the most significant bit position of each original
        sample is aligned with the most significant bit position of the
        16-bit output samples that are written.  This usually produces an
        image which displays correctly when opened in a viewer.
 n3) kdu_expand -i in.j2c -o out.tif -fprec 16F5
     -- This example of precision forcing tells the image file writer to
        first map the decompressed sample values to a 16-bit representation
        in a similar way to the -fprec 16M example above, but then to
        re-interpret the 16-bit integer bit-patterns as floating point
        numbers with a sign bit, followed by 5 exponent bits and then
        10 mantissa bits in the least significant bits -- this corresponds
        exactly to IEEE half-floats.
     -- The file writer here will actually write a floating-point TIFF file
        to represent the numerical values derived by re-interpreting integers
        as floats.  The image will have the correct appearance if the image
        was originally compressed by kdu_compress with "-fprec 16F5", as
        explained above in compression example (Ca).
     -- You will receive a warning message if you try to use this example
        on a codestream (in.j2c) that does not involve a Part-2
        non-linear point transform with type NLType=SMAG or NLType=UMAG,
        since compression of floating-point bit patterns as integers is
        usually a sensible thing to do only in the presence of such an
        NLT, as explained in kdu_compress examples (Ca), (Cb), etc.
 n4) kdu_expand -i in.j2c -o out.tif -fprec F5
     -- As above, but avoids forcing the decompressed data into integers with
        anything other than the precision identified in the codestream.
        The "F5" suffix tells the file writer to re-interpret the decoded
        data as floating-point bit patterns in with 5 exponent bits.
     -- If a raw codestream has been compressed with an SMAG or
        UMAG non-linear point transform, you will get a warning message if
        you fail to provide a floating-point "-fprec" option to "kdu_expand",
        since it is expected that the original compressed data should have
        been floating point bit-patterns interpreted as integers, for which
        such transforms are important for compression efficiency.
     -- In response to such warning messages, the simplest thing to do if
        you have no idea how the content was originally compressed is to
        try various exponents, such as "F5" (half-floats), "F8" (single
        precision floats) and perhaps other values, until you get a
        result that looks right.
     -- This kind of guesswork is only required when decompressing raw
        codestreams that involve a sign-magnitude non-linear point
        transform.  If the codestream is embedded within a JPX file,
        the JPX file should include a Pixel Format box that identifies
        the number of exponent bits (indirectly) and the kdu_expand
        demo-app uses this information to interpret the data correctly.
        You can, however, still force a different interpretation of the
        decompressed data via the "-fprec" option.

kdu_buffered_expand
---------------------
  This application offers many of the same features as "kdu_expand".  The
  primary difference is that this application buffers stripes of the output
  image(s) that it writes in memory, pulling these stripes from the higher
  level `kdu_stripe_decompressor' API, which also takes care of all format
  and buffer reorganization requirements.  By contrast, "kdu_expand"
  converts image samples line by line from one of the four fundamental
  internal representations, interleaves them (if necessary) and
  writes them to the supplied output files (if any).

  The `kdu_stripe_decompressor' API is strongly recommended for most users,
  alongside the `kdu_region_decompressor' and `kdu_region_compositor' API's.
  These high level API's all incorporate highly efficient SIMD data
  conversion functions for X86-family processors that can leverage
  advanced instruction sets such as SSSE3 and AVX2.  The
  `kdu_stripe_decompressor' also implements a very sophisticated work flow
  for decompressing images that contain many tiles, which is likely to
  produce a more efficient solution than an implementation you might
  choose to develop using the lower level `kdu_multi_synthesis' API's
  directly.

  To keep the code substantially less convoluted than "kdu_expand", this
  demo app offers support for only a smaller set of output file formats:
  8-bit PGM, 8-bit PPM, 8-bit BMP and raw files with up to 16 bits/sample.

 a) kdu_buffered_expand -i in.jp2 -o out.ppm
 b) kdu_buffered_expand -i in.jp2 -o out.bmp
 c) kdu_buffered_expand -i in.jp2 -o out.raw
 d) kdu_buffered_expand -i in.jp2 -cpu
    -- Note: when no output file is specified, image file I/O is avoided
       as a throughput bottleneck, so this is the best environment in which
       to obtain meaningful timing measurements.
    -- In this case, all data format conversion and buffer reoganization
       operations happen as usual to write to an internal memory buffer
       that would normally be dumped into an output file.
    -- By contrast, when no output file is supplied to the "kdu_expand"
       demo app, the decompression proceeds as usual, but no attempt is
       made to convert decompressed sample values from one of the four
       fundamental internal representations to a representation that
       would correspond more naturally to what most applications expect
       of a buffered image.  For this reason, throughput timing using this
       application is potentially more indicative than "kdu_expand"; in
       most cases, both will produce very close results, though,
       so long as an X86 processor with SSSE3 support is used, since
       those are the ones for which accelerated data conversion routines
       are provided out-of-the-box.
  e) kdu_buffered_expand -i in.jp2 -precise -cpu
     -- Explore the impact of doing all internal processing with floating
        point or 32-bit integer precision, depending on whether the
        source file specifies irreversible or reversible processing.

kdu_v_expand
------------
 This is the companion to kdu_v_compress. It can write both VIX and YUV
 formats, including high precsision YUV files, appending YUV filenames
 with a suitable suffix that reveals the precision, dimensions, colour
 format (including just Y or even RGB) and frame rate, using common
 conventions.

 a) kdu_v_expand -i in.mj2 -o out.vix
    -- Decompress Motion JPEG2000 file to a raw video output file.  For
       details of the trival VIX file format, consult the usage statement
       printed by `kdu_v_compress' with the `-usage' argument.
 b) kdu_v_expand -i in.mj2 -o out.vix -double_buffering 32
    -- Allows higher levels of thread parallelism by utilizing the
       double-buffered processing option of the core `kdu_multi_synthesis'
       class.  DWT operations are performed concurrently in each image
       component in this case, in addition to the usual concurrent block
       decoding operations.  The "double buffering" refers only to the
       use of two modest stripe buffers for image component samples, allowing
       one buffer to be generated by DWT synthesis processing while the
       other is subjected to any required colour transformations and
       transferred to the decompressed frame buffers.
 c) kdu_v_expand -i in.mj2 -o out.vix -double_buffering 32 -in_memory 4
    -- As above, but compressed video frames are loaded into memory in the
       background (while the previous frame is being decompressed) and each
       compressed frame is fully decompressed to an internal memory buffer
       4 times, only the last result is written to the output file.  The
       `-in_memory' option (without repeats) is automatically selected
       whenever favourable, unless you explicitly specify `-not_in_memory';
       it normally results in the highest throughput.  Repeated decompression
       of each frame allows you to assess the throughput that can be achieved
       in the absence of I/O bottlenecks.
 d) kdu_v_expand -i stream.jpb -o out.vix
    -- Decompress an elementary broadcast stream, writing result to a
       VIX file.

kdu_vex_fast
------------
    In the same way that "kdu_vcom_fast" extends "kdu_v_compress" to
    scenarios in which multiple compression engines can be constructed
    to run in parallel, each highly multi-threaded internally, the
    "kdu_vex_fast" application essentially extends "kdu_v_expand" to
    allow multiple frame decompression engines, each of which can be
    heavily multi-threaded.  As with "kdu_vcom_fast", the main purpose
    of this is to allow you to explore the optimal organization of
    threads and frame processing engines, noting that more engines means
    higher delay, while engines with a large number of threads can
    potentially be starved of work to do.

 a) kdu_vex_fast -i in.mj2 -o out.vix
    -- Does exactly the same thing as `kdu_v_expand', but in a slighly
       different way.  On multi-CPU platforms, the default behaviour here
       is to create one frame processing engine for every 4 physical/virtual
       cores advertised by the CPU.  This means that a typical processor
       with 4 CPU cores and 8 hardware threads will run 2 parallel frame
       processing engines, each of which deploys 4 threads to process
       each of its frames.  This represents a low latency configuration,
       with relatively low memory requirements, that typically achieves
       close to the maximum throughput.
 b) kdu_vex_fast -i in.mj2 -quiet
    -- Use this option to measure CPU time without the overhead of writing
       decompressed frames to disk.  All processing steps are taken and
       frames are written to an internal display buffer which could be
       blasted directly to a graphics card.  This option is identical to
       that obtained by specifying the "-display" argument without any
       target display parameter.
 c) kdu_vex_fast -i in.mj2 -quiet -engine_threads 2 2
    -- As above, but in this case 2 parallel frame processing engines are
       created and each one is assigned a multi-threaded processing
       environment with 2 threads.  This example would keep a 4-core
       machine, or one with 2 cores and hyperthreading, busy almost 100%
       of the time.   The default engine thread assignment for such a
       machine would be equivalent to "-engine_threads 4", which has the
       minimum possible delay and roughly half the memory consumption.
 d) kdu_vex_fast -i in.mj2 -quiet \
    -engine_threads 2:(0,1) 2:(2,3) 2:(4,5) 2:(6,7)
    -- Similar to the above example, but this example is targeted toward
       a machine with 8 cpu cores, where each pair shares a common L2
       cache.  Four frame processing engines are created to run in
       parallel, where each processing engine has 2 threads of execution,
       for parallel processing within the frame.  To maximize cache
       utilization efficiency, the pair of threads associated with each
       engine is assigned to be scheduled on a corresponding pair of CPUs
       which share the same L2 cache.
    -- The scheduling assignment is identified by the colon-separated
       CPU affinity descriptor which follows each engine's thread
       count.  For more on affinity descriptors, consult the `-usage'
       statement.
 e) kdu_vex_fast -i in.mj2 -quiet -engine_threads 4 4 -display W30
    -- This example is targeted towards a machine with 8 physical/virtual
       CPUs (e.g., a 4-core machine with hyperthreading).  In this case,
       the "W30" parameter to "-display" causes the video to be
       delivered at a constant frame rate of 30 frames/second (if possible)
       via DirectX9.  This option is supported only on Windows platforms,
       and then only if the application is compiled against the DirectX 9
       (or higher) SDK.  The interface is simple, but demonstrative.
 f) kdu_vex_fast -i in.mj2 -quiet -engine_threads 2 -display F30
    -- As above, but the video is displayed in full-screen mode with the
       most appropriate display size (and frame/rate) that can be found.
       Again, this option is available only when compiled against the
       DirectX9 SDK or higher.
 g) kdu_vex_fast -i in.mj2 -quiet \
    -engine_threads 2:(0,1) 2:(2,3) 2:(4,5) 2:(6,7) -trunc 3
    -- Similar to example d), except that not all of the compressed bits
       are decompressed.  A heuristic is used to strip away some final
       coding passes from code-blocks in order to trade quality for
       processing speed.  In this example, roughly 3 final coding passes
       (one bit-plane) is stripped away from every code-block; the
       parameter to `-trunc' can be a real-valued number, in which case
       the heuristic treats some blocks differently to others, based on an
       internal heuristic.  This method may be used to accelerate
       decompression in a similar way to stripping away final quality
       layers, except that the `-trunc' method does not rely upon the
       content having been created with multiple quality layers.

kdu_jp2info
-----------
 a) kdu_jp2info -i in.j2c
    -- print quick summary of the characteristics of a raw codestream file.
 b) kdu_jp2info -i in.j2c -siz
    -- as above, but also print all information provided by the SIZ marker
       segment.
 c) kdu_jp2info -i in.jp2 -siz
    -- print quick summary of the box structure of a JP2 file, as well as
       its embedded codestream, expanding all details of the codestream's
       SIZ marker segment.
 d) kdu_jp2info -i in.jp2 -boxes 256
    -- print a more detailed summary of the box structure of a JP2 file,
       expanding the contents of JP2 boxes wherever textualization facilities
       are provided for those boxes by the underlying Kakadu SDK.  Long
       boxes may be only partially expanded, based on roughly the first
       256 bytes of the box contents (you can specify any limit here).
 e) kdu_jp2info -i in.jpx
    -- get an overview of the box structure and codestream dimensions
       associated with a complex JPX file that might contain any number of
       codestreams, complex inter-linked metadata, etc.
 f) kdu_jp2info -i video.mj2
    -- get an overview of the structure of a Motion JPEG2000 file
 g) kdu_jpinfo -i stream.jpb -boxes 100 -siz
    -- print the structure, box contents, codestream dimensions and SIZ
       marker segments for every frame recorded in an elementary broadcast
       stream.
 h) kdu_jp2info -i in.jpx -hex 512
    -- provide a hex dump for boxes found within the JPX file, dumping at
       most 512 bytes from each box.
 i) kdu_jp2info -i in.jpx -boxes 128 -hex 128
    -- attempt to expand boxes found in the JPX file into meaningful textual
       descriptions, but provide a hexdump for box types that have no
       textualization service currently implemented in the Kakadu SDK.

kdu_transcode
-------------
NB1: From KDU-7.2, the "kdu_transcode" application is able to introduce or
     modify Part-2 multi-component transforms to an existing codestream, so
     long as the underlying codestream image components remain unchanged.
NB2: From KDU-7.2, the "kdu_transcode" application is able to output JPX
     files (JP2-compatible or otherwise) so long as you supply the metadata
     required to build compositing layers -- colour space and channel bindings.
     It is possible to write multiple compositing layers for a single
     codestream.
NB3: From KDU-7.2, the "kdu_transcode" application is able to accept JPX
     files containing multiple codestreams, transcoding each codestream
     in turn, and writing the result back to a JPX file with one or more
     compositing layers for each transcoded stream, so long as all streams
     are treated in the same way.

 a) kdu_transcode -i in.j2c -o out.j2c -rate 0.5
    -- reduce the bit-rate, using as much information as the quality layer
       structure provides.
 b) kdu_transcode -i in.j2c -o out.j2c -reduce 1
    -- reduce image resolution by 2 in each direction
 c) kdu_transcode -i in.j2c -o out.j2c -rotate 90
    -- rotate image in compressed domain.  Some minor distortion increase
       will usually be observed (unless the code-stream was lossless) upon
       decompression (with -rotate -90), but subsequent rotations or block
       coder mode changes will not incur any distortion build-up.
 d) kdu_transcode -i in.j2c -o out.j2c "Cmodes=ERTERM|RESTART" Cuse_eph=yes
                  Cuse_sop=yes
    -- Add error resilience information.
 e) kdu_transcode -i in.j2c -o out.j2c Cprecincts={128,128} Corder=PCRL
    -- Convert to spatially progressive organization (even if precincts
       were not originally used).
 f) kdu_transcode -i in.jp2 -o out.j2c
    -- Extracts the code-stream from inside a JP2 file.
 g) kdu_transcode -i in.j2c -o out.j2c Cprecincts={128,128} Corder=RPCL
                  ORGgen_plt=yes
    -- You can use something like this to create a new code-stream with
       all the information of the original, but having an organization
       (and pointer marker segments) which will enable random access
       into the code-stream during interactive rendering.  The introduction
       of precincts, PLT marker segments, and a "layer-last" progression
       sequence such as RPCL, PCRL or CPRL, can also improve the memory
       efficiency of the "kdu_server" application when used to serve up
       a very large image to a remote client.
 h) kdu_transcode -i in.j2c -o out.j2c
    Mcomponents=6 Ncomponents=6 Nsigned=no Nprecision=8 Mmatrix_size:I7=9
    Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
    Mvector_size:I1=3 Mvector_size:I2=3
    Mvector_coeffs:I1=128,128,128 Mvector_coeffs:I2=164,164,164
    Mstage_inputs:I16={0,2} Mstage_outputs:I16={0,2}
    Mstage_collections:I16={3,3} Mstage_xforms:I16={MATRIX,7,0,0,0}
    Mstage_inputs:I25={0,2},{0,2} Mstage_outputs:I25={0,5}
    Mstage_collections:I25={3,3},{3,3}
    Mstage_xforms:I25={MATRIX,0,1,0,0},{MATRIX,0,2,0,0}
    Mnum_stages=2 Mstages=16,25
    -- Things are really starting to hot up here.  We are transcoding an
       existing Part-1 codestream "in.j2c", which was compressed using the
       conventional irreversible decorrelating colour transform, into a
       Part-2 codestream that uses a two-stage multi-component transform.
    -- The number of codestream image components must not change (3 in this
       case), but the number of output image components in this case is 6.
    -- The first stage (during decompression) of the MCT implements the
       3x3 matrix transform that corresponds exactly to the Part-1
       irreversible decorrelating transform in the original codestream (so
       as to preserve the colour samples).
    -- The second stage of the MCT provides two transform blocks, each of
       which accepts the 3 transformed (to RGB) colour channels from the first
       stage and produces 3 separate output channels, for a total of 6.  The
       first set of output channels are processed simply by adding 128.  The
       second set is processed by adding 164, increasing their intensity.
    -- We could extend this example to present numerous output channels,
       each with different contrast, brightness or even colour properties.
 i) kdu_transcode -i in.j2c -o out.jpx -jpx_layers sRGB,0,1,2+sRGB,3,4,5
    Mcomponents=6 Ncomponents=6 Nsigned=no Nprecision=8 Mmatrix_size:I7=9
    Mmatrix_coeffs:I7=1,0,1.402,1,-0.344136,-0.714136,1,1.772,0
    Mvector_size:I1=3 Mvector_size:I2=3
    Mvector_coeffs:I1=128,128,128 Mvector_coeffs:I2=164,164,164
    Mstage_inputs:I16={0,2} Mstage_outputs:I16={0,2}
    Mstage_collections:I16={3,3} Mstage_xforms:I16={MATRIX,7,0,0,0}
    Mstage_inputs:I25={0,2},{0,2} Mstage_outputs:I25={0,5}
    Mstage_collections:I25={3,3},{3,3}
    Mstage_xforms:I25={MATRIX,0,1,0,0},{MATRIX,0,2,0,0}
    Mnum_stages=2 Mstages=16,25
    -- This example is the same as the last one, except that we now write
       a JPX file that has one compositing layer for each of the two sets
       of RGB channels produced by the Multi-Component transform.
    -- Try viewing this file in "kdu_show" and switching back and forth
       between the two compositing layers using the enter and backspace
       keys, for example.

kdu_transcode advanced Part-15 (HTJ2K) Features
-----------------------------------------------
    These additional examples relate to Part-15 of the JPEG 2000 standard,
    also known as HTJ2K (High Throughput JPEG 2000), or simply JPH.  The
    examples here will grow considerably in the coming months, but first
    we just provide some very simple examples to get you going.

Ha) kdu_transcode -i src.jp2 -o htj2k_out.j2c Cmodes=HT
    -- Transcodes any JP2 source to an HTJ2K codestream, using the HT
       fast block coding algorithm.
    -- This works even for sources with multiple quality layers, and
       you get an output that also has all the same quality layers,
       even though HTJ2K is not really quality scalable.  Try
       viewing a multi-layer JPH file in kdu_show, and reducing the
       number of rendered quality layers using the "<" accelerator key.
       You will notice that hte lower quality layers have either no
       information, or almost no information, but they do retain all
       the information required to get back to the original JP2 file.
Hb) kdu_transcode -i htj2k_src.jph -o no_ht.j2c Cmodes=0
    -- This can be used to convert a codestream to one that uses the
       original fully embedded (non-HT) JPEG 2000 block coding algorithm,
       even if the source codestream (htj2k_src.j2c here) used the HT block
       coding algorithm.
    -- If there are multiple quality layers, the resulting codestream
       will have all the expected quality scalability, even if the
       source used the HT block coding algorithm that is not really
       itself quality scalable.  This is because HTJ2K codestream
       packet headers can describe quality layer boundaries via so-called
       "placeholder passes".
    -- NOTE: the "no_ht.j2c" codestream produced here is actually marked as
       an HTJ2K codestream (if the input was), even though it does not use
       the HT block coding algorithm.  This is because we have only changed
       the block coding mode, but not the codestream type.  HTJ2K
       codestreams are compatible with all other JPEG 2000 family
       features, and can use the HT block coding algorithm for some,
       none or all code-blocks.
Hc) kdu_transcode -i htj2k_src.j2c -o j2k_out.j2c Sncap=P15
    -- Demonstrates the best way to completely remove all Part-15
       (HTJ2K) capabilities and features from a codestream, via the
       "negated capabilities" attribute Sncap.  You can use this with
       other capability families also, such as "Sncap=P2", to remove
       (or deny) the relevant capabilities from the codestream.
       The "Sncap=P15" value here, not only removes Part-15 support
       indication from the `Scap' attribute that goes in the SIZ marker
       segment, but also removes the `Cmodes_HT' and `Cmodes_HTMIX' block
       coding mode flags, without altering any other mode flags that might
       still remain valid (Cmodes=CAUSAL is particularly important to
       retain, since it must be retained if present to ensure truly
       reversible transcoding).
    -- The "j2k_out.j2c" codestream produced here will conform to Part-1
       of JPEG 2000 unless there were other capabilities (e.g., from Part-2)
       which could also be removed, as described above.  Transcoding
       might not always be possible after removing certain capabilities,
       but a codestream that contains Part-15 (HTJ2K) features can always
       be transcoded to one that does not, in a truly reversible manner,
       which includes preservation of all profile information from any
       original JPEG 2000 source file that might have been trancoded to
       Part-15.  This is a very important feature that ensures that
       transcoding to/from Part-15 really loses no information at all.
Hd) kdu_transcode -i htj2k_layered_src.j2c -o htj2k_out.j2c Clayers=1
    -- This demonstrates a really cool feature.  An HTJ2K codestream can
       be produced by direct encoding or by transcoding from an existing
       JPEG 2000 source codestream, and in both cases can have multiple
       quality layers, even though the HT block coding algorithm itself
       is not really quality scalable.  Later, the codestream can be
       transcoded back to a non-HT representation, restoring all quality
       layer boundaries, as demonstrated in (Hb) and (Hc) above.  However,
       his example shows that it can also be transcoded to another HTJ2K
       codestream that has a smaller number of quality layers, and the
       effect is the same as if it had been transcoded to a non-HT
       representation, then transcoded again to discard quality layers,
       then transcoded back to the HT representation.  Internally, this
       is achieved by decoding and re-encoding code-blocks (not always
       required) to the quality associated with the desired quality layer,
       which is already recorded in the original codestream.
He) kdu_transcode -i src.jp2 -o htj2k_out.j2c Cmodes=HT SCP15_limb=11
    -- Same as example (Ha), but in this example the MAGB (HT block
       coder precision bound) attribute is bounded by specifying
       `SCP15_limb' to be 11.  The limit makes no difference unless
       the system thinks that a value larger than 11 is required to
       be sure of maintaining all quantized data without error.
    -- It is possible to explicitly specify a desired `SCP15_magb'
       value, but that may leave a value that is unnecessarily large,
       possibly discouraging low powered decoders from attempting to
       process the codestream.  The `SCP15_limb' attribute supplied
       here does not force the MAGB value to be 11 -- it can still be
       smaller.

kdu_show
--------
     "kdu_show" is a powerful interactive viewing, browsing and metadata
  editing application.  Almost all the implementation complexity is
  buried inside the platform independent `kdu_region_compositor',
  `kdu_region_animator' and `kdu_client' objects.
     "kdu_show" exists on Windows and MAC platforms, providing mostly the
  same functionality on both operating systems (at least that is the intent).
  One difference between Windows and MAC is the accelerator keys.
  Specifically, the MAC version uses the command key for some accelerators
  where the Windows version uses the control key; this is done only to
  maintain standard conventions for applications in these two environments.
     There is also a quite separate version of kdu_show that runs very
  efficiently on IOS devices, which shares the same rendering brain as
  kdu_macshow.  The IOS version of kdu_show is not included automatically
  with the standard Kakadu SDK, but can be obtained separately as a
  sophisticated starting point that could save a lot of time in developing
  your own mobile apps based on Kakadu.  The description below covers only the
  desktop versions of kdu_show that are included with standard Kakadu
  releases.
     At the time of this writing, the kdu_macshow program is much more
  user-friendly than kdu_winshow, because it has recently been revamped
  to show off most of the more advanced features of the underlying
  `kdu_region_compositor' (and `kdu_region_animator') high level Kakadu
  API's.  The kdu_macshow application provides a similar browsing feeling
  to what one might expect from a tablet or phone, augmented by the
  additional capabilities provided by more user input devices, a larger
  display surface and control over window size.  Internally, it contains
  a large component (the rendering brain) that abstracts all rendering,
  animation and most user interface features while remaining (almost)
  platform neutral.  A further revised version of this will be the heart
  of all variations of kdu_show from version 7.11 onwards.
     You can learn to use "kdu_show" as you would any desktop application,
  by following the menu item descriptions and taking advantage of the
  tooltips and accelerator keys provided for most menu items, as well as just
  playing around with the mouse (left-clicks, right-clicks, double-clicks,
  left-mouse drags and shift-left-mouse drags all have useful behaviours).
  Since "kdu_show" now offers a great deal more than it did originally, we
  also provide a separate small manual, which may be found in the file,
  "kdu_show.pdf".  At this point, however, we simply summarize
  some of the key features and give some useful accelerators which you will
  probably use a lot.
  
  Partial Feature List:
  * You may open new image files at any time.  You can also open multiple
    windows within the same application and you can arrange for menu
    commands to be "broadcast" to all windows at once -- useful for animation,
    rotation or zooming.
  * Opens JP2 files, JPX files, unwrapped JPEG2000 code-streams, and
    Motion JPEG2000 files, using the file contents (rather than the file
    name suffix) to distinguish between the different formats.
  * You may re-open a failed image file (often after setting the "mode" to
    "resilient" or "resilient+SOP assumption").
  * You may view code-stream parameters and the tile structure
    using the File->Properties menu item (ctrl-P / cmd-P).
       -- Note that double-clicking on any code-stream parameter attribute
          displayed in the popup window will bring up a description of
          the attribute.
  * You may examine individual components (typically, the colour components)
    of an image, individual compositing layers of a multi-layer image, or
    navigate between composited frames of an animation of video.  Compositing
    layers, image compositions and animation are JPX features.
  * You may view the metadata structure of any JP2-family file, using the
    "metashow" feature, which is accessed via the Metadata menu.
  * A metadata catalog sidebar automatically opens to display any JPX/JP2
    metadata labels, including cross-links (shown like hyperlinks with
    colour coded semantics).  The catalog sidebar is tightly integrated
    with the image view.  Clicking on region-of-interest overlays within the
    image view takes you to any relevant catalog entry, for example, while
    double-clicking catalog entries causes the image view window to change
    (if appropriate) to display associated compositing layers, codestreams
    and/or regions of interest.
  * Click and drag in the image window to define a focus box (hit "f"
    to remove a current focus box or "h" to change the way it is highlighted).
    Focus boxes are used to centre "zoom in" operations, to identify regions
    of interest during JPIP browsing sessions (see below), and to define
    regions to be labeled with new metadata.
  * Use the Metadata menu (or appropriate accelerators) as one way to
    add metadata to the image.  Doing this without a focus box will, by
    default, associate metadata with the current compositing layer or
    codestream (depending on the viewing mode).  With a focus box in place,
    the new metadata will be associated with the corresponding region of
    the top-most visible codestream, but you can change all the associations
    manually inside the metadata editor if you like.
    (try hitting ctrl-A / cmd-A).
  * The metadata editor provided to enable the above features allows you to
    navigate amongst sibling and parent/child relationships.  It also allows
    you to save metadata boxes to files, load them from files and change their
    image entity and/or region of interest associations.  Also, you can edit
    XML and other types of metadata, rather than just labels, by selecting
    appropriate external file-based editors from a pop-up list.
  * The metadata editor includes a powerful region-of-interest shape editor
    that starts automatically if you are editing an ROI description node.
    The shape editor has three modes (vertex-mode, edge-mode and path-mode).
    Play around with them to get an idea what the differences are.  In path
    mode you can define paths (typically you would fix the path width to 1
    when doing this) that can subsequently be filled (if you like).  There is
    also the option to scribble a boundary and have it automatically filled
    with a region approximation algorithm whose complexity you can select.
  * You can save the current image as a raw code-stream, a JP2 file or a
    JPX file, although raw originals must currently be saved as raw outputs
    and vice-versa.  You can even save over the currently open file -- this
    actually writes a file with a modified name (appends the emacs "~"
    character) which is replaced over the current file if all goes well, when
    the application exits, or the file is closed.  These capabilities allow
    for convenient interactive editing of a file's metadata, whereby you
    can mark up regions with arbitrary labels and have the information
    preserved.  There is also a menu option which allows you to save just
    the header and metadata structure of an image and reference the
    codestreams via links into to their original files.  This can be
    particularly convenient when editing metadata (e.g., marking up regions)
    for very large images.
  * There is a special "Scale X2" feature which can be used to represent
    each rendered image pixel with a 2x2 block of display pixels.  This is
    similar to zooming, but the key difference is that zooming tries to
    take advantage of the wavelet transform to render as little data as
    possible.  Thus, zooming out (say, to 50%) while using the "Scale X2"
    feature allows you to discard the highest resolution DWT coefficients
    but still get a displayed image which is large enough to allow you to
    distinguish the original rendered image pixels on most displays.  The
    "Scale X2" feature is also faster than "Zoom In" as a mechanism for
    displaying enlarged images -- this can make a difference in demanding
    video applications.
  * In the kdu_macshow variant, the "Scale X2" feature is augmented
    with retina display features and auto-scaling for retina displays
    occurs normally to give you the best possible display of any image
    content on whatever screen the window happens to sit or move to.
  * You can control the number of threads used for decompression processing
    through the "Modes" menu. By default, the number of threads used for
    processing is set based on the number of hardware threads offered by
    you platform.
  * "kdu_show" also contains powerful animation features.  It can be used to
    play video sources (forwards, backwards, variable speed, etc.), but it
    can also be used to play metadata-driven animations.  A metadata-driven
    animation is initiated by holding the shift key down and double-clicking
    (or pressing enter) over an entry in the metadata catalog side-bar.  If
    the metadata entry or any of its descendants are associated with imagery
    or image regions, these will be assembled into an animation that walks
    through the imagery and image regions in a nice way -- give it a go.
  * All of the above features also work when the image, composition,
    video or animation is remotely located and served via JPIP.

  Some useful accelerators:
  -- ctrl-o / cmd-o              -> open file
  -- ctrl-u / cmd-u              -> open URL (via JPIP)
  -- ctrl-w / cmd-w              -> close the window
  -- ctrl-q / cmd-q              -> quit the application
  -- ctrl-d / cmd-d              -> duplicate currently window
                                    (this is especially interesting with JPIP)

  -- w                           -> widens the display
  -- s                           -> shrinks the display
  -- arrow keys                  -> rapid navigation
  -- shift + left mouse button   -> pan view window or focus box using mouse
  -- ctrl+z                      -> zooms out
  -- z                           -> zooms in
  -- shift+ctrl+z                -> zooms out a little bit
  -- shift+z                     -> zooms in a little bit
  -- alt+z                       -> find nearest zoom for optimal rendering
  -- cmd+ and cmd-               -> magnify/demagnify natural zoom/resize
  -- shift+s                     -> shrinks the focus box
  -- shift+w                     -> widens the focus box
  -- shift+arrow keys            -> moves the focus box
  -- f                           -> disables focus box
  -- h                           -> modify highlighting of focus box
  -- ctrl-p/cmd-p                -> show properties
  -- ctrl-m (cmd-m)              -> activate "metashow"; note that clicking on
                                    various items in the metadata tree can have
                                    useful navigational side effects, as
                                    described in parentheses next to those
                                    items
  -- ctrl-shift-c/cmd-shift-c    -> active/deactivate metadata catalog
  -- ] and [                     -> rotate clockwise and counter-clockwise

  -- 1,+,-                       -> enter single-codestream, single-component
                                    mode and display image component 1,
                                    display the next component (+), or the
                                    previous component (-)
  -- L                           -> enter single compositing layer mode
                                    (equivalent to the full colour image, for
                                    files with only one compositing layer,
                                    including JP2 files)
  -- c                           -> enter composited image mode, displaying
                                    the complete composited result associated
                                    with a single animation frame.  If there
                                    are no composition instructions in the
                                    file, this is equivalent to "L", displaying
                                    a full colour image
  -- <ENTER>,<BACKSPACE>         -> move forward or backward amongst the
                                    sequence of frames (in composited image
                                    mode or when viewing Motion JPEG2000
                                    tracks), the sequence of compositing layers
                                    (in single layer mode), or the sequence
                                    of codestreams (in single component mode).
                                 -> if the metadata catalog sidebar has focus,
                                    these keys have a different interpretation;
                                    ENTER navigates the image view to reveal
                                    imagery associated with currently selected
                                    metadata, while BACKSPACE deletes selected
                                    metadata (after raising a confirmation
                                    dialog to be safe).
  -- <,>                         -> adjust number of quality layers, refreshing
                                    the display to reveal the rendered result
                                    obtained from using only those quality
                                    layers
  -- ctrl-t / cmd-t              -> toggle the status bar contents
                                    (lots of useful info here)
  -- ctrl-a / cmd-a              -> add metadata (opens the metadata editing
                                    dialog box)
  -- alt-o                       -> toggle metadata overlay mode
                                    (flashing->static->off)


  -- <right click>/ctrl-e/cmd-e  -> edit existing metadata
  -- ctrl-<left click>           -> same as <right click>
  -- <double click>              -> great for navigating between frames of a
                                    JPX composition which contain common image
                                    content -- or navigating the image view
                                    based on metadata in the catalog view.
  -- ctrl-c / cmd-c              -> copy label in metadata catalog
  -- ctrl-x / cmd-x              -> cut any item in metadata catalog
  -- ctrl-L / cmd-L              -> prepare link to metadata item in pastebar
  -- ctrl-v / cmd-v              -> paste metadata pastebar as child of item  
  -- ctrl+alt+z                  -> undo metadata shape editing operation
  -- ctrl+shift+alt+z            -> redo metadata shape editing operation

  -- alt-ENTER                   -> Play any available JPX animation or
                                    Motion JPEG2000 track.
  -- alt-BACKSPACE               -> Play animations backwards
  -- alt-.                       -> Stop animations
  -- shift-<double click>        -> Play metadata-driven animation (in catalog)
  -- shift-ENTER                 -> same as shift-<double click>

  Useful gestures (currently kdu_macshow only):
  -- two finger pan              -> pan/flick view or focus box
  -- two finger pinch            -> magnify view or focus box
  -- two finger rotate           -> rotate view
  -- shift + touch               -> pan/flick/push view or focus box like on
                                    a phone or tablet.

  Invoking kdu_show from the command-line:
    * The Windows version of kdu_show accepts an optional file name or URL (see
    below) when invoked from the command-line, which may be used to open an
    initial image.
    * The Mac version of kdu_show can also be invoked from the command-line
    (i.e., from a BSD shell terminal) using the "open" command, as in:
    "open -a kdu_show" or "open -a kdu_show test.jp2".  If you do a lot of
    work from the command-line, it could be a good idea to define "kdu_show"
    as an alias for "open -a kdu_show".

  A few words about HTJ2K codestreams (or JPH files):
    Unsurprisingly, "kdu_show" can view any content compressed using the
    new High Throughput JPEG 2000 (HTJ2K) standard -- JPEG 2000 Part-15,
    and of course it can do so very quickly!  However, it is worth
    understanding one small point of differentiation between HTJ2K
    content and content compressed using the original JPEG 2000 algorithm.
       Specifically, you should be aware that the HT block coding algorithm is
    not fully embedded, possessing very little real quality scalability,
    even though you can create HTJ2K codestreams (or JPH files) with multiple
    quality layers (Clayers attribute).
       HTJ2K codestreams with multiple quality layers retain all relevant
    information about the quality boundaries within each code-block, so
    that this information can be used during transcoding.  In particular,
    kdu_transcode can be used to transcode multi-layer content to another
    HTJ2K codestreams, or to a traditional JPEG 2000 codestream, discarding
    quality layers, and the result is exactly what you might expect.  Multi-
    layer HTJ2K codestreams are mostly provided so that you can preserve
    quality layer boundaries from an original JPEG 2000 codestreams, during
    transcoding, and restore them when transcoding back.
       The "kdu_show" application allows you to render content with a reduced
    number of quality layers -- easiest way to do this is via the "<" and ">"
    accelerator keys.  For JPEG 2000 content that does not use the HT block
    coder, this produces the expected behaviour.  For content that uses the
    HT block coder of JPEG 2000 Part-15 for some or all of the code-blocks,
    discarding quality layers would actually produce large degradations in
    visual quality since the encoded bits actually sit within the last one
    or two quality layers, in most cases.  To avoid this, from version 8.0.2,
    Kakadu treats quality layer constraints differently when rendering content
    containing HT code-blocks.  In particular, when the number of quality
    layers is constricted during a rendering process, Kakadu actually parses
    all the layers anyway, for precincts that may hold HT code-blocks, and
    it always passes sufficiently many of the available quality layers to
    the block decoder as required to ensure that at least the first HT
    Cleanup pass is decoded, if there is one.
       The result is that you may not see a visual change in image quality
    when you use the "<" and ">" accelerators in kdu_show with multi-layer
    HTJ2K content (if you do, the change will be only small, due to
    discarding of at most the HT refinement passes that follow the HT
    Cleanup pass).  However, you can tell that this is going on by looking
    at the bottom-right status panel (after possibly toggling the status
    to the relevant display mode).  You will see in this panel something
    like "Q.Lyrs=6/20", as an example, when rendering only the first 6
    of 20 quality layers for regular JPEG 2000 content, while for HTJ2K
    content you will see something like "Q.Lyrs=6+/20", meaning that at
    least 6 quality layers are being rendered, but maybe more for HT
    code-blocks.  The "+" only shows when the multi-layer content uses the
    HT block coding algorithm.
                                    
  A few words on JPIP browsing:  
    "kdu_show" is also a fully fledged remote image/video/metadata browser,
    capable of communicating with the "kdu_server" application (or any 3'rd
    party application which provides a sufficiently comprehensive
    implementation of the JPIP standard (JPEG2000 Part 9).  Video/animation
    browsing works only for JPX files at present, but JPX files can support
    arbitrarily long videos or composited animations very efficiently,
    with much more flexibility than MJ2.
    -- To open a connection with a remote server, you can give the URL as
       an argument to "kdu_show" on start up, or you can use the
       "File:Open URL" menu item.  The latter option allows you to customize
       proxy settings (if you need to use a proxy), cache directories, and
       protocol variants.  These settings are also used when you open
       a URL directly from the command line using something like
          kdu_show jpip://kakadu.host.org/huge.jp2
       or
          kdu_show http://kakadu.host.org?target=huge.jp2&fsiz=640,480&roff=100,20&rsiz=200,300
       For specific information on the syntax of JPIP URL's consult the
       information and links provided in the "jpip-links-and-info.html" file
       within the "documentation" directory.
 
       The "File:Open URL" menu item brings up a dialog box, which allows
       you to enter the name of the file you wish to browse.  This is
       actually the resource component of the JPIP URL and may contain
       a query sub-string (portion of the URL following a '?' symbol).  Query
       strings allow you to construct your own explicit JPIP request, so long
       as you know the JPIP request syntax.  If a non-empty query contains
       anything other than a target file name (JPIP "target" field), only
       one request will ever be issued to the server, meaning that interactive
       requests will not be generated automatically as you navigate around
       the image.  Otherwise, all the interesting requests are generated
       for you as you zoom and pan the view window, or a focus window, or
       as you adjust the image components or number of quality layers to
       be displayed.  If you are interested in finding out more about the
       JPIP syntax without reading any documents, you might like to run
       a copy of the "kdu_server" application locally, specifying the
       `-record' command line option -- this prints a copy of all requests
       and all response headers.

       The "File:Open URL" menu item also allows you to select one of four
       options in the "Channels and Sessions" drop-down list.  For the most
       efficient client-server communication, with the most compact requests
       and server administered flow/responsiveness control, select the
       "http-tcp" or "http-udp" option.  These use HTTP for request/response
       communication and an auxiliary TCP/UDP connection for the server
       communicated image and meta-data stream.

       All communication uses port 80 by default, to minimize firewall
       problems, but many organizations insist that all external traffic
       go through an HTTP proxy.  If this is the case, only pure HTTP
       communication will work for you, so you should select the "http"
       option in the "Channels and Sessions" drop-down list.  If the
       server offers insufficient support, "http-udp" is automatically
       downgraded to "http-tcp", which is automatically downgraded to "http"
       only or even "none", as required.  Kakadu's JPIP server supports
       all modes, but special command-line arguments are required to activate
       UDP communications.
       
       The final option in the "Channels and Sessions" drop-down list is
       "none", meaning that no attempt will be made to create a JPIP channel
       for which the server would be obliged to manage a persistent session.
       In this case, communication with the server proceeds over HTTP, but is
       stateless, meaning that all requests are idempotent, having no side
       effects.  In this mode, each request must carry sufficient information
       to identify the relevant contents of the client's cache, so that the
       server need only send the missing items.  This is by far the least
       efficient form of communication from virtually all perspectives:
       network traffic, client complexity and server complexity/thrashing.
       It is provided principally to test Kakadu's support for stateless JPIP
       communication.  Nevertheless, you may find it necessary to use this
       mode if you have an extremely unreliable network connection and
       are required to communicate via HTTP/1.0 proxies.

       Note that HTJ2K codestreams (and JPH files) can be browsed using
       JPIP, just like original JPEG 2000 code-streams.  However, because
       the HT block coder is not quality scalable, the browsing experience
       will not be nearly so responsive -- the most valuable aspect of
       quality scalability is that browsing over low bandwidth channels
       becomes extremely responsive and efficient.  Also, if you happen
       to restrict the number of quality layers, using the "<" and ">"
       accelerator keys, during JPIP browsing of multi-layer HTJ2K content
       (a strange situation), you will notice that the progress bar never
       reaches full quality (and it should also be visually obvious that
       something is missing) until you raise the number of quality layers
       using the ">" accelerator.  The reason for this is that JPIP
       requests include the quality layer constraint, yet the internal
       (portable) rendering tools used by "kdu_show" know that HT code-blocks
       generally need all quality layers to provide a good visual result,
       as explained above under "A few words about HTJ2K codestreams", which
       is reflected by the "progress" indicator.

kdu_server
----------
  To start an instance of the "kdu_server" application, you need not supply
any arguments; however, you may find the following command line options
useful:
  * kdu_server -u
    -- Prints a brief usage statement
  * kdu_server -usage
    -- Prints a detailed usage statement
  * kdu_server -address localhost -port 8080
    -- This sort of thing should always work, even if you're not connected to
       the internet.  Sets the server to use the local loopback IP address of
       127.0.0.1 with a port that should not be already taken by another
       HTTP server you may have running on your machine.
  * kdu_server -passwd try_me
    -- Enables remote administration via the "kdu_server_admin" application
  * kdu_server -wd /users/me/my_images -restrict
    -- Sets "/users/me/my_images" to be the working directory and restricts
       access to images in that directory or one of its descendants
       (sub-directories).
  * kdu_server -log \my_images\jpip_service.log
    -- Redirect all logs to the specified log file, rather than having them
       go to stdout.  If the log file path is not absolute, it is expressed
       relative to the directory within which "kdu_server" is invoked, not
       the "-wd" directory.
  * kdu_server -record
    -- Sends a record of all human-readable communication (to and from the
       client) to standard out, intermingled with the regular log file
       transcripts.  The volume of this communication can be large if the
       channel transport type selected by the client is "none" or "http".
  * kdu_server -clients 5
    -- Set the maximum number of clients which can be served simultaneously
       to 5.
  * kdu_server -sources 3 -clients 7
    -- Serve up to 7 clients at once, but no more than 3 different images at
       once: the server shares image resources amongst clients.
  * kdu_server -clients 3 -max_rate 80000
    -- Set the maximum number of bytes per second at which data will be
       shipped to any given client.  The limit is currently 10000 bytes/s,
       which gives quite a convincing (and usable) demonstration of the
       spatial random access properties of the EBCOT compression paradigm
       and its incarnation in JPEG2000.
  * kdu_server -restrict -delegate host1:81*4 -delegate host2:81*8
    -- Commands like this show off some of the more advanced capabilities of
       the "kdu_server" application.  The server delegates incoming client
       requests to alternate hosts.  The "host1" machine is presumably
       executing an instance of the "kdu_server" application, configured
       to listen on port 81.  "host2" is presumably doing the same.
       The "*4" and "*8" suffices are host loading indicators.  The server
       will delegate 4 consecutive requests to "host1" before moving on to
       delegate 8 consecutive requests to "host2", returning then to "host1".
       This sequence is broken if one of the hosts refuses to accept the
       connection request; in that case, the other host is used and its
       load counter is started from scratch.  There is no way to predict
       the real load on the two machines, since they do not provide direct
       feedback of this form.  Nevertheless, the load sharing algorithm
       will distribute an expected load in proportion to the supplied load
       sharing factors.  The algorithm also encourages the frequent re-use
       of machines which are known to be good, minimizing failed connection
       attempts to machines which may be temporarily out of service.  The
       principle server will perform the service itself only if all delegates
       refuse to accept the connection (either they are out of service, or
       have reached their connection capacity).
          It is worth noting that delegation is not used if the client's
       communication is stateless ("Channels and Sessions" drop-down box in
       the "File:Open URL" dialog is set to "none").  This is because
       stateless requests are served immediately, while the first request
       which specifies a transport type of "http-tcp" or "http" serves to
       create a new session on the server.  Regardless of the reasons for
       its existence, this policy may be quite convenient, since it allows
       you to employ one host to serve stateless requests (these are
       less efficient, often substantially so) and different hosts to serve
       session-oriented requests.

  The "kdu_server" application can ship any valid JPEG2000 file to a
remote client.  However, some tips will help you create (or transcode)
compressed images which minimize the memory resources and loading burden
imposed on the server.
  1) It is generally recommended that you compress the original image
     using 32x32 code-blocks (Cblk={32,32}) instead of the default 64x64
     code-blocks.  This can be helpful even for very large images, but if
     the original uncompressed image size is enormous, larger
     code-blocks can help reduce the internal state memory resources
     which the server must dedicate to the client connection.
  2) If the image is moderate to large (or even huge) in size (say above
     1Kx1K, but becoming really important above 10Kx10K), it is recommended
     that you insert information into the code-stream which will enable
     the server to access it in a random order.  Specifically, you should
     insert PLT marker segments (ORGgen_plt=yes), use moderate precinct
     dimensions (Cprecincts={256,256} or Cprecincts={128,128}) and employ
     a fixed, "layer-last" progression order -- RPCL (preferred), CPRL
     or PCRL.  The "kdu_compress" examples (r) and (t) and the "kdu_transcode"
     example (g) should provide you with guidance in these matters.  It
     currently appears that tiling the image offers no significant
     advantages for remote browsing of JPEG2000 content.  In my personal
     experience, untiled images seem to work very well without the ugly
     tiling artefacts which immediately stand out when tiled images are
     browsed over low bandwidth connections.  Moreover, the server has
     to do a lot of extra work to serve low resolution image content from
     a tiled image.  32x32 code-blocks are still a good idea when working
     with very large images.  Don't forget to provide lots of quality layers;
     if you only have one quality layer, the browsing experience will still
     be effective, but probably not much better than other popular geospatial
     remote browsing tools, which are not based on JPEG2000.
     With plent of quality layers, however, the server delivers a truly
     quality progressive experience over the view window of choice at any
     resolution of interest, which gives you an effective experience even at
     extremely low data rates.  Of course, Kakadu's client and server tools
     do a great deal more than remote image delivery.  3D imagery, compressed
     using multi-component transforms, for example, is delivered extremely
     efficiently, taking all the signal processing properties of the
     3D transforms into account to give you the most relevant information
     as quickly as possible for the fastest possible incrementally improving
     result at the client.
```