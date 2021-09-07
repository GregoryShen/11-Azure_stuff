# [Publisher-Subscriber pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber)

Enable an application to announce events to multiple interested consumers asynchronously, without coupling the senders to the receivers.

**Also called**: Pub/sub messaging

## Context and problem

In cloud-based and distributed applications, components of the system often need to provide information to other components as events happen.

Asynchronous messaging is an effective way to decouple senders from consumers, and avoid blocking the sender to wait for a response. However, using a dedicated message queue for each consumer does not effectively scale to many consumers. Also, some of the consumers might be interested in only a subset of the information. How can the sender announce events to all interested consumers without knowing their identities?

## Solution

Introduce an asynchronous messaging subsystem that includes the following:

* An input messaging channel used by the sender. The sender packages events into messages, using a known message format, and sends these messages via the input channel. The sender in this pattern is also called the *publisher*.[^1]
* One output messaging channel per consumer. The consumers are known as *subscribers*.
* A mechanism for copying each message from the input channel to the output channels for all subscribers interested in that message. This operation is typically handled by an intermediary such as a message broker or event bus.

The following diagram shows the logical components of this pattern:

![](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/publish-subscribe.png)

Pub/sub messaging has the following benefits:

* It decouples subsystems that still need to communicate. Subsystems can be managed independently, and messages can be properly managed even if one or more receivers are offline.
* It increases scalability and improves responsiveness of the sender. The sender can quickly send a single message to the input channel, then return to its core processing responsibilities. The messaging infrastructure is responsible ensuring messages are delivered to the interested subscribers.
* It improves reliability. Asynchronous messaging 

## Issues and considerations

## When to use this pattern

## Example

## Next steps

## Related guidance

[^1]: A *message* is a packet of data. An *event* is a message that notifies other components about a change or an action that has taken place.

