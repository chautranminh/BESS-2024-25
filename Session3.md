# Obtaining landcover of fieldtrip areas
* [Source of the data](https://github.com/gretacv/Spatial_analysis_BESS/blob/main/fielwork_locations.geojson)
## Reprojecting
```
processing.run("native:reprojectlayer", {'INPUT':'C:\\Users\\nguye\\Downloads\\fielwork_locations.geojson','TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'CONVERT_CURVED_GEOMETRIES':False,'OPERATION':'+proj=pipeline +step +proj=unitconvert +xy_in=deg +xy_out=rad +step +proj=utm +zone=30 +ellps=GRS80','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/fieldworklocations_25830.gpkg'})
```

## Buffering
```
 processing.run("native:buffer", {'INPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/fieldworklocations_25830.gpkg','DISTANCE':3000,'SEGMENTS':15,'END_CAP_STYLE':0,'JOIN_STYLE':0,'MITER_LIMIT':2,'DISSOLVE':False,'SEPARATE_DISJOINT':False,'OUTPUT':'TEMPORARY_OUTPUT'})
```

## Download land cover data
* [Source](https://livingatlas.arcgis.com/landcoverexplorer/#mapCenter=-3.28600%2C31.34000%2C3&mode=step&timeExtent=2017%2C2021&year=2022&downloadMode=true)

## Zonal histogram
```
 processing.run("native:zonalhistogram", {'INPUT_RASTER':'C:/Users/nguye/Downloads/30T_20230101-20240101.tif','RASTER_BAND':1,'INPUT_VECTOR':'C:\\Users\\nguye\\OneDrive - IE University\\Escritorio\\IE\\2ndYear\\GIS\\challenge_topillo\\fieldlocations_3km.gpkg|layername=fieldlocation_3km','COLUMN_PREFIX':'LC_','OUTPUT':'ogr:dbname=\'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/lccc.gpkg\' table="lc" (geom)'})
```

picture
