---
layout: post
title: Bringing Your Maps into Focus
categories: [gdal, geospatial, gis, wms, webmapping]
author: jayvarner
---
## The Problem
Here at Emory, we have some pretty cool old [maps of Atlanta](http://www.digitalgallery.emory.edu/luna/servlet/view/all/where/Atlanta?sort=title%2Cpage_no_%2Ccity%2Cdate) that we wanted to share in a more interactive way. But when we tried, the maps looked like looked this:

<a class="jsbin-embed" href="http://jsbin.com/howitu/1/embed?output">JS Bin on jsbin.com</a>
<script src="http://static.jsbin.com/js/embed.min.js?3.35.12"></script>

After a good bit of research, lots of trial and error, and working with our colleagues at Georgia State University and the University of North Carolina, we came up with a process that gave us this:

<a class="jsbin-embed" href="http://jsbin.com/gekulo/2/embed?output">JS Bin on jsbin.com</a>
<script src="http://static.jsbin.com/js/embed.min.js?3.35.12"></script>


## What We Did
We [georeferenced](https://en.wikipedia.org/wiki/Georeference) our map in the appropriate [coordinate system](https://en.wikipedia.org/wiki/Coordinate_system) for the map’s area. In our case [EPSG:2240](http://spatialreference.org/ref/epsg/2240/). We’ll refer to specific coordinate systems with their [EPSG code](https://en.wikipedia.org/wiki/International_Association_of_Oil_%26_Gas_Producers#European_Petroleum_Survey_Group).

Next we took the resulting [GeoTIFF](https://en.wikipedia.org/wiki/GeoTIFF) and uploaded it to our [GeoServer](http://geoserver.org) so the map could be shared via [WMS](https://en.wikipedia.org/wiki/Web_Map_Service).

Finally, like the examples above, we used [Leaflet](http://leafletjs.com/) to display the maps in the browser overlaid on a modern street map.

## What We Did *Wrong*
Our first problem was that the GeoTIFF is a high resolution image. We were squeezing a 5,931 x  5,779 pixel image into a 326 x 276 pixel box. There's just too much data. Notice if you zoom all the way in to our original example, the map looks great.

<a class="jsbin-embed" href="http://jsbin.com/jinure/embed?output">JS Bin on jsbin.com</a>
<script src="http://static.jsbin.com/js/embed.min.js?3.35.12"></script>

Our other problem was that we used a coordinate system that is inappropriate for web display. Tiled base maps that are provided by organizations like [OpenSteetMap](http://www.openstreetmap.org/about) are based on [EPSG:3857](http://spatialreference.org/ref/sr-org/7483/), aka [WGS 84  Web Mercator](https://en.wikipedia.org/wiki/Web_Mercator#EPSG:3857).

## GDAL Can Solve those Problems
"[GDAL](http://www.gdal.org/) is a translator library for raster and vector geospatial data formats..." This is our three step process.

### Reproject
When you georeference a scanned map, you select a [projection](https://en.wikipedia.org/wiki/Map_projection). For most GIS needs, you will want to use the projection appropriate for the chunk of Earth your map covers. However, for displaying maps on the web, you want to use EPSG:3857[^whcihcode]. To reproject a GeoTIFF you can use the `gdalwarp` command:

[^whcihcode]:There is confusion between EPSG:3857 and EPSG:4326. For displaying raster data using something like Leaflet, OpenLayers, etc. 3857 will result in a much clearer image. Here are two links that helped us understand. [http://gis.stackexchange.com/a/48952](http://gis.stackexchange.com/a/48952) and [http://www.faqoverflow.com/gis/48949.html](http://www.faqoverflow.com/gis/48949.html)


#### Syntax
{% highlight ruby %}
$ gdalwarp -s_srs <source ESPG> -t_srs <target EPSG> -r average </path/to/source/geo.tif> </path/to/new/geo.tif>
{% endhighlight %}

#### Example
```
$ gdalwarp -s_srs EPSG:2240 -t_srs EPSG:3857 -r average /data/atlanta_1928_sheet45.tif /data/tmp/atlanta_1928_sheet45.tif
```

Now you have a new GeoTIFF projected in Web Mercator and preserved your original.

### Tile and Compress
GeoTIFFs are organized in 1 pixel strips by default. OpenStreetMap tiles are 256x256. We want to match OpenStreetMap. GeoTIFFs can be huge and it's not very nice to make users load large files when they don't have to. GDAL can also compress our GeoTIFFs. There are many compression algorithms, but we’re pretty happy with the results we’re getting with `JPEG`.

### Syntax
```
$ gdal_translate -co 'TILED=YES/NO' -co 'BLOCKXSIZE=XXX' -co 'BLOCKYSIZE=XXX' -co 'COMPRESS=XYZ' </path/to/tmp.tif> </path/to/new.tif>
```

### Example
```
$ gdal_translate -co 'TILED=YES' -co 'BLOCKXSIZE=256' -co 'BLOCKYSIZE=256' -co 'COMPRESS=JPEG' /data/tmp/atlanta_1928_sheet45.tif  /data/processed/atlanta_1928_sheet45.tif
```

### Add Overviews
Finally we add overviews. Remember when we said the main reason the map looked so bad was because there was just too much data? Well, overviews fix that. What happens here is we create internal, low resolution versions of our map. We will explain this in more depth in our follow up post.

### Syntax
```
gdaladdo --config GDAL_TIFF_OVR_BLOCKSIZE XXX -r average </path/to/new.tif> levels
```

### Example
```
gdaladdo --config GDAL_TIFF_OVR_BLOCKSIZE 256 -r average /data/processed/atlanta_1928_sheet45.tif 2 4 8 16 32
```

## Conclusion
…..
