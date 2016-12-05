# Geozones

##############
## The geozone
##############

A geozone is the equivalent of a folder in a file system. It can contain other geozones (*sub-geozones*) and devices.  It also represents an rectangular area of the world map. 
It is identified by a server-generated ID. Below are its main attributes:

|Attributes||
|----------|---------|
|id| Geozone ID (int) |
|name| Geozone name |
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


####################
## Create a geozone
####################

> Creating a geozone called `My new geozone` under the geozone ID 1

```python
import requests

payload = {
  'name': 'My new geozone', 
  'parentId': 1,
  'latMin': 48.79035455868981,
  'latMax': 48.92655972351346,
  'lngMin': 2.1097183227539067,
  'lngMax': 2.5491714477539067,
  'ser': 'json'
}

r = requests.post(SLV_CMS + "/api/assetmanagement/createGeozone", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "activityCardsCount" : 0,
  "childrenCount" : 0,
  "devicesCount" : 0,
  "hierarchyActivityCardsCount" : -1,
  "hierarchyDevicesCount" : -1,
  "id" : 3831,
  "idsPath" : "1/3831",
  "latMax" : 48.92655972351346,
  "latMin" : 48.79035455868981,
  "lngMax" : 2.5491714477539067,
  "lngMin" : 2.1097183227539067,
  "name" : "My new geozone",
  "namesPath" : "GeoZones/My new geozone",
  "parentId" : 1,
  "properties" : null,
  "type" : "geozone"
}
```

To create a geozone, call the method `createGeozone` available at `/api/assetmanagement`. 

|Arguments||
|----------|---------|
|name (required)| Geozone name |
|parentId (required)| ID of the parent geozone|
|latMax (required)| Maximum latitude (float)|
|latMin (required)| Minimum latitude (float)|
|lngMax (required)| Maximum longitude (float)|
|lngMin (required)| Minimum longitude (float)|


####################
## Update a geozone
####################

> Updating the geozone ID 3831 with some additional properties

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', '3831'), 
  ('newName', 'Paris'), 
  ('lngMax', '2.5491714477539067'),
  ('lngMin', '2'),
  ('latMax','48.92655972351346'),
  ('latMin','48.79035455868981'),
  ('propertyName', 'virtual.energy.consumption.jan'),
  ('propertyValue', '18000'),
  ('propertyName', 'virtual.energy.consumption.feb'),
  ('propertyValue', '17500'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/assetmanagement/updateGeozone", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
   "activityCardsCount" : 0,
   "childrenCount" : 0,
   "devicesCount" : 0,
   "hierarchyActivityCardsCount" : -1,
   "hierarchyDevicesCount" : -1,
   "id" : 3831,
   "idsPath" : "1/3824/3831",
   "latMax" : 48.92655972351346,
   "latMin" : 48.79035455868981,
   "lngMax" : 2.5491714477539067,
   "lngMin" : 2.0,
   "name" : "Paris",
   "namesPath" : "GeoZones/Paris",
   "parentId" : 1,
   "properties" : null,
   "type" : "geozone"
}
```

To modify an existing geozone, call the method `updateGeozone` available at `/api/assetmanagement`. In addition to its main attribute, a geozone can have any number of optional arbitrary properties, so this method may take any number of `propertyName`/`propertyValue` pairs in addition to its required parameters.

|Arguments||
|----------|---------|
|geozoneId (required) | Geozone ID |
|newName (required)   | Geozone name |
|latMax (required)    | Maximum latitude (float)|
|latMin (required)    | Minimum latitude (float)|
|lngMax (required)    | Maximum longitude (float)|
|lngMin (required)    | Minimum longitude (float)|
|parentId             | ID of the new parent geozone|
|propertyName         | Property name | 
|propertyValue        | Property value | 

<aside class="warning">
Do not forget to set newName even if you don't want to change the name of your geozone, otherwise it will be changed to My new geozone by the server! 
</aside>

<aside class="warning">
Do not forget to set all longitudes and latitudes, as otherwise they will default to null.
</aside>

<aside class="notice">
In the server response, properties is always null even if you have set some optional properties using propertyName/propertyValue
</aside>


######################
## Delete a geozone
######################

> Deleting the geozone ID 3831 and everything belonging to it

```python
import requests

