# CDR Field Descriptions

This article provides field descriptions for the Call Detail Records (CDRs) in the order in which they appear in the CDR.

## CDR Structure

In PortSIP PBX, the Call Detail Record (CDR) includes both call information and call targets. A **call target** refers to an endpoint. For example, if user 102 registers with the PBX from an IP phone and an app, when user 101 calls 102, the PBX will send INVITE messages to both the IP phone and the app. In this scenario, the CDR will have two call targets: one for the IP phone and another for the app. Please see the CDR below for reference.

```
```

