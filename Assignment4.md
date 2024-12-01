# Layers input
1. Distribution of land: parcelas (to look at the areas and perimeters of agricultural fields and assess the scale). 
  [Source](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLAYGDTSLCPAR2021)
  * Important Categorises: TA - arable land, PA - pasture with trees, PR - shrubs, PS - pasture, FO - forest.
  * Layer 1 name: SEGOVIA - 40_RCFE
    * Layer type: Shapefile
    * Scale: Segovia province
  * Layer 2 name: PALENCIA - 34_RCFE
2. Crop types: To specify the main crops in different parcels
  [Source](https://mcsncyl.itacyl.es/es/descarga)
  * Layer 1 name: croptype
    * Layer type: GEOTiff
    * Scale: Castilla y Leon (2023)
  * Layer 2 name: Codificacion de la capa raster
    * Layer type: CSV
    * Scale: Castilla y Leon
3. Castilla y Leon regions
  [Source]()

# Processing data
1. Covert raster layer "croptype" into vector layer. Rename: "newcroptype"
2. Join attributes (Land_crop) from the CSV file to Layer "newcroptype". Rename: "newcroptypepart2" (After finishing i realised i could have created a CSV file in excel with pivot table and use spatial join)
3. Change the projection of "newcroptypepart2" from CRS 4326 to CRS 25830. 
4. Clip the layer "newcroptypepart2" by Palencia and Segovia Region, using the extent from features in "prov_cyl_recintos" layer. New layers' names: "palencia_crops" and "segovia_crops"
```
 processing.run("native:clip", {'INPUT':'C:\\Users\\localuser\\Documents\\GIS data\\newcroptypespart2.gpkg|layername=newcroptypespart2','OVERLAY':QgsProcessingFeatureSourceDefinition('C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos', selectedFeaturesOnly=True, featureLimit=-1, geometryCheck=QgsFeatureRequest.GeometryAbortOnInvalid),'OUTPUT':'C:/Users/localuser/Documents/GIS data/palencia_crops.gpkg'})

```

```
processing.run("native:clip", {'INPUT':'C:\\Users\\localuser\\Documents\\GIS data\\newcroptypespart2.gpkg|layername=newcroptypespart2','OVERLAY':'C:/Users/localuser/Documents/GIS data/sg_province.gpkg|layername=prov_cyl_recintos','OUTPUT':'C:/Users/localuser/Documents/GIS data/segovia_crops.gpkg'})
```

6. Join the farm sizes data from layer PALENCIA - 34_RCFE and SEGOVIA - 40_RCFE with the "palencia_crops" and "segovia_crops" in that order. 
7. Export data - Create graph
8. Export map

# Spatial analysis
## Scale assessment
1. Hello
2. Yellow
## Visual deliverables

![image](https://github.com/user-attachments/assets/eea12b22-32f5-477d-aa05-5ceab4fb4b43)
