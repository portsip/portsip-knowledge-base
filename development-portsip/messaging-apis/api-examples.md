# API Examples

In this article, we will provide examples in Python to demonstrate how the client application interacts with the PortSIP PBX IM service. For these examples, we assume the PBX server is hosted at `https://pbx.portsip.com:8887`.

## Connecting IM Server

The client application needs to use WebSocket to connect to the URL `wss://pbx.portsip.com:8887/im`.

```python
ssl_context = ssl.create_default_context()  
ssl_context.check_hostname = False  
ssl_context.verify_mode = ssl.CERT_NONE
websocket = await websockets.connect(uri, ssl = ssl_context)
```

## Authentication

Once the connection to the IM server is successfully established, the client must authenticate with the server in order to send and receive messages.

### Obtain Access Token

The IM service authenticates the client application using the PortSIP PBX access token. Please follow the instructions in the article "[PortSIP PBX REST API Authentication](../rest-apis/authentication.md)" to obtain the token.

### Authenticate with IM Service using the Access Token

Once the access token is obtained, you can authenticate with the IM service by sending the `login` command to the server.

```python
auth = str(user) + "@" + domain + ":" + access_token
secret = base64.b64encode(auth.encode())
uuid4 = uuid.uuid4()
message = {
    "login": {
    "id": str(uuid4),
    "scheme": "access_token",
    "secret": secret.decode()
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("client_auth  response : " + response)
ret = json.loads(response)
if ret["ctrl"]["code"] == 200 :
    return ret["ctrl"]["params"]["user"]
else :
    logger.error("client_auth [" + str(user) + "] retcode not 200  response : " + response) 
return ""
```

If authentication is successful, the server will respond as shown below.

{% code overflow="wrap" %}
```json
{
    "ctrl": {
        "id": "fef15fa2-3e27-49ec-9286-50279d67cde3",
        "params": {
            "authlvl": "auth",
            "token": "AAAAhnJBgwwAAAAvbEGDDB7R+mgUAAAAAQAC9wKOmcHTClrJc8wtV5MD7AIx0LTN5iy5E901p+EBgg==",
            "user": "usr804252613548777777"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:06:38.924626Z"
    }
}
```
{% endcode %}

* `["ctrl"]["params"]["user"]`: Represents the user ID.
* `["ctrl"]["params"]["token"]`: Represents the user's authentication token, which can be used for verifying permissions in future HTTP file uploads to the IM server.

## Subscribe to P2P Topic

P2P topics represent a communication channel between two users, and these topics do not have an owner.

The topic IDs for the two participating users are different. Each user views the other user’s ID as the P2P topic ID. For example, if the two users in the topic are `usr804252613548703744` and `usr804252613548777777`, the first user will see the topic ID as `usr804252613548777777`, while the second user will see the topic ID as `usr804252613548703744`.

```python
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "usr804252613548703744"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
```

## Sending Message to P2P Topic

You can send a message to a P2P topic for another user using the following code snippet. The `["pub"]["id"]` is a UUID that the client application must generate. The server will return this ID in its response, allowing the client to track the message by this ID.

```python
uuid4 = uuid.uuid4()
message = {
    "pub": {
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "noecho": False,
        "content": "test test test"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("pub_message to : usr804252613548703744 response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("pub_message  retcode error  response : " + response)
```

The response will look like the following:

```json
{
    "ctrl": {
        "id": "99c5075a-cf52-493c-8572-9e30437a02ce",
        "topic": "usr804252613548703744",
        "params": {
            "seq": 377
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T08:24:41.913558Z"
    }
}

```

The user with the ID `804252613548777777` will receive the message as shown below:

```json
{
    "data": {
        "topic": "usr804252613548703744",
        "from": "usr804252613548777777",
        "ts": "2024-10-24T08:24:41.913558Z",
        "seq": 377,
        "content": "test test test",
        "reqid": "99c5075a-cf52-493c-8572-9e30437a02ce"
    }
}
```