payload = {
  'geozoneId': '3831', 
  'pullUpContent': 'false',
  'ser': 'json'
}

r = requests.post(SLV_URL + "/api/assetmanagement/deleteGeozone", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
"OK"
```

To delete a geozone, use the method `deleteGeozone` available at `/api/assetmanagement`. 

|Arguments||
|----------|---------|
|geozoneId (required) | Geozone ID |
|pullUpContent| If set to true, moves the content of the geozone under the parent geozone. If set to false, deletes everything under the geozone. True by default|

<aside class="notice">
Setting pullUpContent to true actually does not work. So the behaviour of this method is always to delete everything under the geozone.
</aside>


##########################
## Retrieve all geozones
##########################

> Retrieving all geozones

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/asset/getAllGeozones", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "activityCardsCount" : 0,
    "childrenCount" : 10,
    "devicesCount" : 7,
    "hierarchyActivityCardsCount" : -1,
    "hierarchyDevicesCount" : -1,
    "id" : 1,
    "idsPath" : "1",
    "latMax" : 80.0,
    "latMin" : -80.0,
    "lngMax" : 179.9999,
    "lngMin" : -179.9999,
    "name" : "GeoZones",
    "namesPath" : "GeoZones",
    "parentId" : null,
    "properties" : null,
    "type" : "geozone"
  },
...
]

```
To retrieve all geozones visible by the current user, use the method `getAllGeozones` available at `/api/asset`. It has no required parameter.


###########################################
## Retrieve optional properties
###########################################

> Retrieving the optional properties of the geozone ID 1

```python
import requests

payload = {
  'geozoneId': 1,
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/assetmanagement/getGeozoneValueDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "category" : "standard",
    "criticity" : 0,
    "dataFormat" :
       {
          "min" : "0"
       },
    "failure" : false,
    "help" : null,
    "label" : "January kWh",
    "labelKey" : "db.meaning.virtual.energy.consumption.jan.label",
    "name" : "virtual.energy.consumption.jan",
    "type" : "int",
    "unit" : null
  }
  ...
]
```


To retrieve the optional properties of a geozone, you may use the method `getGeozoneValueDescriptors` available at `/api/logging`. 

|Arguments||
|----------|---------|
|geozoneId (required) | Geozone ID |


############################
## Retrieve optional values
############################

> Retrieving two optional values for geozone ID 1

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', '1'), 
  ('valueName', 'virtual.energy.consumption.jan'),
  ('valueName', 'virtual.energy.consumption.feb'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/logging/getGeozoneLastValues", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "eventTime" : "2016-12-05 00:00:00",
    "groupId" : 1,
    "info" : "",
    "meaningLabel" : "January kWh",
    "name" : "virtual.energy.consumption.jan",
    "updateTime" : "2016-12-05 00:00:00",
    "value" : 20000.0
  },
  {
    "eventTime" : "2016-12-05 00:00:00",
    "groupId" : 1,
    "info" : "",
    "meaningLabel" : "February kWh",
    "name" : "virtual.energy.consumption.feb",
    "updateTime" : "2016-12-05 00:00:00",
    "value" : 19000.0
  }
]

```



To retrive the values of those optional properties, use the method `getGeozoneLastValues` available at `/api/logging`. This method takes any number of `valueName`. The server response includes values in the same order as the request parameters.


|Arguments||
|----------|---------|
|geozoneId (required) | Geozone ID                      |
|valueName (required) | Name of the optional attribute  |

<aside class="notice">
In the server response, `eventTime` and `updateTime` always have a time set to midnight.
</aside>



