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

##### Advanced concepts

##### Client libraries

##### Integration

##### Next steps



### Concept

Compare Azure messaging services

Compare Azure queues and Services Bus queues

Queues, topics, and subscriptions

### Architecture

Reference architecture for queues and events