## Get the Topics I Have Subscribed To

You can use the following code to retrieve the topics you have subscribed to.

```python
uuid4 = uuid.uuid4()
message = {
    "get": {
        "sub": {
            "ims": "2024-01-03T07:38:56.304Z"
         },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "sub"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
 
```

The response will look like the following:

```json
{
    "meta": {
        "id": "362c603e-4343-40a6-ab32-4e4248171d89",
        "topic": "usr804252613548703744",
        "ts": "2024-10-24T01:34:48.960092Z",
        "sub": [
            {
                "updated": "2024-10-24T01:04:20.272656Z",
                "acs": {
                    "want": "JRWPA",
                    "given": "JRWPAS",
                    "mode": "JRWPA"
                },
                "user": "usr804252613548703744"
            },
            {
                "updated": "2024-10-24T01:30:49.077521Z",
                "acs": {
                    "want": "JRWPA",
                    "given": "JRWPA",
                    "mode": "JRWPA"
                },
                "read": 367,
                "recv": 367,
                "user": "usr804252613548777777"
            }
        ]
    }
}
```

## Query Topic Details

You can use the following message `{get what="desc"}` to query the details of a topic. For example, group-related information can be found within the details of a group topic.

```
uuid4 = uuid.uuid4()
message = {
    "get": {
        "desc": {
            "ims": "2024-01-03T07:38:56.304Z"
         },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "desc"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
```

The response will look like the following:

```json
{
    "meta": {
        "id": "926c7203-0ff7-4241-b9d2-0b46a5e625c6",
        "topic": "usr804252613548703744",
        "ts": "2024-10-24T01:30:45.144013Z",
        "desc": {
            "created": "2024-10-24T01:04:20.286287Z",
            "updated": "2024-10-24T01:26:51.198007Z",
            "touched": "2024-10-24T01:26:51.197597Z",
            "acs": {
                "want": "JRWPA",
                "given": "JRWPA",
                "mode": "JRWPA"
            },
            "seq": 305,
            "read": 305,
            "recv": 305
        }
    }
}
```

## Obtain Historical Messages

By using the `["meta"]["desc"]["seq"]` field returned from the `{get what="desc"}` message, you can retrieve the number of messages in the subscribed topic. You can then use `{get what="data"}` to fetch historical messages.

```python
uuid4 = uuid.uuid4()
message = {
    "get": {
        "data": {
            "before": 10000,
            "limit": 1000,
            "since": 0
        },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "data"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("sub_message to usr804252613548703744 response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("sub_message  retcode error  response : " + response)
while IS_RUNNING:
    try:
        message = await asyncio.wait_for(websocket.recv(), timeout=SENDER_SLEEP)
        logger.debug("sub_message  : " + message)
        ret = json.loads(message)
        if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
            logger.error("sub_message  retcode error  response : " + message)
        if 'data' in ret:
            //recived msg
    except asyncio.TimeoutError:
        break
    except ConnectionClosedError as e:
        logger.error("Connection closed unexpectedly : " + str(e))
        break

```

## Creating a Group Chat

You can use the following message to create a group chat.

```python
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "new"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("group_create  response : " + response)
ret = json.loads(response)
if ret["ctrl"]["code"] == 200 :
    return ret["ctrl"]["topic"]
else :
    logger.error("group_create retcode not 200  response : " + response) 
return ""
```

The response will look like the following, the `["ctrl"]["topic"]` represents the group topic. Then other users can subscribe to group messages by sending `{sub topic="grpf0NlkdDEQHs"}`.

```json
{
    "ctrl": {
        "id": "06679cbc-b1d8-4821-a71f-44c1229e350e",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "acs": {
                "want": "JRWPASDO",
                "given": "JRWPASDO",
                "mode": "JRWPASDO"
            },
            "tmpname": "new"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:12:11.153147Z"
    }
}

```

