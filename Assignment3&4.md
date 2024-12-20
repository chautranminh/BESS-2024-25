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
4. Livestocks route in Castilla y Leon
  * Layer name: Livestock_routes
  * Layer type: Geopackage
  * Scale: Castilla y Leon

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
7. Clip the "Livestock_routes" layer to Palencia region. New layer name: "LivestockRoute_Palencia"
```
 processing.run("native:clip", {'INPUT':'C:\\Users\\localuser\\Documents\\GIS data\\livestock_routes.gpkg|layername=livestocks_routes','OVERLAY':QgsProcessingFeatureSourceDefinition('C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos', selectedFeaturesOnly=True, featureLimit=-1, geometryCheck=QgsFeatureRequest.GeometryAbortOnInvalid),'OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/livestock_routes_palencia.gpkg\' table="LivestockRoute_Palencia" (geom)'})
```

8. Export the 2 layers just created to CSV file and upload into excel. Perform general data cleaning in Excel, so that the table now has 4 columns: Parcel, Land_cover, Perimeter, Surface_area. 
   * In brief: there are 98221 parcels in Palencia.
9. Create bar graph and pie chart
10. Export map

![MapAssignment3ver2](https://github.com/user-attachments/assets/c23c8924-debd-4850-a6f9-5bbe1efae50d)



# Spatial analysis
## 1. Scale assessment
### Data cleaning
* From the table of data exported from QGIS, filter out the data in the "Land_cover" values that do not belong to agricultural purposes, which are: .
* Copy the data into a new sheet (Sheet 2). Create a pivot table with this new table (Sheet2PivotTable), placing the "Parcel" as column, the "Max of surface area" as value. In a new column named "Scale", use the funtion: =IF(A2<50000, "SMALL", "LARGE") to categorise parcels with surface area < 50000 meter square as "Small", and the other as "Large"
* Copy this data into a new sheet (Sheet 3).
* From sheet 3, create a Pivot Table inside sheet 3, putting "Scale" as column and "Sum of Max of surface area" as value. Put the result onto Data Wrapper, we get Graph 1.
* Use XLStats to do visualise data using the step follow. Click on Visualising Data --> Univariate plots --> Choose the Max of surface area column as quantitative data and the "Scale" column as qualitative data (for the first time) and then as Subsample. We get graph 2 and 3
### Deliverables
**Graph 1:**

  ![NlDng-scale-of-parcels-in-palencia-agriculture-farms](https://github.com/user-attachments/assets/7da86961-f685-4d29-9b53-26f6f830987d)
**Graph 2:**

  ![image](https://github.com/user-attachments/assets/6c77d99f-dc45-4880-ba3a-18ca17cbf6b7)
**Graph 3:**

![image](https://github.com/user-attachments/assets/0beafc3f-9d93-478c-84a1-d43cc30eb8d8)
### Relevance to the challenge
* By creating diagrams showing the scale of farm fields in Palencia, we obtain an overview of the farm scale in Palencia, from there, we can tailor our solution to such scales. In the box plot in Graph 3, there seems to be an outlier in our data or miscalculation in the data processing, which needs to be reexamined. 


## 2. Land cover
### Data cleaning
* From the original data exported from QGIS, use Pivot Table in a new sheet "Sheet 4" and put "Land cover" as Column and "Perimeter" and "Surface area" as Value (Choose Sum of value option in Value setting)
- Put the results onto Data Wrapper and customised, we got Graph 4 and 5:
### Deliverable
**Graph 4:**
    
   ![KdfAi-land-cover-in-palencia-by-perimeter-and-area](https://github.com/user-attachments/assets/d78b14fb-755b-408e-a05c-7e85c38d695a)

**Graph 5:**
    
   ![KdfAi-land-cover-in-palencia-by-perimeter-and-area (2)](https://github.com/user-attachments/assets/bfd3e4fd-53d1-4041-b325-30775ca1edd0)
### Relevance to the challenge
* The graphs give an overview of the share of different vegetations in the Palencia topography. It is to our surprise that the share of alfalfa and rapeseed crops are very low, but monoculture crops such as wheat and barley sunflower is relatively significant. Considering that Palencia is the city with the most significant damage from vole pest, we suggest that perhaps it is not because of the large surface area of the afalfa or rapeseed crops, but the fact that many crops, including but not limited to afalfa and rapeseeds, are grown in the monoculture practice, with low frequencies of natural predators among the area. What is also worth noticing is the low share of pasture fields, though we are not sure about the landscape of these pastures. This can be a challenge if we want to, in the long run, introduce traditional grazing practises such as transhumance or pastoralism.  

# Question 
We found this [article online](https://scijournals.onlinelibrary.wiley.com/doi/full/10.1002/ps.8344) and it does not seem like it can be georeferenced just by adding points like we did in class but it needs to have buffer zone. But i'm not sure how to locate the center especially when there are so many zones overlapping each other, is there any precise way to do it or do i just estimate the center and create the buffer zone accordingly?
