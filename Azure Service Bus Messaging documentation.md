# [Azure Services Bus Messaging documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)

## About Services Bus Messaging

### Overview

#### [What is Service Bus Messaging?](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)

Microsoft Azure Service Bus is a fully managed enterprise message broker with message queues and publish-subscribe topics. Service Bus is used to decouple applications and services from each other, providing the following benefits:

* Load-balancing work across competing workers
* Safely routing and transferring data and control across service and application boundaries
* Coordinating transactional work that requires a high-degree of reliability

##### Overview

Data is transferred between different applications and services using **messages**. A message is a container decorated with metadata, and contains data. The data can be any kind of information, including structured data encoded with the common formats such as the following ones: JSON, XML, Apache Avro, Plain Text.

Some common messaging scenarios are:

* **Messaging**. Transfer business data, such as sales or purchase orders, journals, or inventory movements.

* **Decouple applications.** Improve reliability and scalability of applications and services. Producer and consumer don't have to be online or readily available at the same time. ==The load is leveled such that traffic spikes don't overtax a service.==

* **Load Balancing**. Allow for multiple competing consumers to read from a queue at the same time, each safely obtaining exclusive ownership to specific messages.

* **Topics and subscriptions**. Enable 1:*n* relationships between publishers and subscribers, allowing subscribers to select particular messages from a published message stream.

* **Transactions**. Allows you to do several operations, all in the scope of an atomic transaction. For example, the following operations can be done in the scope of a transaction.

  1. Obtain a message from one queue.
  2. Post results of processing to one or more different queues.
  3. Move the input message from the original queue.

  The results become visible to downstream consumers only upon success, including the successful settlement of input message, allowing for once-only processing semantics. This transaction model is a robust foundation for the compensating transactions pattern in the greater solution context.
  
* **Message sessions**. Implement high-scale coordination of workflows and multiplexed transfers that require strict message ordering or message deferral.

If you're familiar with other message brokers like Apache ActiveMQ, Service Bus concepts are similar to what you know. As Service Bus is a platform-as-a-service (PaaS) offering, a key difference is that you don't need to worry about the following actions. Azure takes care of those chores for you.

* Worrying about hardware failures
* Keeping the operating systems or the products patched
* Placing logs and managing disk space
* Handling backups
* ==Failing over to a reserve machine==

##### Compliance with standards and protocols

The primary wire protocol for Service Bus is Advanced Messaging Queueing Protocol(AMQP) 1.0, an open ISO/IEC standard. It allows customers to write applications that work against Service Bus and on-premises brokers such as ActiveMQ or RabbitMQ. The AMQP protocol guide provides detailed information in case you want to build such an abstraction.

Service Bus Premium is fully compliant with the Java/Jakarta EE Java Message Service(JMS) 2.0 API. And, Service Bus Standard supports the JMS 1.1 subset focused on queues. JMS is a common abstraction for message brokers and integrates with many applications and frameworks, including the popular Spring framework. To switch from other brokers to Azure Service Bus, you just need to recreate the topology of queues and topics, and change the client provider dependencies and configuration. For an example, see the ActiveMQ migration guide.

##### Concepts and terminology

This section discusses concepts and terminology of Service Bus.

###### Queues

Messages are sent to and received from queues. Queues store messages until the receiving application is available to receive and process them.

Messages in queues are ordered and timestamped on arrival. Once accepted by the broker, the message is always held durably in triple-redundant storage, spread across availability zones if the namespace is zone-enabled. Service Bus never leaves messages in memory or volatile storage after they've been reported to the client as accepted.

Messages are delivered in pull mode, only delivering messages when requested. Unlike the busy-polling model of some other cloud queues, the pull operation can be long-lived and only complete once a message is available.

###### Topics

You can also use topics to send and receive messages. While a queue is often used for point-to-point communication, topics are useful in publish /subscribe scenarios.

Topics can have multiple, independent subscriptions, which attach to the topic and otherwise work exactly like queues from the receiver side. A subscriber to a topic can receive a copy of each message sent to that topic. Subscriptions are named entities. Subscriptions are durable by default, but can be configured to expire and then be automatically deleted. Via the JMS API, Service Bus Premium also allows you to create volatile subscriptions that exist for the duration of the connection.

