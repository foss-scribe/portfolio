### POST Login

`{base_url}/identity/open/v2/auth/login`


Login or register a new user by providing a choice of authentication methods, as described in the following table.

| Method | Required fields | Scenario |
| :--------- | :--------- | :--------- |
| Mobile/cell phone | `tn` and `code` | Log in (and optionally register) using a phone number and verification code. |
| Mobile/cell phone | `tn` and `password` | Log in using a phone number and password. |
| Email | `email` and `password` | Log in using email and password. |
| WeChat | `code`, `appId`, and `secret` | Log in using [WeChat workflow](../sbc_auth_we_chat/). |
| WeChat (new user) | `tn`, `code`, `unionId` and `unionCode` | Log in (and optionally register) a new user via WeChat. |

Note, if `tn` and `code` is passed, and no user matches the presented phone number and code, the system automatically creates a new user account registered for the provided phone number.

If the phone number exists in the system, and the user logs in with their `tn`, `code`, `unionId` and `unionCode`, they are logged in and their `unionId` is associated with their account. If the phone number does not exist in the system, a new account is registered with the phone number and `unionId` set.

With other combinations, such as `email` and `password`, passing credentials not registered with the system results in an error. To register users with an email, use the [register endpoint](#).

To register and login using WeChat credentials, refer to the [WeChat Authentication recipe](../sbc_auth_we_chat/) for more information.

Note, this endpoint also allows users to accept any required terms and conditions. Terms and conditions are mandated by the API key's settings as configured by the company's administrator.


#### Body

The body accepts JSON with the fields listed in the following table.

| Field | Description |
| :----- | :------- |
| `tn` | The user's fully qualified IDD phone number, for example: +8610000000000. |
| `code` | A verification code sent upon request to the user's phone number, or WeChat callback URL. Mandatory if `tn` or `appId` is presented. |
| `email` | The user's email address, if attempting to log in via email instead of by phone number. |
| `password` | The user's password. Required when `email` is presented. |
| `appId` | WeChat app ID. Required for [WeChat-based authenticated](../sbc_auth_we_chat/). |
| `secret` | WeChat app secret. Required when `appId` is presented. |
| `unionId` | The user's WeChat ID. Required for [WeChat-based authenticated](../sbc_auth_we_chat/). |
| `unionCode` | A one-time WeChat code used to associated a WeChat ID with a Specialized account. |
| `accept` | Optional. A list of terms and conditions to accept upon login, if required. |

**Example body**

*Login via phone number and code*

This example show the request body when attempting to log in using a phone number and verification. The example allow shows the optional `accept` field for accepting terms and conditions.

```json
{
    "tn": "+8610000000000",
    "code": "00000",
    "accept": [ "Specialized_Terms_Apps", "Specialized_Terms"]
}
```


*Login via email and password*


```json
{
    "email" : "user@example.com",
    "password" : "my_secret_password"
}
```

#### Header

```
Content-Type	application/json
Api-Key         {clientId}
```

#### Request
	
``` bash
curl --location --request POST '{base_url}/identity/open/v2/auth/login' \
--header 'Content-Type: application/json' \
--header 'Api-Key: {clientId}' \
--data-raw '{
    "tn": "+8610000000000",
    "code": "00000",
    "accept": [ "Specialized_Terms_Apps", "Specialized_Terms"]
}'
```


#### Response

**Successful -- 200 OK**

On a successful request, the endpoint returns a bearer token, refresh token, token expiry, and the user's record in JSON format with HTTP status code `200 OK`.

``` json
{
    "tokenExp": 1648001093576,
    "refresh": "<refresh_token>",
    "token": "<access_token>",
    "user": {
        "email": null,
        "tn": "+8610000000000",
        "uid": "REDACTED",
        "uuid": "REDACTED",
        "avatar": null,
        "clientId": "specialized-china-dev-test",
        "validated": false,
        "suspended": false,
        "optInMask": 0,
        "attsMask": 0,
        "prefsMask": 0,
        "ridesMask": 0,
        "firstname": null,
        "lastname": null,
        "locale": null,
        "timezone": null,
        "height": null,
        "weight": null,
        "dob": null,
        "dobDayOfYear": null,
        "dobMonth": null,
        "dobDayOfMonth": null,
        "dobYear": null,
        "speedCountry": null,
        "billingCountry": null,
        "purchaseCountry": null,
        "isoRegion": null,
        "metric": null,
        "createdTime": 1647922361480,
        "modifiedTime": 1647922361480,
        "lastLogin": 1647999118613,
        "lastRefresh": 1647995102134,
        "addresses": null,
        "tns": null,
        "identities": {},
        "company": null,
        "roles": [
            "TURBO_SBC_USER",
            "SBC_USER"
        ],
        "attsMap": {
            "ocx": false,
            "betaOverride1": false
        },
        "prefsMap": {
            "ccpa": false
        },
        "ridesMap": {
            "DIRT": false,
            "CYCLOCROSS": false,
            "ROAD": false,
            "MOUNTAIN": false,
            "CASUAL": false,
            "TRIATHLON": false,
            "PEDALASSIST": false,
            "FITNESS": false,
            "ADVENTURE": false,
            "DOWNHILL": false
        }
    }
}
```

#### Errors

**Invalid code**

In this example, the user attempted to pass an invalid or previously used code.

```json
{
    "code": 401906,
    "title": "GENERIC_UNAUTHORIZED",
    "detail": "Login failed",
    "errors": [
        {
            "code": 401114,
            "title": "INVALID_CODE",
            "detail": "Invalid code",
            "source": "code"
        }
    ]
}
```

**Missing identifying field**

In this example, the user attempted to login without including a valid phone number, username (email address), or appId (WeChat app ID).

```json
{
    "code": 400907,
    "title": "GENERIC_BAD_REQUEST",
    "detail": "Required field not found",
    "errors": [
        {
            "code": 400900,
            "title": "REQUIRED_FIELD",
            "detail": "tn|username|appId is a required field",
            "source": "tn|username|appId"
        }
    ]
}
```

**Missing code**

In this example, the user attempted to login without including a verification code.

```json
{
    "code": 400907,
    "title": "GENERIC_BAD_REQUEST",
    "detail": "Required field not found",
    "errors": [
        {
            "code": 400900,
            "title": "REQUIRED_FIELD",
            "detail": "code|password is a required field",
            "source": "code|password"
        }
    ]
}
```

**Invalid phone number**

In this example, the user attempted to pass an invalid phone number.

```json
{
    "code": 400907,
    "title": "GENERIC_BAD_REQUEST",
    "detail": "Invalid parameter",
    "errors": [
        {
            "code": 400913,
            "title": "INVALID_TN_FORMAT",
            "detail": "Phone number must be a fully qualified IDD starting with a +",
            "source": "tn"
        }
    ]
}
```

**WeChat user not registered**

In this example, a WeChat user attempted to login without first being registered with Specialized. To authenticate using WeChat, refer to the [WeChat Authentication recipe](../sbc_auth_we_chat/).

```
{
    "code": 401906,
    "title": "GENERIC_UNAUTHORIZED",
    "detail": "Login failed",
    "errors": [
        {
            "code": 401102,
            "title": "INVALID_USERNAME_OR_PASSWORD",
            "detail": "Incorrect username or password",
            "source": "unionId",
            "meta": {
                "unionCode": "<OTC>",
                "unionId": "<UNIONID>"
            }
        }
    ]
}
```

***