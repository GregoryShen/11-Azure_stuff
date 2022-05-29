# [Azure Services Bus Messaging documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)

## Overview

### [1-1 What is Service Bus Messaging?](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)

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

### [1-2 Compare messaging services](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services?toc=/azure/service-bus-messaging/toc.json)

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

A message is raw data produced by a service to be consumed or stored elsewhere. The message contains the data that triggered the message pipeline. The publisher of the message has an expectation about how the consumer handles the message. A contract exists between the two sides. For example, the publisher sends a message with the raw data, and expects the consumer to create a file from that data and send a response when the work is done.

##### Azure Event Grid

Event Grid is an eventing backplane[^2]that enables event-driven, reactive programming. It uses the publish-subscribe model. Publishers emit events, but have no expectation about how the events are handled. Subscribers decide on which events they want to handle.

Event Grid is deeply integrated with Azure services and can be integrated with third-party services. It simplifies event consumption[^3]and lowers costs by eliminating the need for constant polling. Event Grid efficiently and reliably routes events from Azure and non-Azure resources. It distributes the events to registered subscriber endpoints. The event message has the information you need to react to changes in services and applications. Event Grid isn't a data pipeline, and doesn't deliver the actual object that was updated.

It has the following characteristics:

* Dynamically scalable
* Low cost
* Serverless
* At least once delivery of an event

