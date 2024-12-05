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

6. Join the farm sizes data from layer PALENCIA - 34_RCFE and SEGOVIA - 40_RCFE with the "palencia_crops" and "segovia_crops" in that order. New layers name: "palencia_cropsdata" and "segovia_cropsdata"
7. Export the 2 layers just created to CSV file and upload into excel. Perform data cleaning in Excel
   * In brief: there are 98221 parcels in Palencia.
   * Create Pivot table
8. Create bar graph and pie chart
    Graph 1:
   ![KdfAi-land-cover-in-palencia-by-perimeter-and-area](https://github.com/user-attachments/assets/d78b14fb-755b-408e-a05c-7e85c38d695a)
    Graph 2:
   ![KdfAi-land-cover-in-palencia-by-perimeter-and-area (2)](https://github.com/user-attachments/assets/bfd3e4fd-53d1-4041-b325-30775ca1edd0)
    Graph 3:
   ![NlDng-scale-of-parcels-in-palencia-agriculture-farms](https://github.com/user-attachments/assets/7da86961-f685-4d29-9b53-26f6f830987d)

10. Export map

![MapAssignment3](https://github.com/user-attachments/assets/962f87d1-0d53-4125-bd9d-48691c360128)


# Spatial analysis
## Scale assessment

- These are my goals for the deliverable:
  - Percentage of small-scale, large-scale, medium-scale farm
  - The percentage of different crop types in each farm scale type
  - Percentage of farms that are of close distance to voles’ occurrences.
 
