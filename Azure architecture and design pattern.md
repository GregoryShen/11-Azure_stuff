# 1 Azure application architecture fundamentals

## [1-1 Introduction]((https://docs.microsoft.com/en-us/azure/architecture/guide/))

This library of content presents a structured approach for designing applications on Azure that are scalable, secure, resilient, and highly available. The guidance is based on proven practices that we have learned from customer engagements.

### Introduction

The cloud is changing how applications are designed and secured. Instead of monoliths, applications are decomposed into smaller, decentralized services. These services communicate through APIs or by using asynchronous messaging or eventing. Applications scale horizontally, adding new instances as demand requires.

These trends bring new challenges. Application states are distributed. Operations are done in parallel and synchronously. Applications must be resilient when failures occur. Malicious actors continuously target applications. Deployments must be automated and predictable. Monitoring and telemetry are critical for gaining insight into the system. This guide is designed to help you navigate these changes.

| **Traditional on-premises**          | **Modern cloud**                                   |
| ------------------------------------ | -------------------------------------------------- |
| Monolithic                           | Decomposed                                         |
| Designed for predictable scalability | Designed for elastic scale                         |
| Relational database                  | Polyglot persistence (mix of storage technologies) |
| Synchronized processing              | Asynchronous processing                            |
| Design to avoid failures (MTBF)      | Design for failure (MTTR)                          |
| Occasional large updates             | Frequent small updates                             |
| Manual management                    | Automated self-management                          |
| Snowflake servers                    | Immutable infrastructure                           |

### How this guidance is structured

The Azure application architecture fundamentals guidance is organized as a series of steps, from the architecture and design to implementation. For each step, there is supporting guidance that will help you with the design of your application architecture.