For more information, see [Event Grid overview](https://docs.microsoft.com/en-us/azure/event-grid/overview).

##### Azure Event Hubs

Azure Event Hubs is a big data streaming platform and event ingestion[^4]service. It can receive and process millions of events per second. It facilitates the capture, retention[^5], and replay of telemetry[^6]and event stream data. The data can come from many concurrent sources. Event Hubs allows telemetry and event data to be made available to various stream-processing infrastructures and analytics services. It's available either as data streams or bundled event batches. This service provides a single solution that enables rapid data retrieval for real-time processing, and repeated replay of stored raw data. It can capture the streaming data into a file for processing and analysis.

It has the following characteristics:

* Low latency
* Can receive and process millions of events per second
* At least once delivery of an event

For more information, see [Event Hubs overview](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about).

##### Azure Service Bus

Service Bus is a fully managed enterprise message broker with message queues and publish-subscribe topics. The service is intended for enterprise applications that require transactions, ordering, duplicate detection, and instantaneous[^7]consistency. Service Bus enables cloud-native applications to provide reliable state transition management for business processes. When handling high-value messages that cannot be lost or duplicated, use Azure Service Bus. This service also facilitates highly secure communication across hybrid cloud solutions and can connect existing on-premises systems to cloud solutions.

Service Bus is a brokered messaging system. It stores messages in a "broker"(for example, a queue) until the consuming party is ready to receive the messages. It has the following characteristics:

* Reliable asynchronous message delivery (enterprise messaging as a service) that requires polling
* Advanced messaging features like first-in and first-out(FIFO), batching/sessions, transactions, dead-lettering, temporal control, routing and filtering, and duplicate detection
* At least once delivery of a message
* Optional ordered delivery of messages

For more information, see [Service Bus overview](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview).

##### Comparison of services

| Service     | Purpose                         | Type                         | When to use                                 |
| ----------- | ------------------------------- | ---------------------------- | ------------------------------------------- |
| Event Grid  | Reactive programming            | Event distribution(discrete) | React to status changes                     |
| Event Hubs  | Big data pipeline               | Event streaming(series)      | Telemetry and distributed data streaming    |
| Service Bus | High-value enterprise messaging | Message                      | Order processing and financial transactions |



##### Use the services together

In some cases, you use the services side by side to fulfill distinct roles. For example, an e-commerce site can use Service Bus to process the order, Event Hubs to capture site telemetry, and Event Grid to respond to events like an item was shipped.

In other cases, you link them together to form an event and data pipeline. You use Event Grid to respond to events in the other services. For an example of using Event Grid with Event Hubs to migrate data to a data warehouse, see [Stream big data into a data warehouse](https://docs.microsoft.com/en-us/azure/event-grid/event-grid-event-hubs-integration). The following image shows the workflow for streaming the data.

![](https://docs.microsoft.com/en-us/azure/event-grid/media/compare-messaging-services/overview.png)

### [1-3 Use Service Bus with Java Message Service (JMS) 2.0](https://docs.microsoft.com/en-us/azure/service-bus-messaging/how-to-use-java-message-service-20)



## Quickstarts

### Service Bus queues

#### Create a Service Bus queue

##### 2-1 Azure portal

**Use Azure portal to create a Service Bus namespace and a queue**

###### What are Service Bus queues?

###### Prerequisites

###### Create a namespace in the Azure portal

###### Get the connection string

###### Create a queue in the Azure portal

###### Next steps



##### 2-2 Azure PowerShell



##### 2-3 Azure CLI



##### 2-4 ARM template

#### Send and receive messages

##### 2-5 .NET (Azure.Messaging.ServiceBus)

##### 2-6 Java (azure-messaging-servicebus)

##### 2-7 Python (azure-servicebus)

**Send messages to and receive messages from Azure Service Bus queues(Python)**



##### 2-8 JavaScript (@azure/service-bus)

### Services Bus topics and subscriptions

#### Create topics and subscriptions

##### 2-9 Azure portal

##### 2-10 Azure CLI

##### 2-11 ARM template

#### Publish and subscribe for messages

##### 2-12 .NET (Azure.Messaging.ServiceBus)

##### 2-13 Java (azure-messaging-servicebus)

##### 2-14 Python (azure-servicebus)

##### 2-15 JavaScript (@azure/service-bus)

## Tutorials

### [3-1 Update inventory](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-tutorial-topics-subscriptions-portal)

**Tutorial: Update inventory using Azure portal and topics/subscriptions**

Microsoft Azure Service Bus is a multi-tenant[^3-1-1]cloud messaging service that sends information between applications and services. Asynchronous operations give you flexible, brokered messaging, along with structured first-in, first-out (FIFO) messaging, and publish/subscribe capabilities. This tutorial shows how to use Service Bus topics and subscriptions in a retail inventory scenario, with publish/subscribe channels using the Azure portal and .NET.

In this tutorial, you learn how to:

:heavy_check_mark: Create a Service Bus topic and one or more subscriptions to that topic using the Azure portal

:heavy_check_mark: Add topic filters using .NET code

:heavy_check_mark: Create two messages with different content

:heavy_check_mark: Send the messages and verify they arrived in the expected subscriptions

:heavy_check_mark: Receive messages from the subscriptions

An example of this scenario is an inventory assortment[^3-1-2]update for multiple retail stores. In this scenario, each store, or set of stores, gets messages intended for them to update their assortments. This tutorial shows how to implement this scenario using subscriptions and filters. First, you create a topic with 3 subscriptions, add some rules and filters, and then send and receive messages from the topic and subscriptions.

![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/media/service-bus-tutorial-topics-subscriptions-portal/about-service-bus-topic.png)

If you don't have an Azure subscription, you can create a free account before you begin.

#### Prerequisites

To complete this tutorial, make sure you have installed:

* Visual Studio 2017 Update 3 or later
* NET Core SDK, version 2.0 or later

#### Service Bus topics and subscriptions

Each subscription to a topic can receive a copy of each message. Topics are fully protocol and semantically compatible with Service Bus queues. Service Bus topics support a wide array of selection rules with filter conditions, with optional actions that set or modify message properties. Each time a rule matches, it produces a message. To learn more about rules, filters, and actions, follow this [link](https://docs.microsoft.com/en-us/azure/service-bus-messaging/topic-filters).

#### Create a namespace in the Azure portal

To begin using Service Bus messaging entities in Azure, you must first create a namespace with a name that is unique across Azure. A namespace provides a scoping container for addressing Service Bus resources within your application.

To create a namespace:

1. Sign in to the [Azure portal](https://portal.azure.com/)

2. In the left navigation pane of the portal, select **+ Create a resource**, select **Integration**, and then select **Service Bus**.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/create-resource-service-bus-menu.png)

3. In the **Basics** tag of the **Create namespace** page, follow these steps:

   1. For Subscription, choose an Azure subscription in which to create the namespace.

   2. For Resource group, choose an existing resource group in which the namespace will live, or create a new one.

   3. Enter a name for the namespace. The system immediately checks to see if the name is available. For a list of rules for naming namespaces, see [Create Namespace REST API](https://docs.microsoft.com/en-us/rest/api/servicebus/create-namespace).

   4. For Location, choose the region in which your namespace should be hosted.1.

   5. For Pricing tier, select the pricing tier (Basic, Standard, or Premium) for the namespace. If you want to use topics and subscriptions, choose either Standard or Premium. Topics/subscriptions are not supported in the Basic pricing tier. If you selected the Premium pricing tier, specify the number of messaging units. The premium tier provides resource isolation at the CPU and memory level so that each workload runs in isolation. This resource container is called a messaging unit. A premium namespace has at least one messaging unit. You can select 1, 2, or 4 messaging units for each Service Bus Premium namespace. For more information, see [Service Bus Premium Messaging](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-premium-messaging).

   6. Select Review + create. The system now creates your namespace and enables it. You might have to wait several minutes as the system provisions resources for your account.

      ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/create-namespace.png)

   7. On the Review + create page, review settings, and select Create.

4. Select Go to resource on the deployment page.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/deployment-alert.png)

5. You see the home page for your service bus namespace.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/service-bus-namespace-home-page.png)

#### Get the connection string

Creating a new namespace automatically generates an initial Shared Access Signature(SAS) rule with an associated pair of primary and secondary keys that each grant full control over all aspects of the namespace. See [Service Bus authentication and authorization](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-authentication-and-authorization) for information about how to create rules with more constrained rights for regular senders and receivers. To copy the primary and secondary keys for your namespace, follow these steps:

1. On the Service Bus Namespace page, select Shared access policies on the left menu.

2. On the Shared access policies page, select RootManageSharedAccessKey.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/connection-info.png)

3. In the <u>Policy: RootManageSharedAccessKey</u> window, click the copy button next to <u>Primary Connection String</u>, to copy the connection string to your clipboard for later use. Paste this value into Notepad or some other temporary location.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-namespace-portal/connection-string.png)

