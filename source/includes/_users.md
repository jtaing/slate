# Users


#############
## The user
#############


A user is defined by its login name and the user profile it belongs to. Additionnal info can be defined for a user, most importantly her email address since it is used not only for sending alarms and reports but also for the password recovery process!

Users are referenced by their login name. Below are the main attributes of a user:

|Attributes    |         |
|------------- |---------|
|login         | Login name |
|password      | Password |
|profilName    | User profile | 
|geoZoneRootId | ID of the top-level (*root*) geozone |
|contact       | Contact object (see below) |
|properties    | Array of key/value pairs storing UI configuration properties |


###############
## The contact
###############


A contact is a simple list of properties referenced by a numeric ID:

|Attributes|         |
|----------|---------|
|id       | ID |
|address  | Address |
|mail     | Email address | 
|name     | Name |
|pager    | Pager number |
|phone    | Phone number |


##################
## Create a user
##################


> Create a user belonging to the profile *My profile* with the login *johndoe* and some other example attributes

```python
import requests

payload = {
  'login': 'johndoe', 
  'password': 'Johndoe-2016',
  'firstName': 'John',
  'lastName': 'Doe',
  'profilName': 'My profile',
  'email': 'johndoe@streetlight-vision.com',
  'ser': 'json'
}

r = requests.post(SLV_CMS + "/api/userprofile/createUser", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Example response


```python
{
 "admin" : true,
 "contact" :
  {
   "address" : null,
   "id" : 11,
   "mail" : "johndoe@streetlight-vision.com",
   "name" : null,
   "pager" : null,
   "phone" : null
  },
 "firstName" : "John",
 "lastName" : "Doe",
 "login" : "johndoe",
 "password" : "",
 "profilName" : "My profile",
 "properties" :
  [
  ]
}
```

To create a user profile, you should call the method `createUser` available at `/api/userprofile`.

|Arguments |         |
|----------|---------|
|login (required)       | Login name |
|password (required)    | Password |
|profilName (required)  | User profile |
|firstName              | First name |
|lastName               | Last name |
|address                | Address |
|phone                  | Phone number |
|pager                  | Pager number |
|email                  | Email address |


######################
## Update a user
######################

> Update the email address of *johndoe*


```python
import requests

payload = {
  'login': 'johndoe', 
  'email': 'johndoe-new@streetlight-vision.com',
  'ser': 'json'
}

