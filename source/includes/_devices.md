# Devices

##############
## The device
##############

A device is identified by a server-generated ID. Despite its name, it may represent any object apart from a geozone. A device is always attached to a controller and has its own identifier on that controller. 

|Attributes||
|----------|---------|
|id| Device ID (int) |
|name| Device name |
|categoryStrId | Device category |
|geozoneId | ID of the parent geozone|
|controllerStrId| Controller to which the device is attached |
|idOnController| Device identifier on the controller |
|lat | Latitude |
|lng | Longitude |


##################################
## Retrieve all device categories
##################################

> Retrieving all device categories

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/asset/getCategories", params=payload, auth=('USERNAME', 'PASSWORD'))
```
> Sample response

```python
[
   {
      "displayName" : "Vehicle Charging Station",
      "hidden" : false,
      "strId" : "vehicleChargingStation"
   },
   {
      "displayName" : "Environmental Sensor",
      "hidden" : false,
      "strId" : "envSensor"
   },
   ...
] 
```

To retrive all device categories, use the method `getCategories` available at `/api/asset`. It has no required parameter.

###################
## Create a device
###################

> Creating a new controller device

```python
import requests

payload = {
  'userName': 'My controller', 
  'categoryStrId': 'controllerdevice',
  'controllerStrId': 'mycontroller',
  'idOnController': 'controllerdevice',
  'geoZoneId':3837,
  'lat':48.84031689136024,
  'lng':2.3790979385375977,
  'ser': 'json'
}

r = requests.post(SLV_CMS + "/api/assetmanagement/createCategoryDevice", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "address" : null,
  "categoryStrId" : "controllerdevice",
  "controllerStrId" : "mycontroller",
  "functionId" : null,
  "geoZoneId" : 3837,
  "geoZoneNamesPath" : "GeoZones/My new geozone",
  "id" : 16872,
  "idOnController" : "controllerdevice",
  "lat" : 48.84031689136024,
  "lng" : 2.3790979385375977,
  "modelName" : null,
  "name" : "My controller",
  "nodeTypeStrId" : null,
  "properties" :
    [
      {
        "key" : "controller.firmwareVersion",
        "value" : null
      },
      {
        "key" : "firmwareVersion",
        "value" : null
      },
      {
        "key" : "controller.model",
        "value" : null
      },
      {
        "key" : "controller.host",
        "value" : null
      },
      {
        "key" : "controller.installDate",
        "value" : "2016/12/06 11:47:36"
      },
      {
        "key" : "controller.updateTime",
        "value" : ""
      },
      {
        "key" : "power",
        "value" : null
      },
      {
        "key" : "powerCorrection",
        "value" : null
      },
      {
        "key" : "tooltip",
        "value" : ""
      }
    ],
  "technologyStrId" : null,
  "type" : "device"
}
```

To create a device, use the method `createCategoryDevice` available at `/api/assetmanagement`. Since a device is always attached to a controller, you must specify a controller when creating a device. 

|Arguments||
|----------|---------|
|userName (required)| Device name |
|categoryStrId (required)| Device category |
|geozoneId (required)| ID of the parent geozone|
|controllerStrId (required)| Controller to which the device is attached. Use only lowercase letters and digits |
|idOnController (required)| Device identifier on the controller. Use only lowercase letters and digits |
|lat (required)| Latitude (float)|
|lng (required)| Longitude (float)|

<aside class="success">
When creating a controller, set idOnController to "controllerdevice".
</aside>


###################
## Update a device
###################

> Updating the location of the device `mycontroller`

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('controllerStrId','mycontroller'),
  ('idOnController','controllerdevice'),
  ('valueName', 'lat'),
  ('value','48.84031689136024'),
  ('valueName', 'lng'),
  ('value','2.3790979385375977'),
  ('doLog', 'true'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/loggingmanagement/setDeviceValues", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
   "errorCode" : "0",
   "errorCodeLabel" : null,
   "message" : null,
   "status" : "OK",
   "statusError" : false,
   "statusOk" : true,
   "value" :
      [
         "OK:48.84031689136024",
         "OK:2.3790979385375977"
      ]
}
```

To update a device, use the method `setDeviceValues` available at `/api/loggingmanagement`. You must specify the device to update by its `controllerStrId` and `idOnController` attributes. The method `setDeviceValues` can be used to set any number of arbitrary device attributes by taking any number of `valueName`/`value` pairs. 

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|
|valueName (required)       | Attribute name|
|value (required)           | Attribute value|
|doLog                      | Set to true to log historical changes in the database. Defaults to false|
|eventTime                  | Date and time at which the device has been created, in the format YYYY-MM-DD HH:mm:ss (e.g. 2016-08-02 11:45:00). If not specified, it will be automatically generated by the server|
|createDevice               | Set to true to create the specified device if it does not exist. Defaults to false|