## Join a Group

The user can send the following message to subscribe to the group topic, allowing them to join the group and send or receive messages within the group.

```
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("sub to : grpf0NlkdDEQHs response : " + response)
```

The response will look like the following:

```json
{
    "ctrl": {
        "id": "9c1c98b5-5123-41be-a6b9-2fe03877b946",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "seq": 1
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T07:04:01.052963Z"
    }
}

```

## Sending a Message to a Group

You can use the following code to send a message to a group.

```python
uuid4 = uuid.uuid4()
message = {
    "pub": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "noecho": False,
        "content": "test test test"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("pub_message to : grpf0NlkdDEQHs response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("pub_message  retcode error  response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "c1dd7a45-93db-4af9-870d-10643222b7e7",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "seq": 2
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T07:04:01.120034Z"
    }
}
```

The following message will be sent to all users who have subscribed to the group topic.

```json
{
    "data": {
        "topic": "grpf0NlkdDEQHs",
        "from": "usr901636310534455296",
        "ts": "2024-10-24T07:04:01.120034Z",
        "seq": 2,
        "content": "test test test",
        "reqid": "c1dd7a45-93db-4af9-870d-10643222b7e7"
    }
}
```

## Leave From a Group

Users can unsubscribe from a group by sending a `{leave}` message. This is the opposite function of the `{sub}` message. The `{leave}` message supports two options:

* **Leave without unsubscribing** (`unsub=false`)
* **Unsubscribe and leave** (`unsub=true`)

The server will respond to a `{leave}` message with a `{ctrl}` packet.

* **Leave without unsubscribing** (`unsub=false`) only affects the current WebSocket session. You will still receive messages if you establish a new WebSocket session with the IM server, and if the user is connected to the IM server from another app, they will also continue receiving messages.
* **Unsubscribe and leave** (`unsub=true`) affects all sessions for that user. The user will no longer receive messages from this group across any session unless he subscribes to this group topic again.

```python
uuid4 = uuid.uuid4()
message = {
    "leave": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs"
        "unsub": True 
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("leave to : grpf0NlkdDEQHs response : " + response)
```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grp5XUxk__lpVY",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

## Remove a Member from the Group

The group administrator can remove a member from the group by sending the following message:

