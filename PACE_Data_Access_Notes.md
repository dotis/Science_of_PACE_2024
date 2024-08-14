### Notes on PACE data access and download using CMR API

#### Want to set up a workflow for PACE data
1. Download L2 files for BGC, AOP files
2. Mosaic and mask for clouds as is done for MODIS and VIIRS
3. Subset to Florida peninsula
4. No need for CLIM or 7D mean products yet.
5. 


Steps:
1. API search query - output to .xml file
2. Result .xml file has a bunch of header lines and then one xml line with the needed field ("location")
3. Need to parse xml file:
 - Remove header lines
 - Pull out locations 
 - To parse .xml, use "xmlstarlet sel -t -m '//location' -v '.' -n PACE_test.xml" (from ChatGPT) - can pipe to .csv file
4. Need to run a wget call for each "location" and extract "URL" from the resulting file (not an xml file; uses {} notation)
5. Then use wget to download .nc file using URL from above
6. Include authentication with ~./urs_cookies file during last wget step (as normal)
Can we streamline this?




#### Example API search query for PACE L2 BGC data
ConceptID: C3020920290-OB_CLOUD 
Short_name: PACE_OCI_L2_BGC_NRT

curl -i "https://cmr.earthdata.nasa.gov/search/granules?short_name=PACE_OCI_L2_BGC_NRT&temporal=2024-05-10T00:00:00Z,2024-05-31T00:00:00Z&bounding_box=-85,24,-78.5,28&page_size=200" -o PACE_test.xml

This returns an xml file, which must be parsed.
