# Lamp types

########################
## The lamp type (brand)
########################

A brand is an object representing a lamp type in the SLV CMS. It is referenced by its internal server ID. Below are its main attributes:

|Attributes||
|----------|---------|
|id| Internal server ID (int) |
|name| Brand textual ID |
|description | Brand display name | 

A lamp type also has numerous optional attributes. The list of those attributes should be obtained directly from the server.

######################
## Create a lamp type
######################


> Creating a new lamp type identified as `slv-lamp` and called `SLV Lamp` with a couple of sample properties

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('name', 'slv-lamp'), 
  ('description', 'SLV Lamp'),
  ('propertyName', 'LampPower'),
  ('propertyValue', '85'),
  ('propertyName', 'Vno'),
  ('propertyValue', '230'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/assetmanagement/createBrand", data=payload, auth=('USERNAME', 'PASSWORD'))
```


> Sample response

```python
{
  "label" : "SLV Lamp",
  "properties" :
    {
      "Vno" : "230",
      "name" : "slv-lamp",
      "description" : "SLV Lamp",
      "LampPower" : "85",
      "id" : "1163",
      "lifeTime" : "null"
    },
  "value" : 1163
}

```

To create a new lamp type, the method `createBrand` available at `/api/assetmanagement` should be used. A lamp type can have any number of arbitrary properties, so this method can take any number of `propertyName`/`propertyValue` pairs in addition to its required parameters.

|Arguments |         |
|----------|---------|
|name (required)        | Brand textual ID |
|description (required) | Brand display name | 
|propertyName           | Property name | 
|propertyValue          | Property value | 

The server reponse contains the server-generated internal ID (`properties.id`) that should be used for further operations on lamp types.

#####################
## Update a lamp type
#####################

> Updating the properties *LampPower* and *Vno* of the lamp type ID 1163

```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('id', '1163'), 
  ('propertyName', 'LampPower'),
  ('propertyValue', '48'),
  ('propertyName', 'Vno'),
  ('propertyValue', '110'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/assetmanagement/updateBrand", data=payload, auth=('USERNAME', 'PASSWORD'))
```


> Sample response

```python
{
 "label" : "SLV Lamp",
 "properties" :
    {
      "Vno" : "110",
      "name" : "slv-lamp",
      "description" : "SLV Lamp",
      "LampPower" : "48",
      "id" : "1163",
      "lifeTime" : "null"
    },
 "value" : 1163
}
```

To update the attributes of a lamp type, use the method `updateBrand` available at `/api/assetmanagement`. This method can take any number of `propertyName`/`propertyValue` pairs in addition to the required `id`.

|Arguments |         |
|----------|---------|
|id (required)  | Internal server ID (int) |
|newName        | New brand textual ID |
|newDescription | New drand display name | 
|propertyName           | Property name | 
|propertyValue          | Property value | 


#####################
## Delete a lamp type
#####################

> Deleting the lamp type ID 1163

```python
import requests

payload = {
  'brandId': '1163', 
  'ser': 'json'
}

r = requests.post(SLV_URL + "/api/assetmanagement/deleteBrand", data=payload, auth=('USERNAME', 'PASSWORD'))
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
  "value" : null
}

```


To delete a lamp type from the server, use the method `deleteBrand` available at `/api/assetmanagement`.

|Arguments |         |
|----------|---------|
|brandId (required)  | Internal server ID (int) |


##########################
## Retrieve all lamp types
##########################

> Retrieving all lamp types

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/asset/getAllBrands", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
[
  {
    "label" : "HPS 70W Ferro",
    "properties" :
      {
        "ILevel2min" : "0",
        "Vmax" : "250",
        "ILevel1min" : "100",
        "description" : "HPS 70W Ferro",
        "Ino" : "100",
        "CLOValue" : "100",
        "warmUpTime" : "10",
        "CLOBHLimit" : "0",
        "Pmax" : "70",
        "PFmin" : "0.6",
        "Vmin" : "200",
        "ILevel1max" : "1300",
        "ILevel2max" : "2000",
        "Vno" : "180",
        "name" : "/SLV/HPS/70/MAG",
        "BHmax" : "20000",
        "id" : "1",
        "lifeTime" : "20000",
        "ControlVoltMax" : "0",
        "minOutputValue" : "0",
        "Interface" : "EM"
      },
    "value" : 1
  },
  ...
]
```

To retrieve all lamp types, use the method `getAllBrands` available at `/api/asset`. This method has no required parameter.


######################################
## Retrieve all lamp type attributes
######################################

> Retrieving all lamp type attributes

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/assetmanagement/getBrandPropertyDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```


> Sample response

```python
[
 {
    "category" : "standard",
    "criticity" : 0,
    "dataFormat" :
      {
        "defaultValue" : "Manufacturer/LampTechno/Power/BallastType"
      },
    "failure" : false,
    "help" : null,
    "label" : "Identifier",
    "labelKey" : "brand.property.strId.label",
    "name" : "LampType",
    "type" : "string",
    "unit" : null
 },
 ...
]
```


To retrieve the list of all lamp type attributes available on the server, use the method `getBrandPropertyDescriptors` available at `/api/assetmanagement`. This method has no required parameter.

If an attribute has a specific set of accepted values, its *type* will be *select* and *dataFormat* will list all accepted values. 