#### Create a topic using the Azure portal

1. On the <u>Service Bus Namespace</u> page, select <u>Topics</u> on the left menu.

2. Select <u>+ Topic</u> on the toolbar.

3. Enter a <u>name</u> for the topic. Leave the other options with their default values.

4. Select <u>Create</u>.

   ![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/includes/media/service-bus-create-topics-subscriptions-portal/create-topic.png)

#### Create subscriptions to the topic



#### Create filter rules on subscriptions

#### Send and receive messages

#### Clean up resources

#### Understand the sample code

#### Next steps

### Handle Service Bus events via Event Grid

#### 3-2 Azure Logic Apps

#### 3-3 Azure Functions

### 3-4 Build message-driven business applications with NServiceBus

## Samples

### 4-1 Service Bus samples

## Concepts

### [5-1 Queues, topics, and subscriptions](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-queues-topics-subscriptions)

Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including <u>reliable message queuing</u> and <u>durable publish/subscribe messaging</u>. These brokered messaging capabilities can be thought of as decoupled messaging features that support publish-subscribe, temporal decoupling, and load-balancing scenarios using the Service Bus messaging workload. Decoupled communication has many advantages. For example, clients and servers can connect as needed and do their operations in an asynchronous fashion.

The messaging entities that form the core of the messaging capabilities in Service Bus are **queues, topics and subscriptions**, and rules/actions.

##### Queues

Queues offer **First in, First Out** (FIFO) message delivery to one or more competing consumers. That is, receivers typically receive and process messages in the order in which they were added to the queue. And, only one message consumer receives and processes each message. A key benefit of using queues is to achieve **temporal decoupling of application components**. In other words, the producers (senders) and consumers (receivers) don't have to send and receive messages at the same time. That's because messages are stored durably in the queue. Furthermore, the producer doesn't have to wait for a reply from the consumer to continue to process and send messages.

