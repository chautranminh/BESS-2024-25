## Set the CRS for the project in QGIS
* ESPG: 4326
## Add layers below:
* [Livestocks load index](https://idecyl.jcyl.es/geonetwork/srv/eng/catalog.search#/metadata/SPAGOBCYLCITDTSEFICG). Layer name in QGIS: "LivestockLoadIndex_NitrogenContamination". Layer type: Vector layer
* [municipalities in Castilla y Leon](https://idecyl.jcyl.es/geonetwork/srv/eng/catalog.search#/metadata/SPAGOBCYLCITDTSAULPR): Layer name in QGIS: "castilla_y_leon" . Layer type: Shape file
* [irrigable area](https://idecyl.jcyl.es/geonetwork/srv/eng/catalog.search#/metadata/SPAGOBCYLAYGDTSLCRZR). Layer name in QGIS: "irrigable zones". Layer type: Shape file
* and files from Assignment1.md including "region_of_critical_species" layer, "topillo_distribution" CSV file.
* Insert layers from Wikiloc app which records the tracks of the field trip and the observations. Layer names: "observation_field1" and "Campo de San Pedro - Aldeanueva del Campanario — tracks". Layers type: Vector file
## Import CSV data to vector layer - topillo_distribution 
* Add the CSV file by going to Layer --> Add Layer --> Add delimited text layer
* File format: Custom delimiters: Tab
* Configure the coordinate. X field = decimalLongtitude. Y field = decimalLatitude

## Cropping every information layer to extent of Segovia

1. Create the Segovia outline layer (name: segovia_outline)
  ```
   processing.run("native:clip", {'INPUT':QgsProcessingFeatureSourceDefinition('/vsizip/C:/Users/nguye/OneDrive - IE     University/Escritorio/IE/2ndYear/GIS/challenge_topillo/prov_cyl_recintos.zip/prov_cyl_recintos.shp|layername=prov_cyl_recintos', selectedFeaturesOnly=True, featureLimit=-1,     geometryCheck=QgsFeatureRequest.GeometryAbortOnInvalid),'OVERLAY':'/vsizip/C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/prov_cyl_recintos.zip/prov_cyl_recintos.shp|layername=prov_cyl_recintos','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/segovia_outline.shp'})
  ```
2. Crop the info layers
* Layer 1: irrigable zones --> irrigable_zones_sg
  ```
  processing.run("native:clip", {'INPUT':'/vsizip/C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/rega_cyl_zonas_regables_shp.zip/rega_cyl_zonas_regables.shp|layername=rega_cyl_zonas_regables','OVERLAY':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/segovia_outline.shp','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/irrigable_zones_sg.shp'})
  ```
* Layer 2: region_of_critical_species
  ```
  processing.run("native:clip", {'INPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/protected_species/ps.especies_cyl_areas_criticas.shp','OVERLAY':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/segovia_outline.shp','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/critical_species_sg.shp'})
  ```
* Layer 3: LivestockLoadIndex_NitrogenContamination
This is a raster layer which is imported into QGIS by adding URL in WMS. But clipping raster does not support this. So I exported the layer to a local GEOTIFF (.tif) file and put in again into QGIS. After that i used raster clipping by mask layer.
   ```
  processing.run("gdal:cliprasterbymasklayer", {'INPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/LivestockNitrogenContamination.tif','MASK':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/segovia_outline.shp','SOURCE_CRS':None,'TARGET_CRS':None,'TARGET_EXTENT':None,'NODATA':None,'ALPHA_BAND':False,'CROP_TO_CUTLINE':True,'KEEP_RESOLUTION':False,'SET_RESOLUTION':False,'X_RESOLUTION':None,'Y_RESOLUTION':None,'MULTITHREADING':False,'OPTIONS':'','DATA_TYPE':0,'EXTRA':'','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/LivestockNitrogenContamination_sg.tif'})
  ```
* Layer 4: topillo_distribution
  ```
  processing.run("native:clip", {'INPUT':'delimitedtext://file:///C:/Users/nguye/OneDrive%20-%20IE%20University/Escritorio/IE/2ndYear/GIS/challenge_topillo/vole_distribution/0033613-240906103802322.csv?type=csv&delimiter=%5Ct&maxFields=10000&detectTypes=yes&xField=decimalLongitude&yField=decimalLatitude&crs=EPSG:4326&spatialIndex=no&subsetIndex=no&watchFile=no&field=gbifID:text','OVERLAY':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/segovia_outline.shp','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/topillo_distribution_sg.shp'})
  ```
