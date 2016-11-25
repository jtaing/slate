# Authentication

Authentication to the API is performed via either [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication) or [Form-based Auth](https://en.wikipedia.org/wiki/Form-based_authentication). 

Your SLV CMS credentials serve as API credentials as well. They are represented in the code examples by `USERNAME` and `PASSWORD`.

API requests without authentication will fail with some random-looking HTML content.

## Basic

> To authenticate against an SLV CMS using HTTP Basic Auth, use the following code:

```python
import urllib.request

# This code assumes that all requests will be made with urllib.request

auth_handler = urllib.request.HTTPBasicAuthHandler()
auth_handler.add_password(realm="Authentication required",
                          uri=HOST,
                          user=USERNAME,
                          passwd=PASSWORD)
opener = urllib.request.build_opener(auth_handler)
urllib.request.install_opener(opener)
```

```shell
# With shell, you can just pass the username with each request
curl "api_endpoint_here"
  -u USERNAME:PASSWORD
```

Depending on your programmaing language, you may have to specify a URI or hostname. In the Python example, it is represented by the `HOST` parameter. `HOST is the IP address or domain name of the SLV CMS. Make sure to add the port (separated by a colon) when it is different from 80 and 443, e.g. *mydomain.com:8080*

## Form 

> To authenticate against an SLV CMS using Form-based Auth, use the following code:

```python
import urllib.request
import urllib.parse
import json

def login():
  # Make a first request to auth.json to get a cookie from the CMS
  urllib.request.urlopen(SLV_URL + "/auth.json")
  
  # Send the credentials to the CMS
  credentials = urllib.parse.urlencode({'j_username':USERNAME,'j_password':PASSWORD})
  try:
    urllib.request.urlopen(SLV_URL + "/j_security_check", data=credentials.encode("utf-8"))
  except urllib.error.HTTPError as err:
    print ("Unable to log in: {0}".format(err))

def is_authenticated():
  authenticated = False
  response = urllib.request.urlopen(SLV_URL + "/auth.json").read().decode("utf-8")
  try:
    jsonresponse = json.loads(response)
  except ValueError:
    return False
  if jsonresponse["authenticated"] == "true" :
    authenticated = True
  return authenticated

# Enable cookie saving
opener = urllib.request.build_opener(urllib.request.HTTPCookieProcessor)
urllib.request.install_opener(opener)

login()
if is_authenticated() == False:
  print("Wrong crendentials")
  quit()
else:
  print("Logged in sucessfully")
```

```shell
# Replace SLV_URL by the full URL of the SLV CMS
curl -c cookie.txt "SLV_URL/auth.json" 
curl -b cookie.txt -c cookie.txt -d "j_username=USERNAME" -d "j_password=PASSWORD" "SLV_URL/j_security_check"
curl -b cookie.txt -c cookie.txt "SLV_URL/auth.json"

# Use the same cookie in all subsequent requests
curl -b cookie.txt "api_endpoint_here"
```

In Form-based Auth, you have to specify the full URL of the SLV CMS, e.g. http://mydomain.com:8080/reports. This is represented by the parameter `SLV_URL` in the code examples. 

There are three steps to form-based auth:
1. Make a first request to `SLV_URL/auth.json`. This allows you to get a cookie from the SLV CMS
2. Make a request to `SLV_URL/j_security_check` along with your credentials
3. Make a second request to `SLV_URL/auth.json`. When authenticated successfully, it will return `{"authenticated":"true"}`