You can define rules on a subscription. A subscription rule has a **filter** to define a condition for the message to be copied into the subscription and an optional **action** that can modify message metadata. For more information, see <u>Topic filters and actions</u>. This feature is useful in the following scenarios:

* You don't want a subscription to receive all messages sent to a topic
* You want to mark up messages with extra metadata when they pass through a subscription.

###### Namespaces

A namespace is a container for all messaging components(queues and topics). Multiple queues and topics can be in a single namespace, and namespaces often serve as application containers.

A namespace can be compared to a server in the terminology of other brokers, but the concepts aren't directly equivalent. A Service Bus namespace is your own capacity slice of a large cluster made up of dozens of all-active virtual machines. It may optionally span three <u>Azure availability zones</u>. So, you get all the availability and robustness benefits of running the message broker at enormous scale. And, you don't need to worry about underlying complexities. Service Bus is serverless messaging.

##### Advanced concepts

Service Bus includes advanced features such as message sessions, scheduled delivery, and transactions that enable you to solve more complex messaging problems. 

##### Client libraries

##### Integration



##### Next steps

To get started using Service Bus messaging, see the following articles:

* To compare Azure messaging services, see [Comparison of services](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services?toc=/azure/service-bus-messaging/toc.json&bc=/azure/service-bus-messaging/breadcrumb/toc.json).
* Try the quickstarts for .NET, Java, or JMS.
* To manage Service Bus resources, see Service Bus Explorer.
* To learn more about Standard and Premium tiers and their pricing, see Service Bus pricing.
* To learn about performance and latency for the Premium tier, see Premium Messaging.

### Concept

#### [Compare Azure messaging services](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services?toc=/azure/service-bus-messaging/toc.json)

Azure offers three services that assist with delivering events or messages throughout a solution. These services are:

* Azure Event Grid
* Azure Event Hubs
* Azure Service Bus

Although they have some similarities, each service is designed for particular scenarios. This article describes the differences between these services, and helps you understand which one to choose for your application. In many cases, the messaging services are complementary[^1] and can be used together.

##### Event vs. message services

There's an important distinction between services that deliver an event and services that deliver a message.

###### Event

An event is a lightweight notification of a condition or a state change. The publisher of the event has no expectation about how the event is handled. The consumer of the event decides what to do with the notification. Events can be discrete units or part of a series.

Discrete events report state change and are actionable. To take the next step, the consumer only needs to know that something happened. The event data has information about what happened but doesn't have the data that triggered the event. For example, an event notifies consumers that a file was created. It may have general information about the file, but it doesn't have the file itself. Discrete events are ideal for serverless solutions that need to scale.

Series events report a condition and are analyzable. The events are time-ordered and interrelated. The consumer needs the sequenced series of events to analyze what happened.

###### Message

A message is raw data produced by a service to be consumed or stored elsewhere. The message contains the data that triggered the message pipeline. The publisher of the message has an exception about how the consumer handles the message. A contract exists between the two sides. For example, the publisher sends a message with the raw data, and expects the consumer to create a file from that data and send a response when the work is done.

##### Azure Event Grid



##### Azure Event Hubs



##### Azure Service Bus



##### Comparison of services



##### Use the services together





#### [Compare Azure queues and Services Bus queues](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)



#### [Queues, topics, and subscriptions](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions)

Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging. These brokered messaging capabilities can be thought of as decoupled messaging features that support publish-subscribe, temporal decoupling, and load-balancing scenarios using the Service Bus messaging workload. Decoupled communication has many advantages. For example, clients and servers can connect as needed and do their operations in an asynchronous fashion.

The messaging entities that form the core of the messaging capabilities in Service Bus are **queues, topics and subscriptions**, and rules/actions.

##### Queues



##### Topics and subscriptions



##### Java message service (JMS) 2.0 entities



##### Next steps



### Architecture

#### Reference architecture for queues and events

Send and receive messages - queues

Secure

Authentication and Authorization

Authenticate with shared Access Signature

Create a Service Bus queue

Azure portal

Azure CLI

Azure PowerShell

Azure Resource Manager template

Publish and subscribe for messages

Create Service Bus topics and subscriptions

Azure portal

Azure Resource Manager tempalte

Monitor and manage

Use PowerShell to provision entities

[^1]: adj.互补的，相辅相成的

