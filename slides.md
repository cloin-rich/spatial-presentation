---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://images.unsplash.com/photo-1610118091147-37dd42d13a93?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-up
# use UnoCSS
css: unocss
remoteAssets: true
aspectRatio: 16:10
---

# HAND using arcpy

PPUA 7237 Advanced Spatial Analysis  
Colin A. Richardson

---
layout: statement
---

# Make a flood map anywhere in the US with only a shapefile as an input
<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(90deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: iframe
url: https://nu.maps.arcgis.com/apps/instant/basic/index.html?appid=b71fe9bcdce546eca99253d723c9fedb&locale=en-US
---

---
layout: intro
---

# Introduction  
&nbsp;

<v-click>

What is HAND?  

</v-click>
<v-click>

What is arcpy?  

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>


---
layout: default
---

# What is HAND?  
&nbsp;
<v-click>

# <u>H</u>eight <u>A</u>bove <u>N</u>earest <u>D</u>rainage  

</v-click>

&nbsp;
<v-click>

Choose an input digital elevation model (DEM)

</v-click>
<v-click>

<span style="color:#dcbba1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;USGS 3D Elevation Program, FABDEM, etc.</span>

</v-click>
<v-click>

Choose an input stream network

</v-click>
<v-click>

<span style="color:#dcbba1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;USGS National Hydrography Dataset, MERIT-hydro, etc.</span>

</v-click>
<v-click>

Map the stream network onto the raster of the DEM

</v-click>
<v-click>

<span style="color:#dcbba1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Set all DEM values under streams equal to 0</span>

</v-click>
<v-click>

Walk uphill from each stream cell value

</v-click>
<v-click>

<span style="color:#dcbba1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Calculate the height above the nearest stream cell (aka drainage)</span>

</v-click>
<v-click>

Resulting raster is the HAND raster

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: iframe
url: https://nu.maps.arcgis.com/apps/instant/basic/index.html?appid=d52de729e75549fdb05ae69e9350bf43&locale=en-US
---
---
layout: image
image: ./images/hand zoom.jpg
---
---
layout: two-cols
---

# What is arcpy?  

<v-click>

A python package for ArcGIS 

</v-click>
<v-click>

This geoprocessing window: 

</v-click>
<v-click>

<img src = '/images/geoprocess.jpg' style = "margin: left;
width: 50%;
vertical-align: top"/>

</v-click>
<v-click>

<Arrow x1="250" y1="248" x2="75" y2="248" />

</v-click>
<v-click>

<Arrow x1="250" y1="288" x2="75" y2="288" />

</v-click>
<v-click>

<Arrow x1="250" y1="365" x2="55" y2="365" />

</v-click>
<v-click>

<Arrow x1="250" y1="325" x2="85" y2="325" />

</v-click>
::right::

<v-click>

and this code snippet: 

</v-click>
<v-click>

```ts {0|1-3|4|5-6|all}
import arcpy
from arcpy import env
from arcpy.sa import *
env.workspace = "C:/sapyexamples/data"
outExtractByMask = ExtractByMask("elevation", "mask.shp", "INSIDE")
outExtractByMask.save("C:/sapyexamples/output/maskextract")
```

</v-click>
&nbsp;
<v-click>


# produce identical output

</v-click>



<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(90deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: intro
---

# Methods  
&nbsp;

<v-click>

Data sources  

</v-click>
<v-click>

Geoprocessing workflow  

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: default
---
# Identify basins  
<v-click>

Write a function using USGS web map services

</v-click>
<v-click>

```ts {0|1|2|3-4|5-6|12|all}
def whichHUC6(AOI, name, wdir=None, outdir=None):
  AOI_proj = arcpy.Project_management(AOI, odir + "\\" + name + '-AOI_proj.shp', crs)
  ext = str(arcpy.Describe(AOI_proj).extent).split()[0:4]
  ext_ord = ext[0] + ',' + ext[1] + ',' + ext[2] + ',' + ext[3]

  url = 'https://hydro.nationalmap.gov/arcgis/rest/services/wbd/MapServer/3/query?where=&text=&objectIds=&time=&geometry=' + ext_ord + '&geometryType=esriGeometryEnvelope&inSR=4326&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=huc6&returnGeometry=false&returnTrueCurves=false&maxAllowableOffset=&geometryPrecision=&outSR=4326&havingClause=&returnIdsOnly=false&returnCountOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&gdbVersion=&historicMoment=&returnDistinctValues=false&resultOffset=&resultRecordCount=&returnExtentOnly=false&datumTransformation=&parameterValues=&rangeValues=&quantizationParameters=&featureEncoding=esriDefault&f=pjson'
  data_json = json.loads(urlopen(url).read())
  huc6s = []
  for f in data_json['features']:
      huc6 = f['attributes']['huc6']
      huc6s.append(huc6)
  return huc6s
```

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: iframe
url: https://nu.maps.arcgis.com/apps/instant/basic/index.html?appid=8df8906a6d8a4b65a345d201ac57b9b8&locale=en-US
---
---
layout: default
---
# Download HAND data

<v-click>

Get basin HAND .zip files from Oak Ridge National Laboratory

</v-click>
<v-click>

```ts {0|1|3|14|all}
for huc_i in range(len(huc6s)):
        huc = huc6s[huc_i]
        url = 'https://cfim.ornl.gov/data/HAND/20200601/'+huc+'.zip'
        filedir = os.path.join(cwd, huc + '.zip')
        if os.path.isdir(filedir.split('.')[0]):
            print(filedir.split('.')[0], ' already exists!\n')
            continue
        if os.path.exists(filedir):
            print(filedir,' already exists!\n')
            continue
        else:
            print('\n############################################################\n\n'+'('+str(huc_i+1)+' of '+str(len(huc6s))+') '+'Retrieving data from...'+
                url+'\n\n############################################################')
        save(url, filedir)
```

</v-click>


<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: iframe
url: https://cfim.ornl.gov/data/vis/v0.2.0/
---

---
layout: default
---
# Combine basin files  
<v-click>

Merge and clip raster and vector data from ORNL:

</v-click>

<v-click>

```ts {0|1-4|5-6|7-8|9-10|11|all}    
arcpy.MosaicToNewRaster_management(hand_paths, cwd, name + '_hand_merge.tif',coordinate_system_for_the_raster=4326,number_of_bands=1,pixel_type= "32_BIT_FLOAT")
arcpy.MosaicToNewRaster_management(mask_paths, cwd, name + '_catchmask_merge.tif',coordinate_system_for_the_raster=4326,number_of_bands=1, pixel_type="32_BIT_SIGNED")
arcpy.MosaicToNewRaster_management(dem_paths, cwd, name + '_dem_merge.tif', coordinate_system_for_the_raster=4326,number_of_bands=1, pixel_type='32_BIT_FLOAT')
arcpy.Merge_management(fline_paths, cwd + r'\flowlines_merged.shp')
hand_clip = arcpy.sa.ExtractByMask(cwd+"\\"+name+r'_hand_merge.tif',odir + "\\" + name + '-AOI_proj.shp')
hand_clip.save(odir+"\\"+name+r'_hand.tif')
catchmask_clip = arcpy.sa.ExtractByMask(cwd+"\\"+name+r'_catchmask_merge.tif',odir + "\\" + name + '-AOI_proj.shp')
catchmask_clip.save(odir+"\\"+name+r'_catchmask.tif')
dem_clip = arcpy.sa.ExtractByMask(cwd+"\\"+name+r'_dem_merge.tif', odir + "\\" + name + '-AOI_proj.shp')
dem_clip.save(odir+"\\"+name+r'_dem.tif')
arcpy.Clip_analysis(cwd + r'\flowlines_merged.shp',odir + "\\" + name + '-AOI_proj.shp',odir + "\\"+ name + '-flowlines.shp')
```

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: image
image: ./images/flowlines.jpg
---
---
layout: default
---
# National Water Model  
<v-click>

Provides nationwide discharge from 1979-present & 30 days into the future:

</v-click>

<v-click>

```ts
def getNWMData(dates, name, wdir=None, outdir=None, nco_pth=None, rm_temp=True):
  if len(dates) > 1:
          strt_date = dates[0]
          end_date = dates[1]
          date_range = pandas.date_range(strt_date, end_date)
          for date in date_range:
              year = str(date.year)
              month = str(date.strftime('%m'))
              day = str(date.strftime('%d'))
              date_text = year + month + day + '1200' 
              url = "https://noaa-nwm-retrospective-2-1-pds.s3.amazonaws.com/model_output/" + year + "/" + date_text + ".CHRTOUT_DOMAIN1.comp"
              filedir = os.path.join(cwd,'flows_' + date_text + '.nc')
              print('############################################################\n\nRetrieving data from...\n'+
                url+'\nto...\n'+filedir+'\n\n############################################################')
              urlretrieve(url, filedir)
```
</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: iframe
url: https://nu.maps.arcgis.com/apps/instant/basic/index.html?appid=60e36abd2cfd4beb99b901b5ff9fd120&locale=en-US
---
---
layout: default
---
# Catchment hydraulic properties 
<v-click>

Use Manning's equation to calculate hydraulic properties of each catchment

</v-click>
<v-click>

<img src = '/images/mannings.jpg' style = "margin: left;
width: 100%;
vertical-align: top">

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: default
---
# Calculate flood depth  
&nbsp; 
<v-click>

Use raster calculator to subtract the water depth from the HAND layer:

</v-click>
<v-click>

```ts
depth = arcpy.sa.RasterCalculator(ras_list,ras_names,'Con(hand - stage > 0, 0, stage-hand)',extent_type = 'FirstOf')
depth = arcpy.sa.ExtractByMask(depth,odir + "\\" + name + '-AOI_proj.shp')
depth.save(odir + "\\" + name + '-depth_map.tif')
```

</v-click>

<v-click>

Convert the raster flood depth to a vector flood bound file:

</v-click>

<v-click>

```ts
bound = arcpy.sa.RasterCalculator([depth], ['depth'], 'depth > 0')
bound = arcpy.sa.Reclassify(bound, "Value", arcpy.sa.RemapValue([[1,1], [0,'NODATA']]))
bound = arcpy.RasterToPolygon_conversion(bound, odir + "\\" + name + '-flood_bound.shp', "NO_SIMPLIFY")
```

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: image
image: ./images/flood bound.jpg
---
---
layout: image
image: ./images/flood depth.jpg
---
---
layout: intro
---

# Next steps  
&nbsp;

<v-click>

Finalize and fully test all of the code 

</v-click>
<v-click>

Publish the code as a publicly available python package  

</v-click>


<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: statement
---

# Make a flood map anywhere with only a shapefile as an input

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(90deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: statement
---

# Accomplished!

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(90deg, #623212 10%, #dcbba1 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
layout: iframe-right
url: https://nu.maps.arcgis.com/apps/instant/basic/index.html?appid=d52de729e75549fdb05ae69e9350bf43
---