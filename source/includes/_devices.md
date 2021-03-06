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

#####################
## Retrieve a device
#####################

> Retrieving the device ID 16872 

```python
import requests

payload = {
  'deviceId': '16872',
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/asset/getDevice", params=payload, auth=('USERNAME', 'PASSWORD'))
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
        "value" : "xmltech.v1"
      },
      {
        "key" : "firmwareVersion",
        "value" : "xmltech.v1"
      },
      {
        "key" : "controller.model",
        "value" : null
      },
      {
        "key" : "controller.host",
        "value" : "localhost:8005"
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
        "key" : "userproperty.ControllerCacheMode",
        "value" : "default"
      },
      {
        "key" : "userproperty.controller.auth.password",
        "value" : "password"
      },
      {
        "key" : "userproperty.reportFrequency",
        "value" : 60
      },
      {
        "key" : "userproperty.reportTime",
        "value" : "06:00"
      },
      {
        "key" : "userproperty.serverMsgUrl.webapp",
        "value" : "http://localhost:8080/reports"
      },
      {
        "key" : "userproperty.TimeZoneId",
        "value" : "Europe/Paris"
      },
      {
        "key" : "userproperty.timeout",
        "value" : 30000
      },
      {
        "key" : "userproperty.controller.auth.username",
        "value" : "user"
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

To retrieve a device, use the method `getDevice` available at `/api/asset`. 

|Arguments||
|----------|---------|
|deviceId (required)| Device ID (int) |

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


###########################
## Commission controllers
###########################

> Commissioning the controller configuration and devices on controller *mycontroller*

```python
import requests

payload = {
  'controllerStrId':'mycontroller',
  'flags': 5,
  'ser': 'json'
}

r = requests.post(SLV_URL + "/api/controllermanagement/commissionControllerAsync", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "batchId" : "1481018818069",
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

To commission a controller, i.e. **the controller itself and all devices attached to it**, use the method `commissionControllerAsync` available at `/api/controllermanagement`.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to commission|
|flags (required)  | Indicates which items should be commissioned (int)|

The `flags` parameter should be specified depending on whether you would like to commission the controller configuration, the dimming schedules applicable to the controller devices or/and the controller devices.

|Flag                     |Value |
|-------------------------|------|
|Controller configuration | 1    |
|Calendar and schedules   | 2    |
|Controller devices       | 4    |

If you want to commission more than one item, simply add the corresponding values.

As commissioning a controller may take a long time, this method returns a batch object.

###############################
## Commission specific devices
###############################

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

To commission a list of devices that are not controllers, use the method `commissionControllerDevicesListAsync` available at `/api/controllermanagement`. It is possible to commission multiple devices at once as long as they all belong to the same controller. To do so, specify multiple `idOnController`.

As commissioning devices may take a long time, this method returns a batch object.

|Arguments||
|----------|---------|
|controllerStrId (required) | Controller to which the device is attached|
|idOnController (required)  | Device identifier on the controller|


#################################################
## Retrieve attributes values for a single device
#################################################

> Retrieving the latitude and longitude of device ID 16879

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('param0', '16879'),
  ('valueName', 'lat'),
  ('valueName', 'lng'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getDeviceLastValues", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16879,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp1",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lat",
    "status" : "OK",
    "updateTime" : null,
    "value" : 48.84032042200468
  },
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16879,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp1",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lng",
    "status" : "OK",
    "updateTime" : null,
    "value" : 2.3801171779632573
  }
]
```

To retrieve the latest values for a single device attributes, use the method `getDeviceLastValues` available at `/api/logging`. You may retrieve the value of multiple attributes at once by specifying multiple `valueName`.

|Arguments||
|----------|---------|
|param0 (required)      | Device ID (int)|
|valueName (required)   | Attribute name|
|returnTimeAges         | Set to true to retrieve information about when those values have been recorded. Defaults to false|

###################################################
## Retrieve attributes values for multiple devices
###################################################

> Retrieving the latitude and longitude of devices ID 16879 and 16880

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('param0', '16879'),
  ('param0', '16880'),
  ('param1', 'lat'),
  ('param1', 'lng'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getDevicesLastValues", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16879,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp1",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lat",
    "status" : "OK",
    "updateTime" : null,
    "value" : 48.84032042200468
  },
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16879,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp1",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lng",
    "status" : "OK",
    "updateTime" : null,
    "value" : 2.3801171779632573
  },
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16880,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp2",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lat",
    "status" : "OK",
    "updateTime" : null,
    "value" : 48.840272758283675
  },
  {
    "controllerStrId" : "mycontroller",
    "deviceId" : 16880,
    "errorMessage" : null,
    "eventTime" : null,
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "lamp2",
    "info" : null,
    "meaningLabel" : null,
    "name" : "lng",
    "status" : "OK",
    "updateTime" : null,
    "value" : 2.380476593971253
  }
]

```

To retrieve the latest values for multiple devices, use the method `getDevicesLastValues` available at `/api/logging`. You may specify multiple `param0` and `param1` to retrieve the value of several attributes on more than one device.

