# Accessing CDRs and Recordings

This guide outlines how to leverage the PortSIP REST API to interact with Call Detail Records (CDRs) and call recordings.

## **Authentication Requirement**

Before making any API requests, you must successfully [authenticate and obtain an access token](authentication.md). This token serves as a security credential to authorize your interactions with the PortSIP REST API.

## **Permission Restrictions**

Access to CDRs and recordings is restricted based on user roles within a tenant:

* **Admin or Queue Manager:** Users with these roles can access CDRs and recordings at the tenant level, meaning they can view data for all users within the tenant.
* **Regular Users:** Regular users can only access their own CDRs and recordings, limiting their visibility to personal call data.

## Retrieve CDR <a href="#id-2-retrieve-cdr-list" id="id-2-retrieve-cdr-list"></a>

<mark style="color:green;">`POST`</mark> `/api/cdrs`

Retrieve the CDRs at the tenant level using the specified query parameters.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Query Parameters**



**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "id": 1,
  "name": "John",
  "age": 30
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}