A related benefit is **load-leveling**, which enables producers and consumers to send and receive messages at different rates. In many applications, the system load varies over time. However, the processing time required for each unit of work is typically constant. Intermediating message producers and consumers with a queue means that the consuming application only has to be able to handle average load instead of peak load. The depth of the queue grows and contracts as the incoming load varies. This capability directly saves money regarding the amount of infrastructure required to service the application load. As the load increases, more worker processes can be added to read from the queue. Each message is processed by only one of the worker processes. Furthermore, this pull-based load balancing allows for best use of the worker computers even if the worker computers with processing power pull messages at their own maximum rate. This pattern is often termed the **competing consumer** pattern.

Using queues to intermediate between message producers and consumers providers an inherent loose coupling between the components. Because producers and consumers aren’t aware of each other, a consumer can be upgraded without having any effect on the producer.

###### Create queues

You can create queues using the Azure portal, PowerShell, CLI, or Resource Manager templates. Then, send and receive messages using clients written in C#, Java, Python, and JavaScript.

###### Receive modes

You can specify two different modes in which Service Bus receives messages.

* **Receive and delete**. In this mode, when Service Bus receives the request from the consumer, it marks the message as being consumed and returns it to the consumer application. This mode is the simplest model. It works best for scenarios in which the application can tolerate not processing a message if a failure occurs. To understand this scenario, consider a scenario in which the consumer issues the receive request and then crashes before processing it. As Service Bus marks the message as being consumed, the application begins consuming messages upon restart. It will miss the message that it consumed before the crash.

* **Peek lock**. In this mode, the receive operation becomes two-stage, which makes it possible to support applications that can’t tolerate missing messages.

  1. Finds the next message to be consumed, locks it to prevent other consumers from receiving it, and then, return the message to the application.

  2. After the application finishes processing the message, it requests the Service Bus service to complete the second stage of the receive process. Then, the service **marks the message as being consumed**.

     If the application is unable to process the message for some reason, it can request the Service Bus service to **abandon** the message. Service Bus **unlocks** the message and makes it available to be received again, either by the same consumer or by another competing consumer. Secondly, there’s a timeout associated with the lock. If the application fails to process the message before the lock timeout expires, Service Bus unlocks the message and makes it available to be received again.

     If the application crashes after it processes the message, but before it requests the Service Bus service to complete the message, Service Bus redelivers the message to the application when it restarts. This process is often called **at-least once** processing. That is, each message is processed at least once. However, in certain situations the same message may be redelivered. If your scenario can’t tolerate duplicate processing, add additional logic in your application to detect duplicates. For more information, see <u>Duplicate detection</u>. This feature is known as **exactly once** processing.

##### Topics and subscriptions

A queue allows processing of a message by a single consumer. In contrast to queues, topics and subscriptions provide a one-to-many form of communication in a **publish and subscribe** pattern. It's useful for scaling to large numbers of recipients. Each published message is made available to each subscription registered with the topic. Publisher sends a message to a topic and one or more subscribers receive a copy of the message, depending on filter rules set on these subscriptions. The subscriptions can use additional filters to restrict the messages that they want to receive. Publishers send messages to a topic in the same way that they send messages to a queue. But, consumers don't receive messages directly from the topic. Instead, consumers receive messages from subscriptions of the topic. A <u>topic subscription</u> ==resembles a virtual queue that receives copies of the messages that are sent to the topic==. Consumers receive messages from a subscription identically to the way they receive messages from a queue.

<u>The message-sending functionality of a queue maps directly to a topic and its message-receiving functionality maps to a subscription.</u> Among other things, this feature means that subscriptions support the same patterns described earlier in this section regarding queues: competing consumer, temporal decoupling, load leveling, and load balancing.

###### Create topics and subscriptions

Create a topic is similar to creating a queue, as describes in the previous section. You can create topics and subscriptions using the Azure portal, PowerShell, CLI, or Resource Manager templates. Then, send messages to a topic and receive messages from subscriptions using clients written in C#, Java, Python, and JavaScript.

###### Rules and actions

