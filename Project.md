### Step 1: Add Layers

### Step 2: GeoProcessing

### Step 3: Calculate the Area of each Field
1. **Open Field Calculator:**
   - Right-click on your vector layer (the one containing the agricultural fields) in the **Layers panel**.
   - Select **Open Attribute Table**.
   - Click the **Field Calculator** icon (a pencil).
   - Use the formula below. Unit **hectares** (ha):  
  ```
  $area / 10000
  ```
```
 processing.run("native:joinattributesbylocation", {'INPUT':'C:\\Users\\localuser\\Documents\\GIS data\\VillalarDLC_croptypes.gpkg|layername=VDLC_croptypes','PREDICATE':[0],'JOIN':'/vsizip/C:\\Users\\localuser\\Documents\\GIS data\\47211_Villalar-de-los-Comuneros.zip/47211_RECFE.shp|layername=47211_RECFE','JOIN_FIELDS':['SUPERFICIE','PERIMETRO','PARCELA','USO_SIGPAC','Shape_Leng','Shape_Area'],'METHOD':0,'DISCARD_NONMATCHING':False,'PREFIX':'','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/VDLC_Parcels_info.gpkg\' table="VLDC_Parcels_info2" (geom)'})
```

Spatial join
```

 processing.run("native:joinattributesbylocation", {'INPUT':'C:/Users/localuser/Documents/GIS data/VillalarDLC_croptypes.gpkg|layername=VDLC_croptypes','PREDICATE':[0],'JOIN':'/vsizip/C:/Users/localuser/Documents/GIS data/47211_Villalar-de-los-Comuneros.zip/47211_RECFE.shp|layername=47211_RECFE','JOIN_FIELDS':['SUPERFICIE','PERIMETRO','PARCELA','USO_SIGPAC'],'METHOD':0,'DISCARD_NONMATCHING':False,'PREFIX':'','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/VDLC_ParcelsInfo1.gpkg\' table="ParcelsInfoVLDC" (geom)'})
```
### Step 4: Categorise data

### Step 6: Export Data

### Step 7: Process data in Excel
* Perform data cleaning
* 
