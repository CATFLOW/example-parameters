# example-parameters
Master Database for CATFLOW Soil and Landuse parameterisations   
(Adapted from CATFLOW - User Guide and Program Documentation by Thomas Maurer, Erwin Zehe)  
## landuse
***landuse_id.def*** - This file just holds the assignment land-use unit to several land-use Ids as e.g. exported from a GIS. The number in the first line indicates the land-use-ID column to be related to the first land-use-unit column. These land use units point on a table, that specify time dependent properties of a specific landuse types (one line each), which is stored in ***landuseclass.def***.  
***landuseclass.def*** - This file relates land use IDs, that are assigned to land use units to files that specify the time dependent properties of the specific landuse types (one line each). The first 9 columns hold the land-use identifier (INT). Columns 10-39 hold a descriptive string (CHAR*30). Columns 40-69 hold the filenames (CHAR*30) specifying land use parameters.  
***[landusetype].par*** - For each land use, a file with its parameters has to be specified, which covers all days of a year. From the first line only the number of parameters is read. The second line holds an offset at position 1 and as many factors as parameters specified. This numbers apply to the data read from subsequent lines, namely day number (of a year), surface roughness, macroporousity factor, leaf area index, coverage coefficient, rooting depth, plant height and several parameters characterising the evapo-transpiration model, as plant albedo, minimal stomata resistance, a factor and an inflexion point parameter of the soil moisture weighting function for stomata resistance computation. The last 3 columns hold land-use dependent erosion parameters not yet specified in more detail.
The offset allows shifting the time axis, which then is automatically extended to cover the range from 1 to 366. The factors are used to scale the associated land-use parameters by multiplication. The tables generated this way represent time series of the (changing) parameters, which are evaluated at runtime by linear interpolation.  
The different possible landuse parameters are:
|Parameter|Definition|Comments|
|---|---|---|
|KST|Strickler coefficient||
|FMAC|Macroporousity factor|currently not used in calculations|  
|BFI|Leaf Area Index||  
|BBG|Plant Coverage||  
|TWU|Root Depth||   
|PFH|Plant Height|| 
|PALB|Plant Albedo||  
|RSTMIN|Minimum Stomata Resistance||  
|FBFW|Factor for soil moisture weigthing function for stomata resistance|typically around 30|  
|WPBFW|Inflexion point of soil moisture weigthing function for stomata resistance|typically around 0.05|
|Eros1|land-use dependent erosion parameter|currently not used in calculations|  
|Eros2|land-use dependent erosion parameter|currently not used in calculations|
|Eros3|land-use dependent erosion parameter|currently not used in calculations|  
## soil types
***soil_types.def*** - Here all parameters to describe the hydraulic and transport properties of all soils are specified. At initialisation, CATFLOW produces look-up-tables from this data. The header line of the file contains the number of soil types. Each soil type is then defined starting with a descriptive string and then followed by a block of 5 lines. Sample Definition -
1) boden(ityp)  
2) imod(ityp) iactab(ityp) anisot(1,ityp) anisot(2,ityp) snull(ityp)  
satkni(ityp) balmax(ityp) balmin(ityp)  
f_rs(ityp) wp_rs(ityp) zd_max(ityp)  
eross(1,ityp) eross(2,ityp) eross(3,ityp)  
3) (bodpar(ipbm,ityp), ipbm=1,npbm), sattgr  
4) (abbau(ipbm,ityp), ist=1,istact)  
5) (k_sorp(ipbm,ityp), ist=1,istact)  
6) (d:koef(ipbm,ityp), ist=1,istact)  
  
The different possible soil parameters are:
 |Parameter|Datatype|Definition|
 |---|---|---|
 |boden(ityp)|CHAR*30|descriptive string| 
 |imod(ityp)|INT|soil model identifier| 
 |iactab(ityp)|INT|length of table to be generated|
 |anisot(1,ityp)|REAL|factor on conductivity for the larger main direction of anisotropy|
 |anisot(2,ityp)|REAL|factor on conductivity for the smaller main direction of anisotropy|
 |snull(ityp)|REAL|specific storage coefficient| 
 |satkni(ityp)|REAL|albedo-soil moisture relation parameter|
 |balmax(ityp)|REAL|albedo-soil moisture relation parameter| 
 |balmin(ityp)|REAL|albedo-soil moisture relation parameter| 
 |f_rs(ityp)|REAL|factor for soil moisture weighting of soil resistance| 
 |wp_rs(ityp)|REAL|inflexion point of soil moisture weighting of soil resistance| 
 |zd_max(ityp)|REAL|maximum thickness of top dry layer (soil resistance, experimentally determined)| 
 |eross(i,ityp)|REAL|3 soil dependent erosion parameters (dummy values)| 
 |bodpar(ipbm,ityp)|REAL|parameters of soil model, number npbm+ depends on imod(ityp)|
|abbau(ist, ityp)|REAL|life times characterising first order decay of the specified solutes in the soil|
|k_sorp(ist, ityp)|REAL|adsorption coefficients of the specified solutes in the soil| 
|d_koef(ist, ityp)|REAL|dispersion coefficients of specified solutes in the soil|


