# Layers input
1. Distribution of land: parcelas (to look at the areas and perimeters of agricultural fields and assess the scale). 
  [Source](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLAYGDTSLCPAR2021)
  * Important Categorises: TA - arable land, PA - pasture with trees, PR - shrubs, PS - pasture, FO - forest.
  * Layer 1 name: SEGOVIA - 40_RCFE
    * Layer type: Shapefile
    * Scale: Segovia province
2. Crop types: To specify the main crops in different parcels
  [Source](https://mcsncyl.itacyl.es/es/descarga)
  * Layer 1 name: croptype
    * Layer type: GEOTiff
    * Scale: Castilla y Leon (2023)
  * Layer 2 name: Codificacion de la capa raster
    * Layer type: CSV
    * Scale: Castilla y Leon
3. Livestock trails
  [Source](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLMNADTSAMVPE)
  * Layer name: Livestock routes

# Processing data
1. Covert raster layer "croptype" into vector layer. Rename: "newcroptype"
2. Join attributes (Land_crop) from the CSV file to Layer "newcroptype". Rename: "newcroptypepart2"
3. Reproject layer "newcroptypepart2" from CRS 4326 to CRS 25830. Rename: "croptype_cyl"
4. Join attributes
5. Export data - Create graph
6. Export map

# Spatial analysis
## Scale assessment
1. Hello
2. Yellow
## Visual deliverables

![image](https://github.com/user-attachments/assets/eea12b22-32f5-477d-aa05-5ceab4fb4b43)
