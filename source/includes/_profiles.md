# User profiles

## The user profile

A user profile is a set of properties shared by several users: it is more commonly called a *user group*.

It is referenced by its **name**. Below are its main attributes:

|Attributes||
|----------|---------|
|name | Profile name |
|blockedActions | List of actions that cannot be performed |
|locale | Language following the format `lg_CN`. `lg` is the ISO-639 language code, `CN` is the ISO-3166 country code. | 
|geoZoneRootId | ID of the top-level (*root*) geozone |
|skin | Name of the skin|



## Create a user profile

> Create a new user profile called *My profile* and having access to the geozone ID 1

```python
import requests

payload = {
  'profilName': 'My profile', 
  'geoZoneId': '1',
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/createProfil", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Example response

```python
{
   "name" : "My profile",
   "properties" :
      [
         {
            "key" : "css",
            "value" : "/resources/css/default.css"
         },
         {
            "key" : "skin",
            "value" : "streetlight"
         },
         {
            "key" : "img.bottombanner",
            "value" : "/resources/img/bottomBar.gif"
         },
         {
            "key" : "providerRootId",
            "value" : ""
         },
         {
            "key" : "locale",
            "value" : "en_US"
         },
         {
            "key" : "img.topbanner",
            "value" : "/resources/img/topbanner_left.gif"
         },
         {
            "key" : "customerRootId",
            "value" : ""
         },
         {
            "key" : "desktop",
            "value" : "desktopApplicationDefault.xml"
         },
         {
            "key" : "geoZoneRootId",
            "value" : "1"
         },
         {
            "key" : "maintenanceAccessFromDeviceCard",
            "value" : "false"
         },
         {
            "key" : "hilitable",
            "value" : "/resources/js/hilitableTable.js"
         },
         {
            "key" : "blockedActions",
            "value" : ""
         },
         {
            "key" : "hide.topbanner",
            "value" : "true"
         },
         {
            "key" : "hide.bottombanner",
            "value" : "true"
         },
         {
            "key" : "userType",
            "value" : "unknown"
         },
         {
            "key" : "img.topcenterbanner",
            "value" : "/resources/img/topbanner_center.gif"
         },
         {
            "key" : "img.toprightbanner",
            "value" : "/resources/img/topbanner_right.gif"
         }
      ]
}
```

To create a user profile, you should call the method `createProfil`  available at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|profilName (required)| Name of the new profile |
|geoZoneId (required) | ID of the top-level (*root*) geozone |


## Update a user profile

> Update the user profile called *My profile* with typical profile attributes

```python
import requests

