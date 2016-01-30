# Server-side Implementation

## Development Environment
To implement a simple server, we’ll be using Google App Engine + Python in this example. Please refer to the docs [here](https://cloud.google.com/appengine/docs/python/).

## User Model
The user model stores basic information to manage users. It has information needed for signup, chat, and a few device-related information.

```python
# models.py

class User(ndb.Model):
  email = ndb.StringProperty(required=True, indexed=True)
  nickname = ndb.StringProperty(required=True, indexed=True)
  password = ndb.StringProperty(required=True)
  session = ndb.StringProperty(required=True)
  sendbird_id = ndb.StringProperty(required=True)
  created_time = ndb.DateTimeProperty(auto_now_add=True)
  updated_time = ndb.DateTimeProperty(auto_now=True)
  device_token = ndb.StringProperty(required=True, indexed=True)
  device_type = ndb.StringProperty(required=True, choices=set(["IOS", "ANDROID", "WEB"]))
```

