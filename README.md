# MoesifApi Lib for NodeJS


[Source Code on GitHub](https://github.com/moesif/moesifapi-nodejs)

[Package on NPMJS](https://www.npmjs.com/package/moesifapi)

__Check out Moesif's [Developer Documentation](https://www.moesif.com/docs) and [Node API Reference](https://www.moesif.com/docs/api?javascript) to learn more__


## How to install

```shell
npm install moesifapi
```

## How to use

(See test/ApiControllerTest.js for more usage examples)

### Create a single API event

```javascript
// 1. Import the module
var moesifapi = require('moesifapi');
var api = moesifapi.ApiController;

// 2. Configure the ApplicationId
var config = moesifapi.configuration;
config.ApplicationId = "my_application_id";

// 3. Generate an API Event Model
var reqHeaders = JSON.parse('{' +
        '"Host": "api.acmeinc.com",' +
        '"Accept": "*/*",' +
        '"Connection": "Keep-Alive",' +
        '"User-Agent": "Dalvik/2.1.0 (Linux; U; Android 5.0.2; C6906 Build/14.5.A.0.242)",' +
        '"Content-Type": "application/json",' +
        '"Content-Length": "126",' +
        '"Accept-Encoding": "gzip"' +
    '}');

var reqBody = JSON.parse( '{' +
        '"items": [' +
            '{' +
                '"type": 1,' +
                '"id": "fwfrf"' +
            '},' +
            '{' +
                '"type": 2,' +
                '"id": "d43d3f"' +
            '}' +
        ']' +
    '}');

var rspHeaders = JSON.parse('{' +
        '"Date": "Tue, 23 Jan 2017 23:46:49 GMT",' +
        '"Vary": "Accept-Encoding",' +
        '"Pragma": "no-cache",' +
        '"Expires": "-1",' +
        '"Content-Type": "application/json; charset=utf-8",' +
        '"Cache-Control": "no-cache"' +
    '}');

var rspBody = JSON.parse('{' +
        '"Error": "InvalidArgumentException",' +
        '"Message": "Missing field field_a"' +
    '}');

var eventReq = {
    time: new Date(),
    uri: "https://api.acmeinc.com/items/reviews/",
    verb: "PATCH",
    apiVersion: "1.1.0",
    ipAddress: "61.48.220.123",
    headers: reqHeaders,
    body: reqBody
};

var eventRsp = {
    time: new Date(),
    status: 500,
    headers: rspHeaders,
    body: rspBody
};

var eventModel = {
    request: eventReq,
    response: eventRsp,
    userId: "my_user_id",
    sessionToken: "23jdf0owekfmcn4u3qypxg09w4d8ayrcdx8nu2ng]s98y18cx98q3yhwmnhcfx43f",
    metadata: {
      foo: 'abc',
      bar: 'efg'
    }
};

// 4. Create a single event
api.createEvent(new EventModel(eventModel), function(error, response, context) {
  // Do Something
});
```

### Create a batch of API events

You can also create a batch of events at once by sending a list of events.

```javascript
// 1. Import the module
var moesifapi = require('moesifapi');
var api = moesifapi.ApiController;

// 2. Configure the ApplicationId
var config = moesifapi.configuration;
config.ApplicationId = "my_application_id";

// 3. Generate an API Event Model
var reqHeaders = JSON.parse('{' +
        '"Host": "api.acmeinc.com",' +
        '"Accept": "*/*",' +
        '"Connection": "Keep-Alive",' +
        '"User-Agent": "Dalvik/2.1.0 (Linux; U; Android 5.0.2; C6906 Build/14.5.A.0.242)",' +
        '"Content-Type": "application/json",' +
        '"Content-Length": "126",' +
        '"Accept-Encoding": "gzip"' +
    '}');

var reqBody = JSON.parse( '{' +
        '"items": [' +
            '{' +
                '"type": 1,' +
                '"id": "fwfrf"' +
            '},' +
            '{' +
                '"type": 2,' +
                '"id": "d43d3f"' +
            '}' +
        ']' +
    '}');

var rspHeaders = JSON.parse('{' +
        '"Date": "Tue, 25 Feb 2017 23:46:49 GMT",' +
        '"Vary": "Accept-Encoding",' +
        '"Pragma": "no-cache",' +
        '"Expires": "-1",' +
        '"Content-Type": "application/json; charset=utf-8",' +
        '"Cache-Control": "no-cache"' +
    '}');

var rspBody = JSON.parse('{' +
        '"Error": "InvalidArgumentException",' +
        '"Message": "Missing field field_a"' +
    '}');

var eventReq = {
    time: "2017-02-25T04:45:42.914",
    uri: "https://api.acmeinc.com/items/reviews/",
    verb: "PATCH",
    apiVersion: "1.1.0",
    ipAddress: "61.48.220.123",
    headers: reqHeaders,
    body: reqBody
};

var eventRsp = {
    time: "2016-09-09T04:45:42.914",
    status: 500,
    headers: rspHeaders,
    body: rspBody
};

var eventModel = {
    request: eventReq,
    response: eventRsp,
    userId: "my_user_id",
    sessionToken: "23jdf0owekfmcn4u3qypxg09w4d8ayrcdx8nu2ng]s98y18cx98q3yhwmnhcfx43f"
};

var events = [new EventModel(eventModel),
  new EventModel(eventModel),
  new EventModel(eventModel),
  new EventModel(eventModel)];

// 4. Send batch of events
api.createEventsBatch(events, function(error, response, context) {
  // Do Something
});
```

### Update an end user

Updating an end user will create one if it does not exist,
also know as [upsert](https://en.wikipedia.org/wiki/Merge_(SQL))
The only __required__ field is `user_id`.

```javascript
// 1. Import the module
var moesifapi = require('moesifapi');
var api = moesifapi.ApiController;

// 2. Configure the ApplicationId
var config = moesifapi.configuration;
config.ApplicationId = "my_application_id";

// 3. Generate a User Model
var user = {
    userId: "my_user_id",
    sessionToken: "23jdf0owekfmcn4u3qypxg09w4d8ayrcdx8nu2ng]s98y18cx98q3yhwmnhcfx43f",
    metadata: {
      email: "johndoe@acmeinc.com",
      string_field: "value_1",
      number_field: 0,
      object_field: {
        field_a: "value_a",
        field_b: "value_b"
      }
    }
};

// 4. Create a single user
api.updateUser(new UserModel(user), function(error, response, context) {
  // Do Something
});
```

### Update a batch of end users

Will update all users in a single batch, useful for saving from offline sources like CSV.
Any user that does not exist will be created, also known as [upsert](https://en.wikipedia.org/wiki/Merge_(SQL))
The only __required__ field is `user_id`.

```javascript
// 1. Import the module
var moesifapi = require('moesifapi');
var api = moesifapi.ApiController;

// 2. Configure the ApplicationId
var config = moesifapi.configuration;
config.ApplicationId = "my_application_id";

// 3. Generate an API Event Model
var userA = {
    userId: "12345",
    sessionToken: "23jdf0owekfmcn4u3qypxg09w4d8ayrcdx8nu2ng]s98y18cx98q3yhwmnhcfx43f",
    metadata: {
      email: "johndoe@acmeinc.com",
      string_field: "value_1",
      number_field: 0,
      object_field: {
        field_a: "value_a",
        field_b: "value_b"
      }
    }
};

var userB = {
    userId: "6789",
    sessionToken: "23jdf0oszfexfqe[lwjfiefovprewv4d8ayrcdx8nu2ng]zfeeadedefx43f",
    metadata: {
      email: "maryjane@acmeinc.com",
      string_field: "value_1",
      number_field: 1,
      object_field: {
        field_a: "value_a",
        field_b: "value_b"
      }
    }
};

var users = [new UserModel(userA), new UserModel(userB)];

// 4. Send batch of events
api.updateUsersBatch(users, function(error, response, context) {
  // Do Something
});
```

## How To Test:

````shell
git clone https://github.com/moesif/moesifapi-nodejs
cd moesifapi-nodejs
npm install --global mocha
mocha
```