In many scenarios, messages that have specific characteristics must be processed in different ways. To enable this processing, you can configure subscriptions to find messages that have desired properties and then perform certain modifications to those properties. While Service Bus subscriptions see all messages sent to the topic, you can only copy a subset of those messages to the virtual subscription queue. This filtering is accomplished using subscription filters. Such modifications are called **filter actions**. When a subscription is created, you can supply a filter expression that operates on the properties the message. The properties can be both the system properties (for example, **Label**) and custom application properties(for example, **StoreName**). The SQL filter expression is optional in this case. Without a SQL filter expression, any filter action defined on a subscription will be done on all the messages for that subscription.

For a full working example, see the TopicFilters sample on GitHub.

For more information about filters, see Topic filters and actions.

##### Java message service (JMS) 2.0 entities

The following entities are accessible through the Java message service (JMS) 2.0 API.

* Temporary queues
* Temporary topics
* Shared durable subscriptions
* Unshared durable subscriptions
* Shared non-durable subscriptions
* Unshared non-durable subscriptions

Learn more about the JMS 2.0 entities and about how to use them.

##### Next steps

Try the samples in the language of your choice to explore Azure Service Bus features.

* [Azure Service Bus client library samples for Python](https://docs.microsoft.com/en-us/samples/azure/azure-sdk-for-python/servicebus-samples/)

### [5-2 Premium messaging](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-premium-messaging)



### [5-3 Compare Azure Queues and Service Bus queues](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)



### 5-4 Advanced features

#### [5-4-1 Overview of advanced features](https://docs.microsoft.com/en-us/azure/service-bus-messaging/advanced-features-overview)

Service Bus includes advanced features that enable you to solve more complex messaging problems. This article describes several of these features.

Message sessions

Autoforwarding

Dead-letter queue

Scheduled delivery

Message deferral

Batching

Transactions

Autodelete on idle

Duplicate detection

Geo-disaster recovery

Security

Next steps

#### [5-4-2 Message sessions](https://docs.microsoft.com/en-us/azure/service-bus-messaging/message-sessions)



#### [5-4-3 Duplicate message detection](https://docs.microsoft.com/en-us/azure/service-bus-messaging/duplicate-detection)



#### [5-4-4 Topic filters and actions](https://docs.microsoft.com/en-us/azure/service-bus-messaging/topic-filters)



#### 5-4-5 Message browsing

#### 5-4-6 Message transfers, locks, and settlement

#### [5-4-7 Dead-letter queues](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dead-letter-queues)

**Overview of Service Bus dead-letter queues**

Azure Service Bus queues and topic subscriptions provide a secondary subqueue, called a *dead-letter queue*(DLQ). The dead-letter queue doesn't need to be explicitly created and can't be deleted or managed independent of the main entity.

This article describes dead-letter queues in Service Bus. Much of the discussion is illustrated by the [Dead-Letter queues sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/DeadletterQueue) on GitHub.

##### The dead-letter queue

The purpose of the dead-letter queue is to hold messages that can't be delivered to any receiver, or messages that couldn't be processed. Messages can then be removed from the DLQ and inspected. An application might, with help of an operator, correct issues and resubmit the message, log the fact that there was an error, and take corrective action.

From an API and protocol perspective, the DLQ is mostly similar to any other queue, except that messages can only be submitted via the dead-letter operation of the parent entity. In addition, time-to-live isn't observed, and you can't dead-letter a message from a DLQ. The dead-letter queue fully supports peek-lock delivery and transactional operations.

There's no automatic cleanup of the DLQ. Messages remain in the DLQ until you explicitly retrieve them from the DLQ and complete the dead-letter message.

##### DLQ message count

It's not possible to obtain count of messages in the dead-letter queue at the topic level. That's because messages don't sit at the topic level unless Service Bus throws an internal error. Instead, when a sender sends a message to a topic, the message is forwarded to subscriptions for the topic within milliseconds and thus no longer resides at the topic level. So, you can see messages in the DLQ associated with the subscription for the topic. In the following example, Service Bus Explorer shows that there are 62 messages currently in the DLQ for the subscription "test1".

![](https://docs.microsoft.com/en-us/azure/service-bus-messaging/media/service-bus-dead-letter-queues/dead-letter-queue-message-count.png)

You can also get the count of DLQ messages by using Azure CLI command:

##### Moving messages to the DLQ

There are several activities in Service Bus that cause messages to get pushed to the DLQ from within the messaging engine itself. An application can also explicitly move messages to the DLQ. The following two properties (dead-letter reason and lead-letter description) are added to dead-lettered messages. Applications can define their own codes for the dead-letter reason property, but the system sets the following values.

| Dead-letter reason                        | Dead-letter error description                                |
| ----------------------------------------- | ------------------------------------------------------------ |
| HeaderSizeExceeded                        | The size quota for this stream has been exceeded.            |
| TTLExpiredException                       | The message expired and was dead lettered. See the <u>Time to live</u> section for details. |
| Session ID is null.                       | Session enabled entity doesn't allow a message whose session identifier is null |
| MaxTransferHopCountExceeded               | The maximum number of allowed hops when forwarding between queues. Value is set to 4. |
| MaxDeliveryCountExceeededExceptionMessage | Message couldn't be consumed after maximum delivery attempts. See the <u>Maximum delivery count</u> section for details. |

##### Maximum delivery count

There is a limit on number of attempts to deliver messages for Service Bus queues and subscriptions. The default value is 10. Whenever a message has been delivered under a peek-lock, but has been either explicitly abandoned or the lock has expired, the delivery count on the message is incremented. When the delivery count exceeds the limit, the message is moved to the DLQ. The dead-letter reason for the message in DLQ is set to: MaxDeliveryCountExceeded. This behavior can't be disabled, but you can set the max delivery count to a large number.

##### Time to live

When you enable dead-lettering on queues or subscriptions, all expiring messages are moved to the DLQ. The dead-letter reason code is set to: TTLExpiredException.

The deferred messages will also not be purged[^5-4-7-1]and moved to the dead-letter queue after they expire. This behavior is by design.

##### Errors while processing subscription rules

If you enable dead-lettering on filter evaluation exceptions, any errors that occur while a subscription's SQL filter rule executes are captured in the DLQ along with the offending message. Don't use this option in a production environment in which no all message types have subscribers.

##### Application-level dead-lettering

In addition to the system-provided dead-lettering features, applications can use the DLQ to explicitly reject unacceptable messages. They can include messages that can't be properly processed because of any sort of system issue, messages that hold malformed payloads, or messages that fail authentication when some message-level security scheme is used.

##### Dead-lettering in ForwardTo or SendVia scenarios

Messages will be sent to the transfer dead-letter queue under the following conditions:

* A message passes through more than four queues or topics that are chained together.
* The destination queue or topic is disabled or deleted.
* The destination queue or topic exceeds the maximum entity size.

##### Path to the dead-letter queue

You can access the dead-letter queue by using the following syntax:

```xml
<queue path>/$deadletterqueue
<topic path>/Subscriptions/<subscription path>/$deadletterqueue
```

##### Next steps

See [Enable dead lettering for a queue or subscription](https://docs.microsoft.com/en-us/azure/service-bus-messaging/enable-dead-letter) to learn about different ways of configuring the **dead lettering on message expiration** setting.

#### 5-4-8 Message deferral

#### 5-4-9 Prefetch messages

#### 5-4-10 Chain entities with auto-forwarding

#### 5-4-11 Transaction processing





### Federation

这些都不需要看

### Security

这些都不需要看

### Integration with other services

这些都不需要看



## How-to guides

6-1 Develop

6-2 Migrate

6-3 Monitor

6-4 Integrate

6-5 Manage

6-6 Secure

6-7 Troubleshoot

## Reference

7-1 Monitor data reference



## Resources

这些都不需要看



Architecture

Reference architecture for queues and events

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

[^2]: 底板
[^3]: n.消费，消耗量; 吃，喝; 肺结核
[^4]: n.摄取; 采食
[^5]: n.保留; 记忆力，保持力; 滞留，扣留; 闭尿
[^6]: [təˈlɛmɪtri] n.遥感勘测，自动测量记录传导; 测远术; 遥测
[^7]: adj.瞬间的; 即刻的; 猝发的

[^3-1-1]: n. 房客; 佃户; <律>占用者; 占有者；vt.租借，租用
[^3-1-2]: n.各式各样；什锦

[^5-4-7-1]: vt.清除，（使）净化; （使）通便; 肃清；n.净化; <医>泻药; 整肃



