# Authentication

### Authorization Code Flow

The authorization code is obtained by using an authorization server as an intermediary between the client and resource owner. When calling this method the link to a login page location is returned. Web applications are higly advised to use the Proof Key for Code Exchange scheme (PKCE) for security concerns.

When the client exchanges authorization code to access or refresh tokens, then `Authorization` header is not required. When the client refreshes a token belonging to a session opened using the `refresh_token` grant type, then `Authorization` header is not required. In both cases the client should provide the `client_id` as a form data parameter to identify itself.

### Authenticate with the System Administrator

<mark style="color:green;">`POST`</mark> `/api/login/oauth/token`

Authenticate the PBX system administrator with PortSIP PBX.

**Headers**

| Name         | Value                               |
| ------------ | ----------------------------------- |
| Content-Type | `application/x-www-form-urlencoded` |

**Body**

| Name        | Type   | Description                                                 |
| ----------- | ------ | ----------------------------------------------------------- |
| grant\_type | string | The value is always "password".                             |
| username    | string | The System Administrator's username.                        |
| password    | string | The System Administrator's password.                        |
| scope       | string | The value is always  "all"                                  |
| client\_id  | string | The value is always "9d806019-75b2-4b3d-bb8b-f5a3a412cc0a". |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "access_token": "NGFHYTBINGMTN2Y5NY0ZZJK2LWJJM2QTMMM0YTFIYJC0NDAZ",
    "expires_in": 1800,
    "refresh_token": "MTQ0NGE5M2UTMWE1MY01ZGJKLWIZY2QTMJJMZJU5MJVLNTDI",
    "token_type": "Bearer"
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "errors": [
        {
            "code": "UNAUTHORIZED",
            "message": "Login failed, authentication error"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### Authenticate with a Tenant User

<mark style="color:green;">`POST`</mark> `/api/login/oauth/token`

Authenticate the tenant user with the PortSIP PBX.

**Headers**

| Name         | Value                               |
| ------------ | ----------------------------------- |
| Content-Type | `application/x-www-form-urlencoded` |

**Body**

| Name        | Type   | Description                                                 |
| ----------- | ------ | ----------------------------------------------------------- |
| grant\_type | string | The value is always "password".                             |
| username    | string | The extension's username.                                   |
| domain      | string | The SIP domain of tenant.                                   |
| password    | string | The password of the user.                                   |
| scope       | string | The value is always  "all"                                  |
| client\_id  | string | The value is always "9d806019-75b2-4b3d-bb8b-f5a3a412cc0a". |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "access_token": "NGFHYTBINGMTN2Y5NY0ZZJK2LWJJM2QTMMM0YTFIYJC0NDAZ",
    "expires_in": 1800,
    "refresh_token": "MTQ0NGE5M2UTMWE1MY01ZGJKLWIZY2QTMJJMZJU5MJVLNTDI",
    "token_type": "Bearer"
}
```
{% endtab %}

{% tab title="400" %}
```
{
    "errors": [
        {
            "code": "UNAUTHORIZED",
            "message": "Login failed, authentication error"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### Refresh Access Token

<mark style="color:green;">`POST`</mark> `/api/login/oauth/token`

Refresh the `access_token` using the `refresh_token`.

**Headers**

| Name         | Value                               |
| ------------ | ----------------------------------- |
| Content-Type | `application/x-www-form-urlencoded` |

**Body**

| Name           | Type   | Description                                                            |
| -------------- | ------ | ---------------------------------------------------------------------- |
| grant\_type    | string | The value is always "refresh\_token".                                  |
| refresh\_token | string | The refresh token is obtained from the response of the authentication. |
| client\_id     | string | The value is always "9d806019-75b2-4b3d-bb8b-f5a3a412cc0a".            |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "access_token": "NGFHYTBINGMTN2Y5NY0ZZJK2LWJJM2QTMMM0YTFIYJC0NDAZ",
    "expires_in": 1800,
    "refresh_token": "MTQ0NGE5M2UTMWE1MY01ZGJKLWIZY2QTMJJMZJU5MJVLNTDI",
    "token_type": "Bearer"
}
```
{% endtab %}

{% tab title="400" %}
```
{
    "errors": [
        {
            "code": "UNKNOWN",
            "message": "unknown error"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### Revoke Access Token

<mark style="color:green;">`POST`</mark> `/api/login/oauth/revoke`

Revoke the current access token.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Response**

{% tabs %}
{% tab title="200" %}

{% endtab %}
{% endtabs %}

