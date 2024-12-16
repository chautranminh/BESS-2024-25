# Step 1: Add Layers
1. Layer 1: [Castilla y Leon municipalities](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLCITDTSAULPR)
   * Layer type: geopackage
   * Layer name: prov_cyl_recintos
   * CRS: http://www.opengis.net/def/crs/EPSG/0/25830
   * Scale: 50000
   * License: [CC BY 4.0 ign.es](https://www.scne.es/productos.html#MDT)
3. Layer 2: [Parcels in Villalar de Los Comuneros](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLAYGDTSLCPAR2021)
   * Layer type: Shapefile
   * Layer name: 47211_Villalar-de-los-Comuneros — 47211_RECFE.shp
   * CRS: EPSG:4258 (ETRS89 geographic coordinates). EPSG:4326 (WGS84 geographic coordinates). EPSG:3857 (WGS84 Web Mercator plane coordinates). EPSG:25829 (ETRS89 UTM zone 29). EPSG:25830 (ETRS89 UTM zone 30)
   * Scale: 500
   * License: Not legally valid, for informational purposes. Free and open use. Obligatory reference to the ownership of the source is required: "Junta de Castilla y León". Detailed conditions at (https://datosabiertos.jcyl.es/web/jcyl/RISP/es/Plantilla100/1284235967637/_/_/_)
4. Layer 3: [Land cover distribution in Castilla y Leon](https://mcsncyl.itacyl.es/es/descarga)
   * Layer type: Raster
   * Layer name: Cultivos y Ocupación del suelo 2023
   * CRS: CRS:84. EPSG:4326. EPSG:25830
   * Scale: 2 meters
5. Layer 4: [Livestock routes in Castilla y Leon](https://idecyl.jcyl.es/geonetwork/srv/spa/catalog.search#/metadata/SPAGOBCYLMNADTSAMVPE)
   * Layer type: geopackage
   * Layer name: Livestock_routes
   * CRS: EPSG:4258 (ETRS89 coordenadas geográficas). EPSG:4326 (WGS84 coordenadas geográficas). EPSG:3857 (WGS84 Web Mercator coordenadas planas). EPSG:25829 (ETRS89 UTM huso 29). EPSG:25830 (ETRS89 UTM huso 30)
   * Scale: 5000
# Step 2: GeoProcessing
## Clipping of Valladolid
```
 processing.run("native:clip", {'INPUT':'C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos','OVERLAY':QgsProcessingFeatureSourceDefinition('C:/Users/localuser/Documents/GIS data/prov_cyl_recintos.gpkg|layername=prov_cyl_recintos', selectedFeaturesOnly=True, featureLimit=-1, geometryCheck=QgsFeatureRequest.GeometryAbortOnInvalid),'OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/Villalar de los comuneros.gpkg\' table="VillalarDeLosComuneros" (geom)'})
```
* New layer name: VillalarDeLosComuneros
## Convert Raster layer to Vector layer
**Purpose: to attain attribute table**
```
 processing.run("gdal:polygonize", {'INPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/challenge_topillo/croptypes.tif','BAND':1,'FIELD':'DN','EIGHT_CONNECTEDNESS':False,'EXTRA':'','OUTPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/wtfisgoingon/croptype_vector.gpkg'})
```
* New layer name: newcroptypespart2.gpkg
## Clipping of Land cover distribution in Villalar de Los Comuneros
```
 processing.run("native:clip", {'INPUT':'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/wtfisgoingon/newcroptypespart2.gpkg','OVERLAY':'/vsizip/C:\\Users\\nguye\\Downloads\\47211_Villalar-de-los-Comuneros.zip/47211_RECFE.shp|layername=47211_RECFE','OUTPUT':'ogr:dbname=\'C:/Users/nguye/OneDrive - IE University/Escritorio/IE/2ndYear/GIS/wtfisgoingon/VillalarDLC_croptypes.gpkg\' table="VDLC_croptypes" (geom)'})
```
* New layer name: VDLC_croptypes
## Clipping of Livestock routes in Villalar de Los Comuneros
```
 processing.run("native:clip", {'INPUT':'C:/Users/localuser/Documents/GIS data/livestock_routes.gpkg|layername=livestocks_routes','OVERLAY':'C:/Users/localuser/Documents/GIS data/VDLC_Parcelsinfo.gpkg|layername=VDLC_parcelsinfo','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/livestock_routes_VDLC.gpkg\' table="livestock_routes_vdlc" (geom)'})
```
* New layer name: livestock_routes_vdlc
# Step 3: Categorise legends in the layer "VDLC_croptypes" and export the map
![MapFinal3](https://github.com/user-attachments/assets/bbbcead7-811c-4c85-ac00-acefcb3e2051)

# Step 4: Calculate the Area of each Field in the "VDLC_croptypes"
**Open Field Calculator:**
   - Right-click on your vector layer (the one containing the agricultural fields) in the **Layers panel**.
   - Select **Open Attribute Table**.
   - Click the **Field Calculator** icon (a pencil).
   - Use the formula below. Unit **hectares** (ha):  
  ```
  format_number($area / 10000, 4)
  ```
   - Save as a new field. Note: Choose the setting "Real number". Field name: Area
**Join the layer "VDLC_croptypes" and "47211_Villalar-de-los-Comuneros — 47211_RECFE.shp" to create a comprehensive database including the parcel code, land cover, perimeter and surface area in reality, and calculated surface area through QGIS.
```
 processing.run("native:joinattributesbylocation", {'INPUT':'C:/Users/localuser/Documents/GIS data/VillalarDLC_croptypes.gpkg|layername=VDLC_croptypes','PREDICATE':[0],'JOIN':'/vsizip/C:/Users/localuser/Documents/GIS data/47211_Villalar-de-los-Comuneros.zip/47211_RECFE.shp|layername=47211_RECFE','JOIN_FIELDS':['SUPERFICIE','PERIMETRO','PARCELA','USO_SIGPAC'],'METHOD':0,'DISCARD_NONMATCHING':False,'PREFIX':'','OUTPUT':'ogr:dbname=\'C:/Users/localuser/Documents/GIS data/vdlcparcelsin4.gpkg\' table="vdlc_info" (geom)'})
```
* New layer name: vdlc_info

# Step 6: Export Data in the layer "vdlc_info" and "VDLC_croptypes" to CSV file and upload to Excel
# Step 7: Process data in Excel
* Sheet 1 = "Orginaldata" exported from "VDLC_croptypes", Sheet 2 = "Orginaldat_Parcelsin4" exported from "vdlc_info"
## Data cleaning (Sheet 2)
Remove duplicates (Choose: DN	Land_Cover	SurfaceArea	SUPERFICIE	PERIMETRO	PARCELA	USO_SIGPAC)
## Data processing
### Pivot Table 1: Crop land area from Sheet 1
* Row = Land_cover
* Value = Sum of Surface area
* Filter agricultural crops to get the total surface area (purpose: cost analysis). Results (Figure 1)
### Pivot Table 2: From sheet 2
