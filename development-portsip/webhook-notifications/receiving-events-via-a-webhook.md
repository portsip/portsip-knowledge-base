# Receiving Events via a Webhook

By default, endpoints will not receive any events, as they are subscribed to none. To know more about the subscription process, see [Registering a Webhook](registering-a-webhook.md). Once subscribed to events, endpoints will receive updates in the following fashion:

* Each event update is sent using an HTTP POST request to the endpoint URL.
* The endpoint URL MUST return with a status code 200 to acknowledge the reception of the event.

***

### Event Delivery Guarantees <a href="#event-delivery-guarantees" id="event-delivery-guarantees"></a>

This Webhook API implementation tries to deliver the event notification request reasonably fast. We may buffer notifications for efficiency reasons.

PortSIP PBX cannot guarantee the delivery of every event (see [Endpoint Unavailability](receiving-events-via-a-webhook.md#endpoint-unavailability)). However, PortSIP PBX will retry delivery during the first 24 hours after the event was triggered, PortSIP PBX will retry until the endpoint receives them correctly by responding with an HTTP status code of 200.

{% hint style="warning" %}
If the response code is 404, the PortSIP PBX will stop retrying.
{% endhint %}

PortSIP PBX cannot guarantee the order in which the events arrive. PortSIP PBX does however provide a timestamp in every event for the implementor to understand the exact point in time the event was triggered.

***

### Endpoint Unavailability <a href="#endpoint-unavailability" id="endpoint-unavailability"></a>

When an event notification request to an endpoint is not responded by an HTTP 200 OK status (network issue, endpoint is down ...), the request will be retried. The request will be retried during 15 minutes 5 times with waiting time growing.

To give an order of magnitude of the waiting time before each retry (those data are for information only and are susceptible to change):

| Request | Time interval |
| ------- | ------------- |
| Try #1  | \~ 15 secs    |
| Try #2  | \~ 1 min      |
| Try #3  | \~ 3 mins     |
| Try #4  | \~ 8 mins     |
| Try #5  | \~ 15 mins    |

It means that when the first request fails the next retry is processed after 15 secs, then 1 minute after the last retry... If it’s still failing this request will never be sent afterward.

***

#### Automatic Disablement

{% hint style="danger" %}
If your endpoint's error rate is too high over a long period of time, we MAY have to disable the delivery of event notifications. You will need to fix the problem before re-enabling the event notifications in the admin interface.
{% endhint %}

***

### Implementation Considerations <a href="#implementation-considerations" id="implementation-considerations"></a>

While the rate of event updates is usually reasonable, it can greatly increase depending on external factors. Therefore, it is recommended that the endpoint URL does as little as possible, ideally just storing the event information in a queue for background processing.

Event updates will be sent at least once. In some cases, the same event MAY be sent several times, therefore the endpoint SHOULD be idempotent. In order to help achieve idempotency, each event update has a unique UUID, that will persist if sent more than once.





