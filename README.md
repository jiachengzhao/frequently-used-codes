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

## Python (GEE)
- move images from one collection to another
```
import ee

SOURCE = (
    'projects/monthly-tcan-ta-myd11a1-repo1/assets/cgls1km'
)
DESTINATION = 'projects/nsfc-hkh/assets/cgls1km'
DRY_RUN = True

ee.Initialize(project='nsfc-hkh')

source_info = ee.data.getAsset(SOURCE)

if source_info is None:
    raise RuntimeError(f'Source collection not found: {SOURCE}')

source_type = source_info.get('type', '').upper()
if source_type not in ('IMAGE_COLLECTION', 'IMAGECOLLECTION'):
    raise RuntimeError(
        f'Source is not an ImageCollection; asset type is {source_info.get("type")}'
    )

source_assets = ee.data.listAssets(SOURCE).get('assets', [])

if not source_assets:
    raise RuntimeError('The source collection is empty.')

for asset in source_assets:
    asset_type = asset.get('type', '').upper()
    if asset_type not in ('IMAGE',):
        raise RuntimeError(
            f'Found a non-image child; stopping: {asset.get("name", asset.get("id"))}'
        )

destination_info = ee.data.getAsset(DESTINATION)

if destination_info is not None:
    destination_type = destination_info.get('type', '').upper()
    if destination_type not in ('IMAGE_COLLECTION', 'IMAGECOLLECTION'):
        raise RuntimeError(
            f'Destination exists but is not an ImageCollection: '
            f'{destination_info.get("type")}'
        )

    destination_assets = ee.data.listAssets(DESTINATION).get('assets', [])
    destination_names = {
        (asset.get('name') or asset.get('id')).rsplit('/', 1)[-1]
        for asset in destination_assets
    }
else:
    destination_names = set()

moves = []
for asset in source_assets:
    source_id = asset.get('name') or asset.get('id')
    image_name = source_id.rsplit('/', 1)[-1]
    destination_id = f'{DESTINATION}/{image_name}'

    if image_name in destination_names:
        raise RuntimeError(
            f'Destination already contains an image named {image_name}; '
            'no assets were moved.'
        )

    moves.append((source_id, destination_id))

print(f'Images found: {len(moves)}')

for source_id, destination_id in moves:
    print(f'{source_id}  ->  {destination_id}')

if DRY_RUN:
    print('\nDry run only; no assets were moved.')
else:
    if destination_info is None:
        ee.data.createAsset(
            {'type': 'IMAGE_COLLECTION'},
            DESTINATION
        )
        print(f'\nCreated destination collection: {DESTINATION}')

    for index, (source_id, destination_id) in enumerate(moves, start=1):
        ee.data.renameAsset(source_id, destination_id)
        print(f'Moved {index}/{len(moves)}: {source_id}')

    print('\nMove complete. The source collection is now empty.')
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

