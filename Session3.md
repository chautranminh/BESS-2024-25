# Obtaining landcover of fieldtrip areas
* [Source of the data](https://github.com/gretacv/Spatial_analysis_BESS/blob/main/fielwork_locations.geojson)
## Reprojecting
```
processing.run("native:reprojectlayer", {'INPUT':'C:\\Users\\nguye\\Downloads\\fielwork_locations.geojson','TARGET_CRS':QgsCoordinateReferenceSystem('EPSG:25830'),'CONVERT_CURVED_GEOMETRIES':False,'OPERATION':'+proj=pipeline +step +proj=unitconvert +xy_in=deg +xy_out=rad +step +proj=utm +zone=30 +ellps=GRS80','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/fieldworklocations_25830.gpkg'})
```
