# How to Adjust the REST API Rate Limit?

PortSIP PBX enforces a rate limit on REST API requests to protect system stability and prevent abuse.

#### Default Rate Limits by Version

* **v16.0.x – v16.1.x:** 1,000 requests per minute
* **v16.2.0 and later:** 5,000 requests per minute

***

### Customizing the REST API Rate Limit

If your deployment requires a different rate limit, you can modify the configuration as described below.

***

#### Step-by-Step Instructions

1.  **Open the configuration file**

    ```
    /var/lib/portsip/pbx/system.ini
    ```
2.  **Locate the API gateway section**

    In the configuration file, find:

    ```ini
    [apigateway]
    ```
3.  **Modify the rate limit**

    Locate the `access_limit` parameter and set it to the desired value.

    **Example: Set the limit to 2,000 requests per minute**

    ```ini
    access_limit = 2000
    ```
4.  **Save the file**

    Save your changes and close the editor.

***

### Applying the Changes

After modifying the configuration, you must restart the API Gateway service for the new limit to take effect.

#### Restart the API Gateway (Linux)

Run the following commands:

```bash
cd /opt/portsip
/bin/sh pbx_ctl.sh restart -s portsip.gateway
```

***

#### Expected Outcome

* The new REST API request rate limit is applied immediately after the gateway restart.
* API requests exceeding the configured limit will be throttled according to the new setting.



