# Accessing CDRs and Recordings

This guide explains how to use the **PortSIP REST API** to access and manage **Call Detail Records (CDRs)** and **call recordings**.

***

### Authentication Requirement

Before making any REST API requests, you must successfully authenticate and obtain an access token.\
This token acts as a security credential and is required to authorize all interactions with the PortSIP REST API.

***

### Permission Restrictions

Access to CDRs and call recordings is controlled by **user roles** within a tenant:

* **Admin or Queue Manager**\
  Users with these roles can access CDRs and call recordings at the **tenant level**, allowing them to view data for all users within the tenant.
* **Regular Users**\
  Regular users can only access **their own** CDRs and call recordings, restricting visibility to personal call data only.

***

### Retrieve CDR <a href="#id-2-retrieve-cdr-list" id="id-2-retrieve-cdr-list"></a>

<mark style="color:green;">`POST`</mark> `/api/cdrs`

Retrieve the CDRs at the tenant level using the specified query parameters.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Query Options**

You can specify the query options as the URL parameters to filter the CDR, for more details please see [Query Options Overview](version-22.0/about.md#query-options-overview).&#x20;

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "count": 2,
    "items": [
        {
            "id": "878156644691742720",
            "caller": "102",
            "caller_domain": "test.com",
            "caller_display_name": "Kelly Cruickshank",
            "callee": "103",
            "callee_domain": "test.com",
            "callee_display_name": "Lucas Heidenreich",
            "started_at": "2024-08-20T06:00:59.457188Z",
            "rang_at": "2024-08-20T06:00:59.543647Z",
            "answered_at": "2024-08-20T06:01:01.965027Z",
            "ended_at": "2024-08-20T06:01:36.158919Z",
            "call_id": "JooRSqaUcdwZnsD7DNgA3Q..",
            "direction": "EXTENSION_CALL",
            "end_reason": "CALLER_DISCONNECT",
            "reroute_reason": "",
            "status_code": 0,
            "destination": "",
            "outbound_caller_id": "",
            "did_cid": "",
            "trunk": "",
            "duration": 35
        },
        {
            "id": "878156595253481472",
            "caller": "101",
            "caller_domain": "test.com",
            "caller_display_name": "Dwayne Turcotte",
            "callee": "102",
            "callee_domain": "test.com",
            "callee_display_name": "Kelly Cruickshank",
            "started_at": "2024-08-20T06:00:47.670813Z",
            "rang_at": "2024-08-20T06:00:47.701241Z",
            "answered_at": "",
            "ended_at": "2024-08-20T06:00:49.680179Z",
            "call_id": "zuRUKmbcjgfrNTrp46smiw..",
            "direction": "EXTENSION_CALL",
            "end_reason": "",
            "reroute_reason": "",
            "status_code": 486,
            "destination": "",
            "outbound_caller_id": "",
            "did_cid": "",
            "trunk": "",
            "duration": 0
        }
    ]
}
 
```
{% endtab %}

{% tab title="400" %}
```json
{
 
}
```
{% endtab %}
{% endtabs %}

### Retrieve Call Recordings <a href="#id-2-retrieve-cdr-list" id="id-2-retrieve-cdr-list"></a>

<mark style="color:green;">`POST`</mark> `/api/recordings`

Retrieve the call recordings at the tenant level using the specified query parameters.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Query Options**

You can specify the query options as the URL parameters to filter the CDR, , for more details please see [Query Options Overview](version-22.0/about.md#query-options-overview).&#x20;

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "count": 123,
    "items": [
        {
            "id": "876741670492704768",
            "caller": "17108415934",
            "callee": "1012",
            "started_at": "2024-08-16T08:19:14.746Z",
            "ended_at": "2024-08-16T08:19:28.755Z"
        },
        {
            "id": "875996174459342848",
            "caller": "1107",
            "callee": "1106",
            "started_at": "2024-08-14T06:56:10.235Z",
            "ended_at": "2024-08-14T06:56:19.118Z"
        },
        {
            "id": "875309729700646912",
            "caller": "82250068",
            "callee": "7000",
            "started_at": "2024-08-12T09:28:22.151Z",
            "ended_at": "2024-08-12T09:28:29.564Z"
        }
    ]
}
```
{% endtab %}

{% tab title="400" %}
```json
{
 
}
```
{% endtab %}
{% endtabs %}

### Retrieve Recordings Information <a href="#id-2-retrieve-cdr-list" id="id-2-retrieve-cdr-list"></a>

<mark style="color:green;">`POST`</mark> `/api/recordings/{id}`

This API endpoint allows you to fetch detailed information about a specific recording based on its unique identifier.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Query Options**

Specify the recording ID which you parsed from the response of [Retrieve Call Records](accessing-cdrs-and-recordings.md#id-2-retrieve-cdr-list-1) as the URL path.

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "items": [
        {
            "caller": "102",
            "callee": "103",
            "started_at": "2024-08-20T06:01:02.109Z",
            "ended_at": "2024-08-20T06:01:36.161Z",
            "duration": 34,
            "file_name": "877828121322061824_878156644691742720_102_45060_08202024_140059.wav",
            "file_size": 0,
            "file_url": "/api/blobs/rZTOS5yB5elnATSbfxd_pgAQQKn01i8M"
        }
    ]
}
```
{% endtab %}

{% tab title="400" %}
```json
{
 
}
```
{% endtab %}
{% endtabs %}

### Download a Recording File

<mark style="color:green;">`GET`</mark> `/api/blobs/rZTOS5yB5elnATSbfxd_pgAQQKn01i8M`

Once you have [retrieved the recording information](accessing-cdrs-and-recordings.md#id-2-retrieve-cdr-list-2), the response will contain a `file_url` property. Use this URL to directly download the recording file. **No additional authentication (e.g., access token) is required for this download request.**

Simply make a GET request to the specified `file_url` to initiate the download. The recording file will be available for download at the provided location.



