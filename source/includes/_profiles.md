# User profiles
## The user profile
A user profile is a set of properties shared by several users: it is more commonly called a *user group*.

It is referenced by its **name**.

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

To create a user profile, you should call the method `createProfil`  available at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|profilName (required)| Name of the new profile |
|geoZoneId (required) | ID of the top-level (*root*) geozone |

## Update a user profile

> Update the user profile called *My profile* with typical profile properties

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

After creating a user profile, you will probably want to update it with some basic properties. Call the method `updateProfilProperties` at `/api/userprofile`. 

|Arguments |         |
|----------|---------|
|profilName (required)| Name of the profile to update|
|property.*key* (required) | Property to update, specified by *key*|