r = requests.post(SLV_CMS + "/api/userprofile/updateUser", data=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response


```python
"OK"
```

To update any of the attribute set when creating a user with `createUser` (except the user password), you may use the method `updateUser` available at `/api/userprofile`.

It takes the same parameters:

|Arguments |         |
|----------|---------|
|login (required)       | Login name |
|profilName             | User profile |
|firstName              | First name |
|lastName               | Last name |
|address                | Address |
|phone                  | Phone number |
|pager                  | Pager number |
|email                  | Email address |


###########################
## Update user UI properties
###########################


> Update two dummy properties *property1* and *property2* for the user *johndoe*


```python
import requests
from webob.multidict import MultiDict

payload = MultiDict([
  ('userLogin', 'johndoe'), 
  ('propertyName', 'property1'),
  ('propertyValue', 'value1'),
  ('propertyName', 'property2'),
  ('propertyValue', 'value2'),
  ('ser', 'json')
])

r = requests.post(SLV_CMS + "/api/userprofile/updateUserProperty", data=payload, auth=('USERNAME', 'PASSWORD'))
```

> Example response


```python
"OK"
```


Since user UI properties are actually an array of key/value pairs, a specific method is provided to update them: `updateUserProperty` available at `/api/userprofile`. It takes an arbitrary number of key/value pairs.

|Arguments |         |
|----------|---------|
|userLogin (required)    | Login name |
|propertyName            | Property key |
|propertyValue           | Property value |


###########################
## Retrieve the current user
###########################

> Example request

```python
import requests

payload = {
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/userprofile/getCurrentUser", params=payload, auth=('USERNAME', 'PASSWORD'))

```

> Example response

```python
{
 "admin" : true,
 "contact" :
  {
     "address" : null,
     "id" : 11,
     "mail" : "johndoe-new@streetlight-vision.com",
     "name" : null,
     "pager" : null,
     "phone" : null
  },
 "firstName" : "John",
 "lastName" : "Doe",
 "login" : "johndoe",
 "password" : "",
 "profilName" : "My profile",
 "properties" :
  [
   {
      "key" : "property2",
      "value" : "value2"
   },
   {
      "key" : "property1",
      "value" : "value1"
   }
  ]
}
```

To retrieve the attributes of the current user, call `getCurrentUser` at `/api/userprofile`. This method has no required parameter.


#######################
## Retrieve all users 
#######################

> Get all users under the geozone ID 1 and all of its sub-geozones

```python
import requests

payload = {
  'geoZoneId': 1,
  'recurse': 'true',
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/userprofile/getGeoZoneUsers", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Example response

```python
[
 {
  "admin" : true,
  "contact" :
   {
      "address" : null,
      "id" : 11,
      "mail" : "johndoe-new@streetlight-vision.com",
      "name" : null,
      "pager" : null,
      "phone" : null
   },
  "firstName" : "John",
  "lastName" : "Doe",
  "login" : "johndoe",
  "password" : "",
  "profilName" : "My profile",
  "properties" :
   [
    {
       "key" : "property2",
       "value" : "value2"
    },
    {
       "key" : "property1",
       "value" : "value1"
    }
   ]
 },
 ...
]

```


To obtain the list of all users whose user profile is under a specific geozone , call `getGeoZoneUsers` at `/api/userprofile`.

|Arguments |         |
|----------|---------|
|geoZoneId (required)| Desired top-level geozone|
|recurse | If set to true, this method will also return the users whose user profile have a top-level geozone being a child of the geozone specified by `geoZoneId`. Set to *false* if not specified. |

#################
## Delete a user
#################

> Delete the user *test42*

```python
import requests

payload = {
  'login': 'test42', 
  'ser': 'json'
}

r = requests.post(SLV_URL + "/api/userprofile/deleteUser", data=payload, auth=('USERNAME', 'PASSWORD'))
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


To delete a user, call the method `deleteUser` available at `/api/userprofile`. Note that if you try to delete a user that is not visible to you, you will get an error.

|Arguments |         |
|----------|---------|
|login (required)    | Login name |


###################################
## Update the current user password
###################################

> Set the password to *newPass-1*

```python
import requests

payload = {
  'previousPassword': 'PASSWORD',
  'newPassword': 'newPass-1',
  'ser': 'json'
}

r = requests.post(SLV_CMS + "/api/userprofile/changePassword", data=payload, auth=('USERNAME', 'PASSWORD'))
```


> Example response

```python
"OK"
```


There is a specific method to update the current user password as the old password is required before a new password can be set. The method `changePassword` at `/api/userprofile` must be called to change a password. It only applies to the user who is currently logged in.

|Arguments |         |
|----------|---------|
|previousPassword (required)    | Previous password |
|newPassword (required)         | New password |

<aside class="warning">
You will get an error if the new password does not follow the password complexity rules set by the server.
</aside>


##############################
## Recover a forgotten password
##############################

> Ask the server to initiate the password recovery process for *johndoe*

```python
import requests

payload = {
  'login': 'johndoe',
  'ser': 'json'
}

r = requests.post(SLV_URL + "/public/api/publicconfig/sendResetPasswordRequestByMail", data=payload)
```

> Example response

```python
"OK"
```

To trigger the password recovery mechanism, call the method `sendResetPasswordRequestByMail` available at `/public/api/publicconfig`. This method does not require any authentication!

|Arguments |         |
|----------|---------|
|login (required)    | Login name |