```python
uuid4 = uuid.uuid4()
message = {
    "del": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "user": "usr804252613548777777",
        "what": "sub"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("del usr804252613548777777 from grpf0NlkdDEQHs response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grpf0NlkdDEQHs",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

## Delete a Group

The group administrator can delete a group by sending the following message:

```python
uuid4 = uuid.uuid4()
message = {
    "del": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "what": "topic"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("del grpf0NlkdDEQHs response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grpf0NlkdDEQHs",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

## Update Group Information

The group administrator can update the group information by sending the following message. The information structure is defined in the [**Public** **attributes**](protocol.md#public) of the group topic.

```python
uuid4 = uuid.uuid4()
message =  {
    "set": {
    "id":str(uuid4),
        "topic":"grpf0NlkdDEQHs",
        "desc": {
            "public": {
                "fn": "My group"
            }
        }
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("group_set_test  response : " + response)
while True:
    try:
        message = await asyncio.wait_for(websocket.recv(), timeout=SENDER_SLEEP)
        logger.debug("group_set_test  response : " + message)
    except asyncio.TimeoutError:
        break
    except ConnectionClosedError as e:
        logger.error("Connection closed unexpectedly : " + str(e))
        break

```

The response will look like the following if succeeds:

```json
{
  "ctrl": {
    "id": "83da454f-dad7-4bf0-b6d2-e32e104be8f3",
    "topic": "grpf0NlkdDEQHs",
    "code": 200,
    "text": "ok",
    "ts": "2024-10-25T03:53:20.337069Z"
  }
}
```

## Transfer Group Ownership

The group administrator can transfer ownership of the group to another user by sending the following message. In this example, `usr804252613548777777` indicates that the group ownership will be transferred to the user with the ID `804252613548777777`.

```python
uuid4 = uuid.uuid4()
message = {
    "set":{
        "id":str(uuid4),
        "topic":"grpf0NlkdDEQHs",
        "sub":{
            "mode":"JRWPASDO",
            "user":"usr804252613548777777"
        }
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("group_transfer_test  response : " + response)
while True:
    try:
        message = await asyncio.wait_for(websocket.recv(), timeout=SENDER_SLEEP)
        logger.debug("group_transfer_test  response : " + message)
    except asyncio.TimeoutError:
        break
    except ConnectionClosedError as e:
        logger.error("Connection closed unexpectedly : " + str(e))
        break

```

The responses will look like the following if succeed:

```json
{
    "pres": {
        "topic": "grpf0NlkdDEQHs",
        "what": "acs",
        "dacs": {
            "want": "-ADO",
            "given": "-ADO"
        }
	}
}
```

```python
{
  "ctrl": {
    "id": "5a98cfad-e9e7-4ac4-a2fa-915d3bd72f81",
    "topic": "grpf0NlkdDEQHs",
    "params": {
      "acs": {
        "want": "JRWPASDO",
        "given": "JRWPASDO",
        "mode": "JRWPASDO"
      },
      "user": "usr804252613548777777"
    },
    "code": 200,
    "text": "ok",
    "ts": "2024-10-25T03:40:52.35514Z"
  }
}

```

## Uploading a File to the IM Server

To send a file in a chat, you first need to upload the file to the IM server using an HTTP POST form request to `https://pbx.portsip.com:8887/im/file`. An example code is provided below:

```python
//uuid4 = uuid.uuid4()
filename = str(uuid4)
with open(TEMP_DIR + '/'+ filename, 'w') as f:
    f.write(str(uuid4) * REPEAT_SIZE)

form = FormData()  
form.add_field('id', str(uuid4))  
form.add_field('file', open(TEMP_DIR + '/'+ filename, 'rb'), filename=filename)  

header = {  
    'Authorization': 'token ' + token
}  
try:
    if 'https' in upload_url:
        async with aiohttp.ClientSession(connector=TCPConnector(ssl=False)) as session:  
            async with session.post(upload_url, data=form, headers=header) as response:  
                if response.status == 200:  
                    text = await response.text()  
                    logger.debug(text)
                    count_upload()
                else:
                    text = await response.text()   
                    logger.error("http  post error status :" +  str(response.status) + " response : " + text)
    else:
        async with aiohttp.ClientSession() as session:  
            async with session.post(upload_url, data=form, headers=header) as response:  
                if response.status == 200:  
                    text = await response.text()  
                    logger.debug(text)
                    count_upload()
                else:
                    text = await response.text()   
                    logger.error("http  post error status :" +  str(response.status) + " response : " + text)
except aiohttp.ClientError as e:
    logger.error("http post error : " + str(e))
    continue
os.remove(TEMP_DIR + '/'+ filename)
await asyncio.sleep(UPLOAD_SLEEP)  
```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "93db2d24-6663-4071-a373-586b184be576",
        "params": {
            "url": "NDW4_NYseeH20iuC3rih_wAAwItyRIMM"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:13:54.464796Z"
    }
}
```

`["ctrl"]["params"]["url"]` is the file download URL. The sender can send this URL in a message to the recipient. After receiving the message, the recipient can download the file and display it in the chat according to the file type.

## Download the Chat File

Once a user receives a message containing the file URL, they can assemble the URL as shown below and download the file using an HTTP GET request.

[https://pbx.portsip.com:8887/im/file/NDW4\_NYseeH20iuC3rih\_wAAwItyRIMM](https://pbx.portsip.com:8887/im/file/NDW4\_NYseeH20iuC3rih\_wAAwItyRIMM)