payload = {
  'profilName': 'My profile', 
  'property.locale': 'en_US',
  'property.skin': 'streetlight',
  'property.geoZoneRootId': 1,
  'property.blockedActions': 'SLVAssetManagementAPI!deleteGeoZone,SLVAssetManagementAPI!deleteDevice',
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/updateProfilProperties", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Example response

```python
"OK"
```

After creating a user profile, you will probably want to update its attributes. Call the method `updateProfilProperties` at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|profilName (required)| Name of the profile to update|
|property.*key* (required) | Attribute to update, specified by *key*|


## Delete a user profile


> Delete the user profile called *My profile*


```python
import requests

payload = {
  'profilName': 'My profile', 
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/deleteProfil", params=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response


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

To delete a user profile, call the method `deleteProfil` at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|profilName (required)| Name of the profile to delete|

<aside class="warning">
You cannoy delete a user profile that is attached to an existing user.
</aside>


## Retrieve the profile of the current user


> Get the current user profile


```python
import requests

payload = {
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/getCurrentProfil", params=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response


```python
{
   "name" : "My profile",
   "properties" :
      [
         {
            "key" : "css",
            "value" : "/resources/css/default.css"
         },
         {
            "key" : "skin",
            "value" : "streetlight"
         },
         {
            "key" : "img.bottombanner",
            "value" : "/resources/img/bottomBar.gif"
         },
         {
            "key" : "providerRootId",
            "value" : ""
         },
         {
            "key" : "locale",
            "value" : "en_US"
         },
         {
            "key" : "img.topbanner",
            "value" : "/resources/img/topbanner_left.gif"
         },
         {
            "key" : "customerRootId",
            "value" : ""
         },
         {
            "key" : "desktop",
            "value" : "desktopSilverlight.xml"
         },
         {
            "key" : "geoZoneRootId",
            "value" : "1"
         },
         {
            "key" : "maintenanceAccessFromDeviceCard",
            "value" : "false"
         },
         {
            "key" : "hilitable",
            "value" : "/resources/js/hilitableTable.js"
         },
         {
            "key" : "blockedActions",
            "value" : ""
         },
         {
            "key" : "hide.topbanner",
            "value" : "true"
         },
         {
            "key" : "hide.bottombanner",
            "value" : "true"
         },
         {
            "key" : "userType",
            "value" : "unknown"
         },
         {
            "key" : "img.topcenterbanner",
            "value" : "/resources/img/topbanner_center.gif"
         },
         {
            "key" : "img.toprightbanner",
            "value" : "/resources/img/topbanner_right.gif"
         }
      ]
}
```


To know the attributes of the current user profile, call `getCurrentProfil` at `/api/userprofile`. This method has no required parameter.


## Retrieve all profiles


> Get all user profiles available at the geozone ID 1 and all of its sub-geozones 


```python
import requests

payload = {
  'geoZoneId': 1,
  'recurse': 'true',
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/getGeoZoneProfils", params=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response


```python
[
   {
      "name" : "My profile",
      "properties" :
         [
            {
               "key" : "css",
               "value" : "/resources/css/default.css"
            },
            {
               "key" : "skin",
               "value" : "streetlight"
            },
            {
               "key" : "img.bottombanner",
               "value" : "/resources/img/bottomBar.gif"
            },
            {
               "key" : "providerRootId",
               "value" : ""
            },
            {
               "key" : "locale",
               "value" : "en_US"
            },
            {
               "key" : "img.topbanner",
               "value" : "/resources/img/topbanner_left.gif"
            },
            {
               "key" : "customerRootId",
               "value" : ""
            },
            {
               "key" : "desktop",
               "value" : "desktopSilverlight.xml"
            },
            {
               "key" : "geoZoneRootId",
               "value" : "1337"
            },
            {
               "key" : "maintenanceAccessFromDeviceCard",
               "value" : "false"
            },
            {
               "key" : "hilitable",
               "value" : "/resources/js/hilitableTable.js"
            },
            {
               "key" : "blockedActions",
               "value" : ""
            },
            {
               "key" : "hide.topbanner",
               "value" : "true"
            },
            {
               "key" : "hide.bottombanner",
               "value" : "true"
            },
            {
               "key" : "userType",
               "value" : "unknown"
            },
            {
               "key" : "img.topcenterbanner",
               "value" : "/resources/img/topbanner_center.gif"
            },
            {
               "key" : "img.toprightbanner",
               "value" : "/resources/img/topbanner_right.gif"
            }
         ]
   },
   {
   ...
   }
]
```

You can obtain the list of all user profiles with a specific top-level geozone using the method `getGeoZoneProfils` at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|geoZoneId (required)| Desired top-level geozone|
|recurse | If set to true, this method will also return the user profiles that have a top-level geozone being a child of the geozone specified by `geoZoneId`. Set to *false* if not specified. |

<aside class="warning">
You will get an error if you are using a `geoZoneId` that you do not have access to.
</aside>


## Retrieve all user profile attributes


> Example request


```python
import requests

payload = {
  'ser': 'json'
}
r = requests.get(SLV_URL + "/api/userprofile/getProfilPropertyDescriptors", params=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response

```python
[
   {
      "dataFormat" : null,
      "enumValues" :
         [
            {
               "label" : "Unknown",
               "properties" : null,
               "value" : "unknown"
            },
            {
               "label" : "Human",
               "properties" : null,
               "value" : "human"
            },
            {
               "label" : "Controller",
               "properties" : null,
               "value" : "controller"
            }
         ],
      "format" : null,
      "label" : "??profil.property.name.userType??",
      "labelKey" : "profil.property.name.userType",
      "name" : "userType",
      "type" : "enum.single"
   },
   {
     ...
   }  
]
```


To obtain the list of all user profile attributes available, use the method `getProfilPropertyDescriptors` at `/api/userprofile`. It has no required parameter.

If an attribute has a specific set of accepted values, its *type* will be *enum.single* and *enumValues* will list all accepted values. 

<aside class="notice">
`blockedActions` is a specific case: despite its type being `enum.single`, it actually accepts several values that are separated by a comma.
</aside>