# Geozones

##############
## The geozone
##############

A geozone is the equivalent of a folder in a file system. It can contain other geozones (*sub-geozones*) and devices.  It also represents an rectangular area of the world map. 
It is identified by a server-generated ID. Below are its main attributes:

|Attributes||
|----------|---------|
|id| Geozone ID (int) |
|name| Name |
|type | It will always be `geozone`|
|parentId | ID of the parent geozone|
|idsPath | List of geozone IDs that lead from the top-level geozone to this geozone|
|namesPath | List of geozone names that lead from the top-level geozone to this geozone |
|latMax | Maximum latitude (float)|
|latMin | Minimum latitude (float)|
|lngMax | Maximum longitude (float)|
|lngMin | Minimum longitude (float)|
|devicesCount | Number of devices |
|childrenCount | Number of sub-geozones|