<aside class="success">
Always set doLog to true.
</aside>


########################################
## Retrieve device inventory attributes
########################################

> Retrieving all inventory attributes for the device `mycontroller`

```python
import requests

payload = {
  'controllerStrId': 'mycontroller',
  'idOnController': 'controllerdevice',
  'configFilePath': 'adminEquipmentsDeviceCardConfigs.xml',
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/logging/getDeviceValueDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "category" : "internal",
    "criticity" : 0,
    "dataFormat" :
      {
        "readOnly" : "true",
        "max.length" : "32",
        "format" : "[a-zA-Z0-9\\[\\]_\\-@!:\\. ]+",
        "mainGroup" : "controller.identity",
        "mainGroup.labelKey" : "mainGroup.controller.identity.label",
        "subGroup" : "controller.identity.group1",
        "subGroup.labelKey" : "subGroup.controller.identity.group1.label",
        "mainGroup.label" : "Identity",
        "subGroup.label" : "Identity of the controller"
      },
    "failure" : false,
    "help" : null,
    "label" : "Controller ID",
    "labelKey" : "db.meaning.controllerstrid.label",
    "name" : "controllerStrId",
    "type" : "string",
    "unit" : null
  },
  ...
]
```

To retrieve all inventory attributes available for a device, use the method `getDeviceValueDescriptors` available at `/api/logging`. 

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|
|configFilePath (required)  | Always set to *adminEquipmentsDeviceCardConfigs.xml*|


#########################################
## Retrieve device logging attributes
#########################################

> Retrieving all inventory attributes for the device `mycontroller`

```python
import requests

payload = {
  'controllerStrId': 'mycontroller',
  'idOnController': 'controllerdevice',
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/logging/getDeviceValueDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "category" : "standard",
    "criticity" : 0,
    "dataFormat" : null,
    "failure" : false,
    "help" : null,
    "label" : "Digital Input 1",
    "labelKey" : "db.meaning.digitalinput1.label",
    "name" : "DigitalInput1",
    "type" : "boolean",
    "unit" : null
  },
  ...
]
```

To retrieve all logging attributes available for a device, use the method `getDeviceValueDescriptors` available at `/api/logging` without specifying any `configFilePath`.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|


######################################
## Retrieve device attributes by name
######################################

> Retrieving the description of the attributes `lat` and `lng`

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('valueName', 'lat'),
  ('valueName', 'lng'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getDeviceValueDescriptorsByName", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    [
      {
        "category" : "internal",
        "criticity" : 0,
        "dataFormat" :
           {
           },
        "failure" : false,
        "help" : null,
        "label" : "lat",
        "labelKey" : null,
        "name" : "lat",
        "type" : "double",
        "unit" : null
      },
      {
        "category" : "internal",
        "criticity" : 0,
        "dataFormat" :
           {
           },
        "failure" : false,
        "help" : null,
        "label" : "lng",
        "labelKey" : null,
        "name" : "lng",
        "type" : "double",
        "unit" : null
      }
    ]
}

```

You may retrieve the description and details of device attributes if you always know their name by using the method `getDeviceValueDescriptorsByName` available at `/api/logging`. You may specify any number of attributes.

|Arguments||
|----------|---------|
|valueName (required)       | Attribute name|


##################################
## Retrieve attributes by file
##################################

> Retrieve all device attributes in gcCustomReportReportValuesSelected.xml

```python
import requests

payload = {
  'configFilePath': 'gcCustomReportReportValuesSelected.xml',
  'ser': 'json'
}


r = requests.get(SLV_URL + "/api/logging/getAllDevicesValueDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "category" : "internal",
    "criticity" : 0,
    "dataFormat" :
      {
        "max.length" : "77",
        "mainGroup" : "controller.inventory",
        "mainGroup.labelKey" : "mainGroup.controller.inventory.label",
        "subGroup" : "controller.inventory.location",
        "subGroup.labelKey" : "subGroup.lightpoint.inventory.location.label",
        "subGroup.collapsed" : "true",
        "ExcludeFromCommissioning" : "true",
        "mainGroup.label" : "Inventory",
        "subGroup.label" : "Location"
      },
    "failure" : false,
    "help" : null,
    "label" : "Address 1",
    "labelKey" : "db.meaning.address.label",
    "name" : "address",
    "type" : "string",
    "unit" : null
  },
  ...
]
```

You may retrieve all device attributes listed in a server-side configuration file by calling the method `getAllDevicesValueDescriptors` available at `/api/logging`.

|Arguments||
|----------|---------|
|configFilePath (required)  | File name|

The following file names are available:

|File name |Description|
|----------|-----------|
|gcCustomReportReportValues.xml  | |
|gcCustomReportReportValuesSelected.xml | |
|gcCustomReportReportValuesFavorites.xml | |
|gcDataHistoryReportValues.xml  | |
|gcDataHistoryReportValuesSelected.xml | |
|gcDataHistoryReportValuesFavorites.xml | |
|gcEquipmentsReportValues.xml  | |
|gcEquipmentsReportValuesSelected.xml | |
|gcEquipmentsReportValuesFavorites.xml | |


################################
## Retrieve devices by location
################################

> Retrieving all devices with a latitude between 48.840 and 48.841 and a longitude between 2.378 and 2.381

```python
import requests

