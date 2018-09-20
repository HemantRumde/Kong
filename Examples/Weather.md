# Weather API Tutorial

## Add API
```
curl -i -X POST http://localhost:8001/apis/ \
  -d 'name=openweathermap.org' \
  -d 'upstream_url=http://api.openweathermap.org/data/2.5' \
  -d "methods=GET,HEAD"
```
> Check the newly added API. Note its ID to set plugin for this new API
```
curl -X GET http://localhost:8001/apis
{
   "total":2,
   "data":[
      {
         "created_at":1537478170168,
         "strip_uri":true,
         "id":"628e208c-343f-4c78-9e70-b08a7740229d",
         "name":"openweathermap.org",
         "http_if_terminated":false,
         "https_only":false,
         "upstream_url":"http:\/\/api.openweathermap.org\/data\/2.5",
         "methods":["GET","HEAD"],
         "preserve_host":false,
         "upstream_connect_timeout":60000,
         "upstream_read_timeout":60000,
         "upstream_send_timeout":60000,
         "retries":5
       }
   ]
 }
```
## Authentication plugin for API
Use <code>id</code> of newly created API to attach a plugin. Following command adds key authentication plugin
```
APID="628e208c-343f-4c78-9e70-b08a7740229d"
curl -X POST http://localhost:8001/apis/$APID/plugins \
--data "name=key-auth"
```

## Add New consumer member 
```
curl -X POST http://localhost:8001/consumers \
     --data "username=hemant" --data "custom_id=38027"
```
Check the newly added member 
```
curl -X GET http://localhost:8001/consumers
{"next":null,"data":[{"custom_id":"38027","created_at":1537479999,"username":"hemant","id":"52e9c735-8dd8-4413-92eb-661eb1224c99"}]}
```
## Generate a key for newly added member
```
curl -X POST http://localhost:8001/consumers/hemant/key-auth --data ""
```
## Consume API with Authentication key
```
APID=58495e290bb67690b8c7d8a31727126d
KEY=qRivFprQTrd32nnSdW2l40s7wAKS6yEF

curl -v 'http://localhost:8000/weather?q=Mumbai&APPID=58495e290bb67690b8c7d8a31727126d' \
--header 'Host: api.openweathermap.org' \
--header 'apikey: qRivFprQTrd32nnSdW2l40s7wAKS6yEF'
```