|Arguments||
|----------|---------|
|param0 (required)    | Device ID (int)|
|param1 (required)    | Attribute name |


###################################################
## Retrieve attributes values for a whole geozone
###################################################

> Retrieving the name, latitude and longitude of all streetlights and controllers in geozone ID 3837. We also want to rename the columns in the result from *lat* and *lng* to *Latitude* and *Longitude*.

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', '3837'),
  ('categoryStrId', 'streetlight'),
  ('categoryStrId', 'controllerdevice'),
  ('valueName', 'name'),
  ('valueName', 'lat'),
  ('valueName', 'lng'),
  ('columnName', 'lat:Latitude'),
  ('columnName', 'lng:Longitude'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getGeoZoneDevicesLastValuesAsArray", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "columnLabels" : null,
  "columns" :
    [
       "name",
       "Latitude",
       "Longitude"
    ],
  "columnsCount" : 3,
  "properties" :
    {
    },
  "rowsCount" : 4,
  "values" :
    [
       [
          "New Controller",
          48.84031689136024,
          2.3790979385375977
       ],
       [
          "My controller",
          48.84031689136024,
          2.3790979385375977
       ],
       [
          "Lamp 1",
          48.84032042200468,
          2.3801171779632573
       ],
       [
          "Lamp 2",
          48.840272758283675,
          2.380476593971253
       ]
    ]
}

```

To retrieve the latest values for all devices belonging to a geozone, use the method `getGeozoneDevicesLastValuesAsArray` available at `/api/logging`. You may specify multiple categories and attributes by adding multiple `categoryStrId` and `valueName` to your request.

In the server response, `columns` will list the attribute names by default. If you would like to receive different names instead, you may specify multiple `columnName`, one per attribute to rename, in the format `attribute name`:`desired column name`.

|Arguments||
|----------|---------|
|geozoneId (required)     | Geozone ID (int)|
|categoryStrId (required) | Category |
|valueName (required)     | Attribute name|
|columnName               | Mapping between the attribute name and the desired column name |
|recurse                  | Set to true to obtain devices belonging to the whole geozone tree under the specified geozone. Defaults to false |

############################################################
## Retrieve paginated attributes values for a whole geozone
############################################################

> Retrieving the name, latitude and longitude of all devices in geozone ID 3837, with two devices per page and reading the second page.

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', '3837'),
  ('returnedValueName', 'name'),
  ('returnedValueName', 'lat'),
  ('returnedValueName', 'lng'),
  ('pageSize', '2'),
  ('pageIndex', '1'),
  ('recurse', 'false'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getDevicesLastValuesExtAsPaginatedArray", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "columnLabels" : null,
  "columns" :
    [
       "name",
       "lat",
       "lng"
    ],
  "columnsCount" : 3,
  "pageIndex" : 1,
  "pageSize" : 2,
  "pagesCount" : 3,
  "properties" :
    {
    },
  "rowsCount" : 2,
  "totalSize" : 5,
  "values" :
    [
       [
          "My controller",
          48.84031689136024,
          2.3790979385375977
       ],
       [
          "Lamp 1",
          48.84032042200468,
          2.3801171779632573
       ]
    ]
}
```


To retrieve the latest values for all devices belonging to a geozone, but with a paginated response from the server, use the method `getDevicesLastValuesExtAsPaginatedArray` available at `/api/logging`. You may specify multiple attributes by adding multiple `returnedValueName` to your request.

|Arguments||
|----------|---------|
|geozoneId (required)         | Geozone ID (int)|
|returnedValueName (required) | Attribute name|
|pageSize (required)          | Number of devices per page |
|pageIndex (required)         | Page number, starting from zero|
|recurse (required)           | Set to true to obtain devices belonging to the whole geozone tree under the specified geozone |
|addEventTimeColumns          | Set to true to retrieve the date and time at which those values have been recorded. Defaults to false|
|filterAttributeName          | Searched attribute name|
|filterAttributeOperator      | Search operator |
|filterAttributeValue         | Searched value. If the search operator is binary, the two values should be separated by a pipe |
|sortColumn                   | Sort attribute name |
|sortDesc                     | Set to true to sort in descending order, false in ascending order |

You may also restrict the results by adding search criteria. Each search criterion requires you to specify the attribute name being searched for, the search operator and the searched value. It is possible to add multiple search critiera by specifying multiple tuples (`filterAttributeName`, `filterAttributeOperator`, `filterAttributeValue`). Search operators available are listed below:

| Operator | Description |
|----------|-------------|
| eq    | EQUAL |
| ne    | NOT EQUAL   |
| eq-i    | EQUAL IGNORE CASE   |
| like    | LIKE  |
| like-i    | LIKE IGNORE CASE  |
| partial-match   | PARTIAL MATCH   |
| partial-match-i   | PARTIAL MATCH IGNORE CASE   |
| in    | IN  |
| gt    | GREATER THAN  |
| ge    | GREATER OR EQUAL  |
| lt    | LOWER THAN  |
| le    | LOWER OR EQUAL  |
| nin   | NOT IN  |
| begins    | BEGINS  |
| begins-i    | BEGINS IGNORE CASE  |
| ends    | ENDS  |
| ends-i    | ENDS IGNORE CASE  |
| between   | BETWEEN   |


