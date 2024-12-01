# Configuring Queue Callback

## Queue Callback

A callback is a request callers can make to have their call returned when an agent is unavailable to take it right away. In contact centers, callbacks provide assistance for busy agents and provide an extra level of service to customers who encounter wait times. Callbacks increase customer satisfaction, since a caller does not have to wait on hold or possibly incur long distance charges while waiting to speak with an agent.

<figure><img src="../../../.gitbook/assets/queue_back_1.png" alt=""><figcaption></figcaption></figure>

Tenant Administrators can configure and manage rules to handle callbacks to customers. The queue will automatically offer the customers the ability to retain their position in the queue and arrange for voice callbacks when their turn arrives.

You need to specify the **Callback Outbound Prefix** to match an outbound rule to make the call. Set the **Callback Mode** to be requested by the caller by pressing **3** or to be offered when the queue timeout is reached.

<figure><img src="../../../.gitbook/assets/queue_callback.png" alt=""><figcaption></figcaption></figure>

## Trigger the callback by the customer requesting

If a customer has been waiting in the queue for a long time; he does not want to wait but needs to keep his spot; he simply presses **3**, and he will hear:

> **Please confirm your phone number in full ends with # so that we may call you back. Alternatively, if you would like us to contact you on the current number, just press`#`. To continue holding, please press,`*` and we will be with you as soon as possible**.

## Trigger the callback by the timeout

If a customer has been waiting in queue for longer than the time specified in the **Offered to the caller after timeout in seconds**. the queue will automatically play the prompt to the caller:

> **All of our agents are busy at this time. We appreciate your patience. You can continue to hold or press 3 to keep your position in the queue and schedule a callback.**

Once the caller presses **3**, he will hear:

> **Please confirm your phone number in full ends with # so that we may call you back. Alternatively, if you would like us to contact you on the current number, just press `#`. To continue holding, please press `*` and we will be with you as soon as possible.**

Once the caller confirms their number, the call will automatically end. When the caller’s spot reaches the front of the queue, the PBX will call an available agent. If the agent answers, the PBX will then dial the caller’s number. The callback is considered complete once the caller answers the call.



