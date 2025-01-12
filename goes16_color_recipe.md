


==========================
GOES-16: True Color Recipe
==========================
By: [Brian Blaylock](http://home.chpc.utah.edu/~u0553130/Brian_Blaylock/home.html)
with help from Julien Chastang (UCAR-Unidata).

Additional notebooks analyzing GOES-16 and other data can be found in [Brian's
GitHub repository](https://github.com/blaylockbk/pyBKB_v3/).

This notebook shows how to make a true color image from the GOES-16
Advanced Baseline Imager (ABI) level 2 data. We will plot the image with
matplotlib and Cartopy. The methods shown here are stitched together from the
following online resources:


- [**CIMSS True Color RGB Quick Guide**](http://cimss.ssec.wisc.edu/goes/OCLOFactSheetPDFs/ABIQuickGuide_CIMSSRGB_v2.pdf)
- [ABI Bands Quick Information Guides](https://www.goes-r.gov/education/ABI-bands-quick-info.html)
- [Open Commons Consortium](http://edc.occ-data.org/goes16/python/)
- [GeoNetCast Blog](https://geonetcast.wordpress.com/2017/07/25/geonetclass-manipulating-goes-16-data-with-python-part-vi/)
- [Proj documentation](https://proj4.org/operations/projections/geos.html?highlight=geostationary)

True color images are an RGB composite of the following three channels:

|        --| Wavelength   | Channel | Description   |
|----------|--------------|---------|---------------|
| **Red**  | 0.64 &#181;m |    2    | Red Visible   |
| **Green**| 0.86 &#181;m |    3    | Veggie Near-IR|
| **Blue** | 0.47 &#181;m |    1    | Blue Visible  |

For this demo, we use the **Level 2 Multichannel formated data** (ABI-L2-MCMIP)
for the CONUS domain. This file contains all sixteen channels on the ABI fixed
grid (~2 km grid spacing).

GOES-16 data is downloaded from Unidata, but you may also
download GOES-16 or 17 files from NOAA's GOES archive on [Amazon S3](https://aws.amazon.com/public-datasets/goes/).
I created a [web interface](http://home.chpc.utah.edu/~u0553130/Brian_Blaylock/cgi-bin/goes16_download.cgi?source=aws&satellite=noaa-goes16&domain=C&product=ABI-L2-MCMIP)
to easily download files from the Amazon archive. For scripted or bulk
downloads, you should use `rclone` or `AWS CLI`. You may also download files
from the [Environmental Data Commons](http://edc.occ-data.org/goes16/getdata/)
and [NOAA
CLASS](https://www.avl.class.noaa.gov/saa/products/search?sub_id=0&datatype_family=GRABIPRD&submit.x=25&submit.y=9).

File names have the following format...

`OR_ABI-L2-MCMIPC-M3_G16_s20181781922189_e20181781924562_c20181781925075.nc`

`OR`     - Indicates the system is operational

`ABI`    - Instrument type

`L2`     - Level 2 Data

`MCMIP`  - Multichannel Cloud and Moisture Imagery products

`c`      - CONUS file (created every 5 minutes).

`M3`     - Scan mode

`G16`    - GOES-16

`sYYYYJJJHHMMSSZ` - Scan start: 4 digit year, 3 digit day of year (Julian day), hour, minute, second, tenth second

`eYYYYJJJHHMMSSZ` - Scan end

`cYYYYJJJHHMMSSZ` - File Creation
`.nc`    - NetCDF file extension

