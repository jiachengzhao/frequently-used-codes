# Frequently used codes for scientific analyses

## Google Earth Engine (GEE)
- register for a cloud project
```
earthengine set_project community-data-454112
```

- Reproject image1 to image2
```
image1.resample('bilinear').reproject({
    crs: image2.projection().crs(),
    scale: image2.projection().nominalScale()
});
```

- Export image to asset globally
```
Export.image.toAsset({
    image: image,
    description: exportName,
    assetId: 'projects/hindu-kush-himalayas-data1/assets/Ts-minus-Ta-myd-1km/' + exportName,
    region: ee.Geometry.Polygon([-180, 88, 0, 88, 180, 88, 180, -88, 0, -88, -180, -88], null, false),
    crs: modisProjectionInfo.crs,
    crsTransform: modisProjectionInfo.transform,
    maxPixels: 1e13
  });
```

## R
- Import Windows fonts to R
```
install.packages('remotes')
remotes::install_version('Rttf2pt1', version = '1.3.8')
install.packages('extrafont')
extrafont::font_import()
extrafont::loadfonts(device = 'win')
```
- Rstudio header
```
snippet header
	## ----------------------------------
	# Title: 
	# Objective: 
	# Created by: Jiacheng Zhao
	# Created on: `r Sys.Date()`
	# Copyright (c) Jiacheng Zhao, `r format(Sys.Date(), "%Y")`
	# Nanjing University of Information Science & Technology (NUIST)
	# Email: jzhao@nuist.edu.cn
	## ----------------------------------
```
```
snippet par
	# cex --------------------------------
	cex.axis = 1.1
	cex.point = 1.7
	cex.legend = 1
	cex.label = 0.7
	cex.axis.label = 0.8
	cex.title = 1.5
	
	# font -------------------------------
	font = 'Calibri'
	
	# lwd --------------------------------
	lwd = 0.5
	medlwd = 1.3
	boxwex = 0.18
	lwd.axis = 0.5
	
	# axis -------------------------------
	x.axis.line = 2.2; y.axis.line = 1.8
	mgp1 = c(3, 0.1, 0)
	mgp2 = c(3, 0.2, 0)
	mgp3 = c(3, 0.1, 0)
	mgp4 = c(3, 0.2, 0)
	tck = 0.03
	
	# legend -----------------------------
	inset = c(0, 2)
	x.intersp = 0.9; y.intersp = x.intersp
	
	# color ------------------------------
	col = 'red'
	cols = c('seagreen', 'gold')
	
	# par --------------------------------
	par(
	cex.axis = cex.axis,
	family = font,
	las = 1,
	lwd = lwd,
	mai = c(0, 0.5, 0, 0.2),
	mfrow = c(6, 1),
	oma = c(6.5, 1.5, 2, 1),
	tck = tck
	)

```
- Create an r package
```
require(devtools)
setwd('C:/Users/jzhao/Documents/Nutstore/Projects')
create('smot')
```

- Pattern matching and replacement
```
DT[grep('(?=.*A)(?!.*W)', group, perl = T)]
```

## era5cli
- Requests of ERA5-Land hourly data
   ```
   era5cli hourly --land --variables 2m_temperature  --startyear 1950 --endyear 1950 --splitmonths True
   ```
   
## 7zip
- Batch unzip
   ```
   "C:\Program Files\7-zip\7z.exe" e *.zip
   ```

