---
layout: post
title: An Overview of Overviews
categories: [gdal, geospatial, gis, wms, webmapping]
author: jayvarner
---
## Internal Tiling and Overviews
In our previous post, *Bringing Your Maps into Focus*, we covered a three-step process using [GDAL](http://gdal.org). There is a lot going on in the last two steps, internal tiling and overviews, that are hard to research and understand what’s really going on.

We’re going to walk through what is happening using two command line tools; [`exiftool`](https://en.wikipedia.org/wiki/ExifTool) to inspect the GeoTIFF’s internal metadata and GDAL’s [`gdalinfo`](http://www.gdal.org/gdalsrsinfo.html). Installing both is not really a pain. For Mac there are [Homebrew formulas](http://brewformulas.org/) for each. Most Linux distros have packages, and there are [downloads](http://owl.phy.queensu.ca/~phil/exiftool/) for [Windows](https://trac.osgeo.org/osgeo4w/) .

Let’s look at the exiftool output for a non-georeferenced TIFF. It’s really not that interesting. We’re providing here as a starting point.

~~~
$ exiftool Atlanta_1928_Sheet45.tif
ExifTool Version Number         : 10.02
File Name                       : Atlanta_1928_Sheet45.tif
Directory                       : .
File Size                       : 90 MB
File Modification Date/Time     : 2016:05:24 20:59:18-04:00
File Access Date/Time           : 2016:05:24 20:58:53-04:00
File Inode Change Date/Time     : 2016:05:24 20:59:40-04:00
File Permissions                : rw-r--r--
File Type                       : TIFF
File Type Extension             : tif
MIME Type                       : image/tiff
Exif Byte Order                 : Little-endian (Intel, II)
Subfile Type                    : Full-resolution Image
Image Width                     : 5347
Image Height                    : 6348
Bits Per Sample                 : 8 8 8
Compression                     : LZW
Photometric Interpretation      : RGB
Image Description               :
Strip Offsets                   : (Binary data 3521 bytes, use -b option to extract)
Orientation                     : Horizontal (normal)
Samples Per Pixel               : 3
Rows Per Strip                  : 16
Strip Byte Counts               : (Binary data 2778 bytes, use -b option to extract)
X Resolution                    : 300
Y Resolution                    : 300
Planar Configuration            : Chunky
Resolution Unit                 : inches
Software                        : Adobe Photoshop CS5.1 Windows
Modify Date                     : 2012:02:08 17:22:10
Predictor                       : Horizontal differencing
XMP Toolkit                     : Adobe XMP Core 5.0-c061 64.140949, 2010/12/07-10:57:01
Create Date                     : 2010:06:15 15:11:38-04:00
Metadata Date                   : 2012:02:08 17:22:10-05:00
Creator Tool                    : Adobe Photoshop CS3 Windows
Format                          : image/tiff
Description                     :
Color Mode                      : RGB
Instance ID                     : xmp.iid:DDF74EAAA152E111A8D59B79B1F6E115
Document ID                     : uuid:32FACA12C678DF11BE5CBF86910F7516
Original Document ID            : uuid:32FACA12C678DF11BE5CBF86910F7516
History Action                  : saved
History Instance ID             : xmp.iid:DDF74EAAA152E111A8D59B79B1F6E115
History When                    : 2012:02:08 17:22:10-05:00
History Software Agent          : Adobe Photoshop CS5.1 Windows
History Changed                 : /
Marked                          : False
Current IPTC Digest             : 54de4a51baba1e48176bd21acf562b86
Coded Character Set             : UTF8
Application Record Version      : 0
Caption-Abstract                :
IPTC Digest                     : 54de4a51baba1e48176bd21acf562b86
Displayed Units X               : inches
Displayed Units Y               : inches
Global Angle                    : 30
Global Altitude                 : 30
Copyright Flag                  : False
Photoshop Thumbnail             : (Binary data 5790 bytes, use -b option to extract)
Color Space                     : Uncalibrated
Exif Image Width                : 5347
Exif Image Height               : 6348
Image Size                      : 5347x6348
Megapixels                      : 33.9
~~~

If we run it on a GeoTIFF, we see some extra stuff:

~~~
$ exiftool Atlanta_1928_Sheet45.tif
…
Rows Per Strip                  : 64
…
Geo Tiff Version                : 1.1.0
GT Model Type                   : Projected
GT Raster Type                  : Pixel Is Area
GT Citation                     : NAD83 Georgia State Planes, West Zone, US Foot
Geographic Type                 : NAD83
Geog Citation                   : NAD83
Geog Geodetic Datum             : North American Datum 1983
Geog Angular Units              : Angular Degree
Geog Ellipsoid                  : GRS 1980
Geog Semi Major Axis            : 6378137
Geog Inv Flattening             : 298.257222100911
Projected CS Type               : Unknown (2240)
Projection                      : User Defined
Proj Coord Trans                : Transverse Mercator
Proj Linear Units               : Linear Foot US Survey
Proj Nat Origin Long            : -84.1666666666667
Proj Nat Origin Lat             : 30
Proj False Easting              : 699999.999999999
Proj False Northing             : 0
Proj Scale At Nat Origin        : 0.9999
Image Size                      : 5364x6328
Megapixels                      : 33.9
~~~

And let’s look at what `gdalinfo` has to say:

~~~
$ gdalinfo Atlanta_1928_Sheet45.tif
Driver: GTiff/GeoTIFF
Files: Atlanta_1928_Sheet45.tif
Size is 5364, 6328
Coordinate System is:
PROJCS["NAD83 / Georgia West (ftUS)",
    GEOGCS["NAD83",
        DATUM["North_American_Datum_1983",
            SPHEROID["GRS 1980",6378137,298.2572221009113,
                AUTHORITY["EPSG","7019"]],
            AUTHORITY["EPSG","6269"]],
        PRIMEM["Greenwich",0],
        UNIT["degree",0.0174532925199433],
        AUTHORITY["EPSG","4269"]],
    PROJECTION["Transverse_Mercator"],
    PARAMETER["latitude_of_origin",30],
    PARAMETER["central_meridian",-84.1666666666667],
    PARAMETER["scale_factor",0.9999],
    PARAMETER["false_easting",699999.9999999991],
    PARAMETER["false_northing",0],
    UNIT["US survey foot",0.3048006096012192,
        AUTHORITY["EPSG","9003"]],
    AUTHORITY["EPSG","2240"]]
Origin = (2232544.649310299195349,1362031.125677362782881)
Pixel Size = (0.668734999999993,-0.668735000000005)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  ( 2232544.649, 1362031.126) ( 79d 8' 0.51"W, 33d38'30.07"N)
Lower Left  ( 2232544.649, 1357799.371) ( 79d 8' 2.94"W, 33d37'48.36"N)
Upper Right ( 2236131.744, 1362031.126) ( 79d 7'18.24"W, 33d38'28.34"N)
Lower Right ( 2236131.744, 1357799.371) ( 79d 7'20.68"W, 33d37'46.64"N)
Center      ( 2234338.197, 1359915.248) ( 79d 7'40.59"W, 33d38' 8.35"N)
Band 1 Block=5364x64 Type=Byte, ColorInterp=Red
Band 2 Block=5364x64 Type=Byte, ColorInterp=Green
Band 3 Block=5364x64 Type=Byte, ColorInterp=Blue
~~~

### Strips
> TIFF image data can be organized into strips for faster random access and efficient I/O buffering.[^strips]

[^strips]: [http://www.awaresystems.be/imaging/tiff/tifftags/rowsperstrip.html](http://www.awaresystems.be/imaging/tiff/tifftags/rowsperstrip.html)

Looking at the “Rows Per Strip” tag in the `exiftool` output, we can see that, prior to georeferencing, our TIFF had strips that were 16 rows/pixels tall.  Our GeoTIFF’s strips are 64 rows/pixels tall. `gdalinfo` confirms that:
```
Band 1 Block=5364x64 Type=Byte, ColorInterp=Red
```

This will help the speed that our maps are loaded, Rather than loading all 33,943,392 pixels at once, the image will be rendered in 343,296 pixel strips. But what if the only part of the map that is requested is one corner? The client will get the whole strip and a lot of unwanted data. The whole points of using WMS and slippy maps is only serving the tile(s) that are requested. GeoServer can tile the image on the fly, but adding tiles internally to our GeoTIFF will take some load off GeoServer and make everything faster [IS THIS TRUE?]

### Tiles

> For low-resolution to medium-resolution images, the standard TIFF method of breaking the image into strips is adequate. However high-resolution images can be accessed more efficiently—and compression tends to work better—if the image is broken into roughly square tiles instead of horizontally-wide but vertically narrow strips.[^tiles]

[^tiles]: See page 66 of the [TIFF spec document](http://partners.adobe.com/public/developer/en/tiff/TIFF6.pdf)


Above we see with `gdalinfo` that each band’s block spans the whole image and in the `exiftool` output, our GeoTIFF has no internal tiling. The standard tile size used by OpenStreetMap, Google Maps, etc. is 256 x 256. In the previous post, we ran [`gdal_tranlate`](http://www.gdal.org/gdal_translate.html) on our GeoTIFF we generated tiles that are 256 x 256 `-co 'TILED=YES' -co 'BLOCKXSIZE=256'`[^tiffdriver]

[^tiffdriver]: The `-co` stand for “creation option” and these are specific to the file format. All the options for GeoTIFFs, and all sorts of good info, can be found at [http://www.gdal.org/frmt_gtiff.html](http://www.gdal.org/frmt_gtiff.html)

 The full command from our previous post:

```
$ gdal_translate -co 'TILED=YES' -co 'BLOCKXSIZE=256' -co 'BLOCKYSIZE=256' -co 'COMPRESSION=JPEG' /data/tmp/atlanta_1928_sheet45.tif  /data/processed/atlanta_1928_sheet45.tif
```

Running `exiftool` on the translated GeoTIFF shows:

```
Tile Width    : 256
Tile Length   : 256
```

And `gdalinfo` shows:

```
Band 1 Block=256x256 Type=Byte, ColorInterp=Red
Band 2 Block=256x256 Type=Byte, ColorInterp=Green
Band 3 Block=256x256 Type=Byte, ColorInterp=Blue
```


### Overviews aka Subfiles
A TIFF file can contain multiple subfiles[^subfile]. Using `gdaladdo` we can create multiple subfiles of our map at different resolutions. GDAL calls subfiles [overviews](http://www.gdal.org/frmt_gtiff.html#overviews). Let’s look back at our example:

```
gdaladdo --config GDAL_TIFF_OVR_BLOCKSIZE 256 -r average /data/processed/atlanta_1928_sheet45.tif 2 4 8 16 32
```

[^subfile]: See page 16 of the [TIFF spec document](http://partners.adobe.com/public/developer/en/tiff/TIFF6.pdf).

`gdaladdo` tiles the map again and defaults to 128. You can set `GDAL_TIFF_OVR_BLOCKSIZE` as an [environment variable](https://en.wikipedia.org/wiki/Environment_variable), but here we’re just passing it in as an option. The `-r` is choosing the resampling algorithm. We played around with a few and landed on `average`. You might want to try others.

Finally that list of numbers at the end is the amount the image will be reduced: 2 will reduce by half, 4 will reduce by a quarter, and so on.

Let’s look at what happened by looking at the `gdalinfo` output:

~~~
Band 1 Block=256x256 Type=Byte, ColorInterp=Red
  Overviews: 2673x3186, 1337x1593, 669x797, 335x399, 168x200
Band 2 Block=256x256 Type=Byte, ColorInterp=Green
  Overviews: 2673x3186, 1337x1593, 669x797, 335x399, 168x200
Band 3 Block=256x256 Type=Byte, ColorInterp=Blue
  Overviews: 2673x3186, 1337x1593, 669x797, 335x399, 168x200
~~~

Just for fun and to prove those low resolution versions really do exist, let’s use a bunch of exiftool options to inspect them (thanks Kyle Fenton!). The output is rather verbose so we’ll really just show the important stuff.

~~~
$ exiftool -s -a -e -G5 atlanta_1928_sheet45.tif.tif
[ExifTool]      ExifToolVersion                 : 10.02
[System]        FileName                        : atlanta_1928_sheet45.tif.tif
…
[TIFF-IFD0]     ImageWidth                      : 5345
[TIFF-IFD0]     ImageHeight                     : 6371
…
[TIFF-IFD2]     ImageWidth                      : 1337
[TIFF-IFD2]     ImageHeight                     : 1593
…
[TIFF-IFD4]     ImageWidth                      : 335
[TIFF-IFD4]     ImageHeight                     : 399
…
[TIFF-IFD5]     SubfileType                     : Reduced-resolution image
[TIFF-IFD5]     ImageWidth                      : 168
[TIFF-IFD5]     ImageHeight                     : 200
[TIFF-IFD5]     BitsPerSample                   : 8 8 8
[TIFF-IFD5]     Compression                     : Uncompressed
[TIFF-IFD5]     PhotometricInterpretation       : RGB
[TIFF-IFD5]     SamplesPerPixel                 : 3
[TIFF-IFD5]     PlanarConfiguration             : Chunky
[TIFF-IFD5]     TileWidth                       : 256
[TIFF-IFD5]     TileLength                      : 256
[TIFF-IFD5]     TileOffsets                     : 142744474
[TIFF-IFD5]     TileByteCounts                  : 196608
[TIFF-IFD5]     SampleFormat                    : Unsigned; Unsigned; Unsigned
~~~

TODO: admit you can’t explain the dimensional changes.