payload = {
  'latMin':48.840,
  'latMax':48.841,
  'lngMin':2.378,
  'lngMax':2.381,
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/asset/getDevicesInBounds", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "address" : "5 Boulevard de Bercy",
    "categoryStrId" : "controllerdevice",
    "controllerStrId" : "newcontroller",
    "functionId" : null,
    "geoZoneId" : 3837,
    "geoZoneNamesPath" : "GeoZones/My new geozone",
    "id" : 16871,
    "idOnController" : "controllerdevice",
    "lat" : 48.84031689136024,
    "lng" : 2.3790979385375977,
    "modelName" : null,
    "name" : "New Controller",
    "nodeTypeStrId" : null,
    "properties" : null,
    "technologyStrId" : null,
    "type" : "device"
  },
  ...
]
```


To retrieve all devices within a certain geographical area, you may use the method `getDevicesInBounds` available at `/api/asset`.

|Arguments||
|----------|---------|
|latMin (required)| Minimum latitude (float)  |
|latMax (required)| Maximum latitude (float)  |
|lngMin (required)| Minimum longitude (float) |
|lngMax (required)| Maximum longitude (float) |
|light            | If set to true, always returns `properties=null`. Defaults to true |


##############################
## Retrieve a device timezone
##############################

> Retrieve the timezone of device ID 16871

```python
import requests

payload = {
  'deviceId':16871,
  'ser': 'json'
}


r = requests.get(SLV_URL + "/api/asset/getDeviceTimeZone", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
   "displayName" : "Central European Time [+01:00]",
   "dstSaving" : 3600000,
   "id" : "Europe/Paris",
   "rawOffset" : 3600000
}
```

To retrieve the timezone of a device, call the method `getDeviceTimeZone` available at `/api/asset`. The `id` provided in the result follows the nomenclature used in [tzdata](https://en.wikipedia.org/wiki/Tz_database).

|Arguments||
|----------|---------|
|deviceId (required)| Device ID (int) |


##########################
## Retrieve all timezones
##########################

> Retrieving all timezones available

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/controller/getAvailableTimezones", params=payload, auth=('admin', 'password'))
```

> Sample response

```python
[
  {
    "displayName" : "Niue Time [-11:00]",
    "dstSaving" : 0,
    "id" : "Pacific/Niue",
    "rawOffset" : -39600000
  },
  {
    "displayName" : "Samoa Standard Time [-11:00]",
    "dstSaving" : 0,
    "id" : "Pacific/Midway",
    "rawOffset" : -39600000
  },
  ...
]
```
You may retrieve the list of all timezones available on the server by calling the method `getAvailableTimezones` available at `/api/controller`. This method has no required parameter.



################################
## Retrieve devices by geozone 
################################

> Retrieving all streetlight and switch devices belonging to geozone ID 1 and its sub-geozones

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', 1),
  ('recurse','true'),
  ('categoryStrId', 'streetlight'),
  ('categoryStrId', 'switch'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/asset/getGeozoneDevices", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "address" : null,
    "categoryStrId" : "streetlight",
    "controllerStrId" : "meifoo",
    "functionId" : "Lamp1",
    "geoZoneId" : 1,
    "geoZoneNamesPath" : "GeoZones",
    "id" : 5238,
    "idOnController" : "227.lamp1",
    "lat" : 22.3476744529559,
    "lng" : 114.109046459198,
    "modelName" : "SysNode double DALI ballasts",
    "name" : "227.lamp1",
    "nodeTypeStrId" : "sysnode_dali_2b",
    "properties" :
       [
          {
             "key" : "dimmable",
             "value" : true
          },
          {
             "key" : "controller.firmwareVersion",
             "value" : "talq"
          },
          {
             "key" : "power",
             "value" : 100.0
          },
          {
             "key" : "powerCorrection",
             "value" : 20.0
          },
          {
             "key" : "lampTypeName",
             "value" : "/SLV/HPS/70/MAG"
          },
          {
             "key" : "lampLifeTime",
             "value" : 20000
          },
          {
             "key" : "userproperty.ballast.dimmingtype",
             "value" : "DALI LOG"
          },
          {
             "key" : "userproperty.DimmingGroupName",
             "value" : ""
          },
          {
             "key" : "userproperty.location.locationtype",
             "value" : "LOCATION_TYPE_PREMISE"
          },
          {
             "key" : "userproperty.network.highvoltagethreshold",
             "value" : 245
          },
          {
             "key" : "userproperty.network.lowvoltagethreshold",
             "value" : 215
          },
          {
             "key" : "userproperty.network.supplyvoltage",
             "value" : "120 Volts"
          },
          {
             "key" : "userproperty.OffLuxLevel",
             "value" : 30.0
          },
          {
             "key" : "userproperty.OnLuxLevel",
             "value" : 10.0
          },
          {
             "key" : "userproperty.PowerFactorThreshold",
             "value" : 0.6
          },
          {
             "key" : "tooltip",
             "value" : "SysNode double DALI ballasts[First ballast] - HPS 70W Ferro"
          }
       ],
    "technologyStrId" : "lonworks",
    "type" : "device"
  },
  ...
]
```

To retrieve all devices belonging to a specific geozone, use the method `getGeozoneDevices` available at `/api/asset`. You may restrict your search to certain device categories by specifying them using `categoryStrId`.

|Arguments||
|----------|---------|
|geozoneId (required)| Geozone ID (int) |
|recurse  | Set to true to obtain devices belonging to the whole geozone tree under the specified geozone. Defaults to false |
|categoryStrId  | Category ID (string) |



#####################
## Deleting a device
#####################

> Deleting the device *lamp3* on controller *mycontroller*

```python
import requests