![](https://docs.microsoft.com/en-us/azure/architecture/guide/images/a3g.svg)

### Architecture styles

The first decision point is the most fundamental. What kind of architecture are you building? It might be a microservices architecture, a more traditional N-tier application, or a big data solution. We have identified several distinct architecture styles. There are benefits and challenges to each.

### Technology choices

Knowing the type of architecture you are building, now you can start to choose the main technology pieces for the architecture. The following technology choices are critical:

* *Compute* refers to the hosting model for the computing resources that your applications run on. 
* *Data stores* include databases but also storage for message queues, caches, logs, and anything else that an application might persist to storage.
* *Messaging* technologies enable asynchronous messages between components of the system.

You will probably have to make additional technology choices along the way, but these three elements (compute, data, and messaging) are central to most cloud applications and will determine many aspects of your design.

### Design the architecture

Once you have chosen the architecture style and the major technology components, you are ready to tackle the specific design of your application. Every application is different, but the following resources can help you along the way:

#### Reference architectures

Depending on your scenario, one of our reference architectures may be a good starting point. Each reference architecture includes recommended practices, along with considerations for scalability, availability, security, resilience, and other aspects of the design. Most also include a deployable solution or reference implementaion.

#### Design principles

We have identified 10 high-level design principles that will make your application more scalable, resilient, and manageable. These design principles apply to any architecture style. Throughout the design process, keep these 10 high-level design principles in mind.

#### Design patterns

Software design patterns are repeatable patterns that are proven to solve specific problems. Our catalog of Cloud design patterns addresses specific challenges in distributed systems. They address aspects such as availability, high availability, operational excellence, resiliency, performance, and security.

#### Best practices

Our best practices articles cover various design considerations including API design, autoscaling, data partitioning, caching, and so forth. Review these and apply the best practices that are appropriate for your application.

#### Security best practices

Our security best practices describe how to ensure that the confidentiality, integrity, and availability of your application aren't compromised by malicious actors.

### Quality pillars

A successful cloud application will focus on five pillars of software quality: Cost optimization, Operational excellence, Performance efficiency, Reliability, and Security.

Leverage the Microsoft Azure Well-Architected Framework to assess your architecture across these five pillars.

## 1-2 Architecture styles

### [1-2-1 Overview]((https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/))



### 1-2-2 Big compute

### 1-2-3 Big data

### [1-2-4 Event-driven architecture](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven)



### [1-2-5 Microservices](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices)



### [1-2-6 N-tier application](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)



### [1-2-7 Web-queue-worker](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/web-queue-worker)



# 2 Design Patterns

## [2-1 Overview]((https://docs.microsoft.com/en-us/azure/architecture/patterns/))



## 2-2 Categories

### [2-2-1 Data management](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/data-management)



### [2-2-2 Design and implementation](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/design-implementation)



### [2-2-3 Messaging](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/messaging)



## [2-3 Priority Queue](https://docs.microsoft.com/en-us/azure/architecture/patterns/priority-queue)



## [2-4 Publisher-Subscriber](https://docs.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber)

Enable an application to announce events to multiple interested consumers asynchronously, ==without coupling the senders to the receivers==.

**Also called**: Pub/sub messaging

### 2-4-1 Context and problem

In cloud-based and distributed applications, components of the system often need to provide information to other components as events happen.

==Asynchronous messaging is an effective way to decouple senders from consumers, and avoid blocking the sender to wait for a response.== However, using a dedicated message queue for each consumer does not effectively scale to many consumers. Also, some of the consumers might be interested in only a subset of the information. How can the sender announce events to all interested consumers without knowing their identities?

### 2-4-2 Solution

Introduce an asynchronous messaging subsystem that includes the following:

* An input messaging channel used by the sender. The sender packages events into messages, using a known message format, and sends these messages via the input channel. The sender in this pattern is also called the *publisher*.[^1]
* One output messaging channel per consumer. The consumers are known as *subscribers*.
* A mechanism for copying each message from the input channel to the output channels for all subscribers interested in that message. This operation is typically handled by an intermediary[^2] such as a message broker[^3] or event bus.

The following diagram shows the logical components of this pattern:

![](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/publish-subscribe.png)

Pub/sub messaging has the following benefits:

* It ==decouples subsystems that still need to communicate==. Subsystems can be managed independently, and messages can be properly managed even if one or more receivers are offline.
* It increases scalability and ==improves responsiveness of the sender==. The sender can quickly send a single message to the input channel, then return to its core processing responsibilities. The messaging infrastructure is responsible for ensuring messages are delivered to the interested subscribers.
* It improves reliability. Asynchronous messaging helps applications continue to run smoothly under increased loads and handle intermittent failures more effectively.
* It ==allows for deferred or scheduled processing==. Subscribers can wait to pick up messages until off-peak hours, or messages can be routed or processed according to a specific schedule.
* It ==enables simpler integration between systems== using different platforms, programming languages, or communication protocols, as well as between on-premises systems and applications running in the cloud.
* It facilitates asynchronous workflows across an enterprise.
* It ==improves testability==. Channels can be monitored and messages can be inspected or logged as part of an overall integration test strategy.
* It provides ==separation of concerns for your applications==. Each application can focus on its core capabilities, while the messaging infrastructure handles everything required to reliably route messages to multiple consumers. 

### 2-4-3 Issues and considerations

Consider the following points when deciding how to implement this pattern:

* **Existing technologies**. It is strongly recommended to use available messaging products and services that support a publish-subscribe model, rather than building your own. In Azure, consider using <u>Service Bus</u>, <u>Event Hubs</u> or <u>Event Grid</u>. Other technologies that can be used for pub/sub messaging include Redis, RabbitMQ, and Apache Kafka.
* **Subscription handling**. The messaging infrastructure must provide mechanisms that consumers can use to subscribe to or unsubscribe from available channels.
* **Security**.  Connecting to any message channel must be restricted by security policy to prevent eavesdropping[^4] by unauthorized users or applications.
* **Subsets of messages**. Subscribers are usually only interested in subset of the messages distributed by a publisher. Messaging services often allow subscribers to narrow the set of messages received by:
  * **Topics**. Each topic has a dedicated output channel, and each consumer can subscribe to all relevant topics.
  * **Content filtering**. ==Messages are inspected and distributed based on the content of each message.== Each subscriber can specify the content it is interested in.
* **Wildcard subscribers**. Consider allowing subscribers to subscribe to multiple topics via wildcards.
* **Bi-directional communication**. The channels in a publish-subscribe system are treated as unidirectional[^5]. If a specific subscriber needs to send acknowledgement or communicate status back to the publisher, consider using the [Request/Reply Pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html). This pattern uses one channel to send a message to the subscriber, and a separate reply channel for communicating back to the publisher.
* **Message ordering**. The order in which consumer instances receive messages isn't guaranteed, and doesn't necessarily reflect the order in which the messages were created. Design the system to ensure that message processing is idempotent[^6]to help eliminate any dependency on the order of message handling.
* **Message priority**. Some solutions may require that messages are processed in a specific order. The [Priority Queue pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/priority-queue) provides a mechanism for ensuring specific messages are delivered before others.
* **Poison messages**. A malformed[^7]message, or a task that requires access to resources that aren't available, can cause a service instance to fail. ==The system should prevent such messages being returned to the queue.== Instead, capture and store the details of these messages elsewhere so that they can be analyzed if necessary.
* **Repeated messages**. The same message might be sent more than once. For example, the sender might fail after posting a message. Then a new instance of the sender might start up and repeat the message. The messaging infrastructure should implement ==duplicate message detection and removal== (also known as de-duping[^8]) based on message IDs in order to provide at-most-once delivery of messages.
* **Message expiration**. A message might have a limited lifetime. If it isn't processed within this period, it might no longer be relevant and should be discarded. A sender can specify an expiration time as part of the data in the message. A receiver can examine this information before deciding whether to perform the business logic associated with the message.
* **Message scheduling**. A message might be temporarily embargoed[^9] and should not be processed until a specific date and time. The message should not be available to a receiver until this time.

### 2-4-4 When to use this pattern

Use this pattern when:

* An application needs to broadcast information to a significant number of consumers.
* An application needs to communicate with one or more independently-developed applications or services, which may use different platforms, programming languages, and communication protocols.
* An application can send information to consumers without requiring real-time responses from the consumers.
* The systems being integrated are designed to support an eventual consistency model for their data.
* An application needs to communicate information to multiple consumers, which may have different availability requirements or uptime[^10] schedules than the sender.

This pattern might not be useful when:

* An application has only a few consumers who need significantly different information from the producing application.
* An application requires near real-time interaction with consumers.

### 2-4-5 Example

The following diagram shows enterprise integration architecture that uses Service Bus to coordinate workflows, and Event Grid to notify subsystems of events that occur. For more information, see [Enterprise integration on Azure using message queues and events](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/queues-events).

![Enterprise integration architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/_images/enterprise-integration-queues-events.png)

### 2-4-6 Next steps

The following guidance might be relevant when implementing this pattern:

* [Choose between Azure services that deliver messages](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services).
* The [Event-driven architecture style](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven) is an architecture style that uses pub/sub messaging.
* [Asynchronous Messaging Primer[^11]](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/dn589781(v=pandp.10)). Message queues are an asynchronous communications mechanism. If a consumer service needs to send a reply to an application, it might be necessary to implement some form of response messaging. The Asynchronous Messaging Primer provides information on how to implement request/reply messaging using message queues.

### 2-4-1-7 Related guidance

The following patterns might be relevant when implementing this pattern:

* [Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern). The Publish-Subscribe pattern builds on the Observer pattern by decoupling subjects from observers via asynchronous messaging.
* [Message Broker pattern](https://en.wikipedia.org/wiki/Message_broker). Many messaging subsystems that support a publish-subscribe model are implemented via a message broker.

# 3 Miscellaneous

## [3-1 Request/Reply Pattern](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html)





## [3-2 Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern)



## [3-3 Message Broker pattern](https://en.wikipedia.org/wiki/Message_broker)



# 4 Azure categories

## 4-1 Integration

### 4-1-1 Architectures

#### [4-1-1-1-1 Enterprise integration - queues and events](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/enterprise-integration/queues-events)



[^1]: A *message* is a packet of data. An *event* is a message that notifies other components about a change or an action that has taken place.

[^2]: [ˌɪntərˈmiːdieri], n.中间人; 媒介; 调解人; 中间阶段
[^3]: [ˈbroʊkər], n.经纪人，代理商；v.协调，安排
[^4]: [ˈivzdrɑp] vi.偷听（别人的谈话）
[^5]: [ˌjunɪdə'rekʃənəl] adj.单向的，单向性的
[^6]: [aɪ'dempətənt], n.幂等，等幂；adj.幂等的
[^7]: [ˌmælˈfɔrmd], adj. 难看的，畸形的
[^8]: De-deduping software, also know as dedupe or deduplication software, is a **software application that will help to identify and remove duplicated records from lists and databases**. This process is commonly used to help prevent duplicated names and addresses.
[^9]: [ɪmˈbɑːrɡoʊ], n.禁运（令；v.禁运（货物）
[^10]: n.（计算机等的）正常运行时间
[^11]: [ˈprɪmɚ] n.底漆; 启蒙读本; 入门书



