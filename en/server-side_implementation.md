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

Each field are defined and used as the following:
* email

This is used for logging in. All users must have unique email addresses.

* password

 Password is used for login. In this example, we’ll use a simple SHA256 hash to store encrypted passwords.

* nickname

 Nickname is the display name used during chat. The initial value will be set to the local part of the email address. Users can change their nickname later.

* session

 Session key used between the client and the server.

* sendbird_id

 This is the key used to uniquely identify users during chat. A unique value needs to be assigned to the users. In this example, we’ll simply use the email addresses, but in production, it is recommended to use a different value.

* device_token, device_push

 Data used for push notifications. In this example, we won’t be building a push notification feature.

## User Signup Process
Let’s implement the request handler for the user signup process. Here, we’ll be accepting user data using JSON format through the body within POST request.

### API
```POST /signup```

### JSON
```json
{
  “email”: “USER@EXAMPLE.COM”,
  “nickname”: “USER_NICKNAME”,
  “password”: “USER_PASSWORD”,
  “device_token”: “DEVICE_TOKEN”,
  “device_type”: “IOS”, “ANDROID” or “WEB”
}
```