payload = {
  'controllerStrId': 'mycontroller',
  'idOnController': 'lamp3',
  'ser': 'json'
}

r = requests.post(SLV_URL + "/api/assetmanagement/deleteDevice", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response
```python
"OK"
```


To delete a single device, use the method `deleteDevice` available at `/api/assetmanagement` 

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|


######################
## Deleting devices
######################

> Deleting devices *lamp1* and *lamp2* on controller *mycontroller*

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp1'),
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp2'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/assetmanagement/deleteDevices", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    [
      "OK",
      "OK"
    ]
}
```

To delete devices in bulk, use the method `deleteDevices` available at `/api/assetmanagement` and specify any number of `controllerStrId`/`idOnController` pairs.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|


##################
## Replace lamps
##################

> Replacing the lamps of both devices *lamp1* and *lamp2* on controller *mycontroller*

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp1'),
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp2'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/monitoringmanagement/replaceLamps", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    [
      "OK",
      "OK"
    ]
}
```

You may replace the lamp on a *streetlight* device by calling the method `replaceLamps` available at '/api/monitoringmanagement'. It is possible to specify any number of devices on which to perform this operation by using multiple `controllerStrId`/`idOnController` pairs.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|


########################
## Replace smart nodes
########################

> Replacing the smart nodes of both devices *lamp1* (new unique address: *A1*) and *lamp2* (new unique address: *A2*) on controller *mycontroller*

```python
import requests
from webob.multidict import MultiDict

# SLV CMS parameters
SLVCMSHost = "172.16.244.150:8080"
SLVCMSPath = "reports"
SLV_URL="http://" + SLVCMSHost+ "/" + SLVCMSPath

payload = MultiDict([
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp1'),
  ('newNetworkId','A1'),
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp2'),
  ('newNetworkId', 'A2'),
  ('ser','json')
])

r = requests.post(SLV_URL + "/api/monitoringmanagement/replaceOLCs", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    [
      "OK",
      "OK"
    ]
}
```

You may replace the smart node on a *streetlight* device by calling the method `replaceOLCs` available at '/api/monitoringmanagement'. It is possible to specify any number of devices on which to perform this operation by using multiple `controllerStrId`/`idOnController`/`newNetworkId` tuples.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|
|newNetworkId (required)    | Unique address of the new smart node |



##########################
## Commission devices
##########################

> Commissioning devices *lamp1* and *lamp2* on controller *mycontroller*

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('controllerStrId', 'mycontroller'),
  ('idOnController','lamp1'),
  ('idOnController','lamp2'),
  ('ser','json')
])

r = requests.post(SLV_URL + "/api/controllermanagement/commissionControllerDevicesListAsync", data=payload, auth=('USERNAME', 'PASSWORD'))

```

> Sample response

```python
{
  "batchId" : "1481011141616",
  "batchProgressMessage" : null,
  "batchProgressValue" : -1,
  "batchRunning" : true,
  "cancelRequested" : false,
  "cancelled" : false,
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    {
      "errorMessagesCount" : 0,
      "infoMessagesCount" : 0,
      "login" : "admin",
      "status" : 0,
      "stepResults" :
        [
        ],
      "warningMessagesCount" : 0
    }
}

```

To commission devices, use the method `commissionControllerDevicesListAsync` available at `/api/controllermanagement`. It is possible to commission multiple devices at once as long as they all belong to the same controller. To do so, specify multiple `idOnController`.

As commissioning devices may take a long time, this method returns a batch object.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|