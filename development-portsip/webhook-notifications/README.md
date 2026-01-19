# Webhook Events

PortSIP PBX provides a **Webhook API** that allows external systems to receive **near real-time notifications** when events occur within the PBX. Webhooks offer an efficient alternative to polling the REST API and are designed to support **event-driven integrations**.

By using webhooks, you can trigger actions immediately when specific events occur (for example, events being assigned, deferred, or canceled). This enables use cases such as:

* Live dashboards and wallboards
* Gamification and performance tracking
* Real-time data synchronization with external systems
* Workflow automation and event-driven business logic

The webhook mechanism is based on **HTTP POST requests**. When an event occurs, PortSIP PBX sends an HTTP request containing the **event payload** to a configured **consumer endpoint URL**.

***

### Development Guidelines

When consuming the PortSIP PBX Webhook API, we strongly recommend following these best practices to ensure compatibility and long-term stability:

* All webhook payloads are standard **JSON objects**, encoded using **UTF-8**.
* Always access fields by **field name**, not by position or order within the JSON document.
* Do **not** rely on the ordering of elements in a JSON object unless explicitly documented.
* Always use a **JSON parser** to process webhook payloads. Avoid parsing JSON using regular expressions.
* Design your implementation to **gracefully handle new fields** that may be added to event payloads without prior notice.
* Existing fields will be preserved whenever possible to maintain **backward compatibility**.

***

### Getting Started

The following sections will help you configure and start using PortSIP PBX Webhooks:

* [Create a Webhook](registering-a-webhook.md) – Configure webhook endpoints in PortSIP PBX
* [Receiving Webhooks](receiving-events-via-a-webhook.md) – Handle incoming HTTP event notifications
* [Event Reference](event-reference.md) – Review supported event types and payload structures



