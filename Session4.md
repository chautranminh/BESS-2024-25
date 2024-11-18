1. Removing duplicate geom - Delete duplicate geometry
2. Spatial join between the grid and the point (Make some predictions - 25 squares,)
   Join type - 1 square to many points - Join attributes by location (sample raster value if merge raster value with vector)
   we don't need many columns => fields to add (need to have at least 1 column with values in each row, in this case: scientific name)
4. Make an attribute table in gg sheet -> a table with 2 columns with ref_cell and count_occurences (in each reference cell)
* Make a pivot table - row: cell_code, value = counta of cell_code; filter: scientific name (only choose microtus arvalis)
* Export CSV file
* Put the csv back into qgis
![image](https://github.com/user-attachments/assets/75ee7d0a-4fc3-4977-b03a-e2b9c23562cf)
In case the new field become string field instead of interger -> go to editing mode -> add a new field and save as interger type
![image](https://github.com/user-attachments/assets/3b57b844-d510-4b04-bb46-5ed98bbc2c42)
![image](https://github.com/user-attachments/assets/031f2732-a028-4c12-a141-31499f1122fa)

use categorised data eventhough the data is continuous

Layout manager
Create empty layer: microtus frequency