################################################################
## Retrieve attributes values for a whole geozone in a CSV file
################################################################

> Creating a CSV file called `All devices named -lamp- in geozone 3837` with the name, latitude and longitude of all devices in geozone ID 3837 whose name contain `lamp`

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('geozoneId', '3837'),
  ('returnedValueName', 'name'),
  ('returnedValueName', 'lat'),
  ('returnedValueName', 'lng'),
  ('recurse', 'false'),
  ('filterAttributeName', 'name'),
  ('filterAttributeOperator', 'partial-match-i'),
  ('filterAttributeValue', 'lamp'),
  ('csvPropertyName', 'fileBaseName'),
  ('csvPropertyValue', 'All devices named -lamp- in geozone 3837'),
  ('csvPropertyName', 'separatorChar'),
  ('csvPropertyValue', ';'),
  ('csvPropertyName', 'header'),
  ('csvPropertyValue', 'true'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/createDevicesLastValuesExtAsPaginatedArrayCsvFileUrl", data=payload, auth=('USERNAME', 'PASSWORD'))
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
  "value" : "/repository/protected-files/exports/admin/e036605902c02dd00b6e4574cc4b9f53/All%20devices%20named%20-lamp-%20in%20geozone%203837.csv"
}
```


To obtain the latest values for all devices belonging to a geozone inside a CSV file, call the method `createDevicesLastValuesExtAsPaginatedArrayCsvFileUrl` available at `/api/logging`.

|Arguments||
|----------|---------|
|geozoneId (required)         | Geozone ID (int)|
|returnedValueName (required) | Attribute name|
|recurse (required)           | Set to true to obtain devices belonging to the whole geozone tree under the specified geozone |
|addEventTimeColumns          | Set to true to retrieve the date and time at which those values have been recorded. Defaults to false|
|filterAttributeName          | Searched attribute name|
|filterAttributeOperator      | Search operator |
|filterAttributeValue         | Searched value. If the search operator is binary, the two values should be separated by a pipe |
|csvPropertyName              | CSV property name |
|csvPropertyValue             | CSV property value |

CSV properties are described below. Note that it is possible to specify multiple properties by adding several couples `csvPropertyName`/`csvPropertyValue` to your request.

| CSV property name | Description |
|-------------------|-------------|
| fileBaseName  | CSV filename (excluding the .csv extension) |
| separatorChar | CSV separator |
| header        | If set to true, includes a header line in the CSV file |

The server responds with a file path in `value`. You may then download the file from the concatenation of `SLV_URL` and `value`.


###################################
## Retrieve device logging history
###################################

> Retrieving the values of `LampLevel` and `LampCommandLevel` on devices ID 5229 and 5230 between Nov 1st 2016 1:00pm and Nov 17th 2016 1:00pm

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('deviceId', '5229'),
  ('deviceId', '5230'),
  ('name', 'LampLevel'),
  ('name', 'LampCommandLevel'),
  ('from', '01/11/2016 13:00:00'),
  ('to', '17/11/2016 13:00:00'),
  ('ser', 'json')
])

r = requests.post(SLV_URL + "/api/logging/getDevicesLogValues", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "controllerStrId" : "flowermarket",
    "deviceId" : 5230,
    "errorMessage" : null,
    "eventTime" : "2016-11-07 11:00:00",
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "stl0002",
    "info" : "",
    "meaningLabel" : null,
    "name" : "LampLevel",
    "status" : "OK",
    "updateTime" : "2016-11-07 18:08:07",
    "value" : 100.0
  },
  {
    "controllerStrId" : "flowermarket",
    "deviceId" : 5229,
    "errorMessage" : null,
    "eventTime" : "2016-11-07 18:02:26",
    "eventTimeAgeInSeconds" : null,
    "idOnController" : "stl0001",
    "info" : "",
    "meaningLabel" : null,
    "name" : "LampCommandLevel",
    "status" : "OK",
    "updateTime" : "2016-11-07 18:02:26",
    "value" : 55.0
  },
   ...
]
```

To retrieve the values of a device logging attribute over a period of time, use the method `getDevicesLogValues` available at `/api/logging`. You may specify multiple devices and/or multiple attributes.

|Arguments||
|----------|---------|
|deviceId (required)  | Device ID (int) |
|name (required)      | Attribute name |
|from (required)      | Start date and time in the format dd/MM/yyyy HH:mm:ss |
|to (required)        | End date and time in the format dd/MM/yyyy HH:mm:ss |


<aside class="success">
If a device has reported the same value multiple times over a period of time, the server will provide that value only once in the reponse to this request.
</aside>


###################
## Import devices
###################

You may create devices in bulk by importing a CSV file. SDP files for powerline projects are also supported. The method to use is `importDevicesFromCsvFileAsync`, available at `/api/loggingmanagement`.

The CSV file should be formatted as follows:
