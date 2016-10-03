---
layout: post
title: Spatial Search on the Neighborhood Level
tags: [postgis, postgres, rgeo, activerecord, rails, gis, search]
date: 2016-10-31 13:00:00
authors: jayvarner
---

Hi, I'm a mother fucking intro!

```shell
Map Title                       | Neighborhood |   Intersection   | Distance From Center
--------------------------------+--------------+------------------+----------------------
 1970 Census Tracts             | Reynoldstown | 2320678.21132743 |     689.291231272075
 1970 Population Distribution   | Reynoldstown | 2320678.21132743 |     691.621015924325
 Detailed Base Map Atlanta Regi | Reynoldstown | 2320678.21132743 |     767.725406396319
 [Atlanta Area Transportation S | Reynoldstown | 2320678.21132743 |     839.342774197147
 Employment Data Per County for | Reynoldstown | 2320678.21132743 |     842.497025480909
 [Atlanta Area Transportation S | Reynoldstown | 2320678.21132743 |     911.320352356744
 Downtown Redevelopment, Presen | Reynoldstown | 2320678.21132743 |     947.132811168715
 Decatur Parkway: Bike Network  | Reynoldstown | 2320678.21132743 |      947.14764346836
 Proposed Rapid Transit System: | Reynoldstown | 2320678.21132743 |     1055.00442324029
 Building Permits 1923 City of  | Reynoldstown | 2320678.21132743 |     1079.72606412013
 Proposed Coach Routes City of  | Reynoldstown | 2320678.21132743 |     1088.79152428309
 Simplified Base Map, Atlanta R | Reynoldstown | 2320678.21132743 |     1109.30297857508
 Intown South Study Area        | Reynoldstown | 2320678.21132743 |     1126.28578355835
 1970-1976 Regional Highway Imp | Reynoldstown | 2320678.21132743 |      1144.6806907954
 Decatur Parkway: Location Map  | Reynoldstown | 2320678.21132743 |      1178.9646329174
 Present Track Layout City of A | Reynoldstown | 2320678.21132743 |     1199.38245812927
 Present Jitney Routes City of  | Reynoldstown | 2320678.21132743 |     1207.50843366127
```

```shell
substr             |     name     |   intersection   | distance_from_center
--------------------------------+--------------+------------------+----------------------
Atlanta Cadastral Survey, Tax  | Reynoldstown | 325177.867767423 |     172.853711383999
Atlanta Cadastral Survey, Tax  | Reynoldstown | 330992.647158763 |     262.939804784926
Atlanta Cadastral Survey, Tax  | Reynoldstown | 314542.092335886 |     272.774210414324
Atlanta Cadastral Survey, Tax  | Reynoldstown | 320280.111507124 |     367.440877852164
Atlanta Cadastral Survey, Tax  | Reynoldstown | 317549.813578507 |     398.381727374759
Atlanta Cadastral Survey, Tax  | Reynoldstown | 299424.511837158 |     472.045078730046
Atlanta Cadastral Survey, Tax  | Reynoldstown | 269030.487721559 |     490.902091904119
City of Atlanta: Sheet 34      | Reynoldstown | 1157218.28177139 |     505.532462432542
Atlanta Cadastral Survey, Tax  | Reynoldstown | 923790.348954586 |     566.286017369435
Atlanta Cadastral Survey, Tax  | Reynoldstown | 256523.298115837 |      605.77147099246
Decatur Parkway: Plan and Prof | Reynoldstown | 228848.287363262 |     657.149923251237
Right of Way Plan              | Reynoldstown | 184687.182759514 |     676.070126957151
Atlanta Cadastral Survey, Tax  | Reynoldstown | 249739.377788139 |     686.039997624549
1970 Census Tracts             | Reynoldstown | 2320678.21132743 |     689.291231272075
1970 Population Distribution   | Reynoldstown | 2320678.21132743 |     691.621015924325
Atlanta Cadastral Survey, Tax  | Reynoldstown | 274043.516013673 |     716.627496423213
Atlanta Cadastral Survey, Tax  | Reynoldstown | 261523.785076538 |     727.667468082185
City of Atlanta: Sheet 35      | Reynoldstown | 1055688.23514957 |     727.981910571818
Detailed Base Map Atlanta Regi | Reynoldstown | 2320678.21132743 |     767.725406396319
```

```sql
SELECT substr(raster_layers.title, 1, 30), neighborhoods.name, ST_AREA(ST_INTERSECTION(raster_layers.boundingbox, neighborhoods.polygon)) AS intersection,
ST_DISTANCE(ST_Centroid(neighborhoods.polygon), ST_Centroid(raster_layers.boundingbox)) AS distance_from_center
FROM raster_layers INNER JOIN neighborhoods
ON  ST_INTERSECTS(raster_layers.boundingbox, neighborhoods.polygon )
WHERE neighborhoods.id = 37
ORDER BY distance_from_center ASC, intersection DESC;
```
