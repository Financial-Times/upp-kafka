# UPP - Kafka

UPP Kafka is the deployment of Apache Kafka within the UPP clusters. Apache Kafka is an open-source stream-processing software platform aiming to provide a unified, high-throughput, low-latency platform for handling real-time data feeds.

## Primary URL

<https://github.com/Financial-Times/upp-kafka>

## Service Tier

Platinum

## Lifecycle Stage

Production

## Delivered By

content

## Supported By

content

## Known About By

- mihail.mihaylov
- hristo.georgiev
- elitsa.pavlova
- kalin.arsov
- boyko.boykov

## Host Platform

AWS

## Architecture

UPP Kafka is used for persistance of publish requests from external CMSes (incoming messages queue) and additionally as a backbone for enabling the decoupling and asynchronous communication between the services in the content and metadata publishing pipelines. UPP services connect either directly to the Kafka brokers or via the UPP Kafka REST proxy using a few Java and Go libraries.

A single Kafka broker is deployed along with a single ZooKeeper instance in all UPP Publishing and Delivery K8s clusters. Apache Kafka needs Apache ZooKeeper for various metadata storage purposes and cluster management (although the "cluster" in UPP has a single member, ZooKeeper is still required). The service runs and old version of Kafka (0.8.2.0) using a public Docker image: <https://hub.docker.com/r/wurstmeister/kafka>

## Contains Personal Data

No

## Contains Sensitive Data

No

## Dependencies

- upp-zookeeper

## Failover Architecture Type

ActiveActive

## Failover Process Type

FullyAutomated

## Failback Process Type

FullyAutomated

## Failover Details

The service is deployed in all Publishing and Delivery clusters.

The failover guide for the Delivery clusters is located here:
<https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/delivery-cluster>

The failover guide for the Publishing clusters is located here:
<https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/publishing-cluster>

## Data Recovery Process Type

NotApplicable

## Data Recovery Details

Kafka is configured to retain messages on disk for up to 7 days.
However if the data on the persistent storage is lost there is no recovery process as these messages are of temporary nature and will be resubmitted on republish if needed.

## Release Process Type

PartiallyAutomated

## Rollback Process Type

Manual

## Release Details

Manual failover is needed when a new version of the service is deployed to production because the publishing pipeline is depending on it.

## Key Management Process Type

Manual

## Key Management Details

To access the service clients need to provide basic auth credentials. To rotate credentials you need to login to a particular cluster and update varnish-auth secrets.

## Monitoring

UPP Kafka doesn't have monitoring of its own but several services which connect to it have healthchecks for Kafka connectivity, e.g.:
- Publishing EU Native Ingester service health: <https://upp-prod-publish-eu.upp.ft.com/__health/__pods-health?service-name=native-ingester-cms>
- Publishing US Native Ingester service health: <https://upp-prod-publish-us.upp.ft.com/__health/__pods-health?service-name=native-ingester-cms>
- Delivery EU PAC Annotations Mapper service health: <https://upp-prod-delivery-eu.upp.ft.com/__health/__pods-health?service-name=pac-annotations-mapper>
- Delivery US PAC Annotations Mapper service health: <https://upp-prod-delivery-us.upp.ft.com/__health/__pods-health?service-name=pac-annotations-mapper>

Additionally, the UPP Kafka Lagcheck service is depending on Kafka and ZooKeeper and one can check its health status.

## First Line Troubleshooting

<https://github.com/Financial-Times/upp-docs/tree/master/guides/ops/first-line-troubleshooting>

## Second Line Troubleshooting

Kafka panic guide: <https://sites.google.com/a/ft.com/universal-publishing/ops-guides/panic-guides/kafka-panic-guide>

Also one can refer to the GitHub repository README for additional troubleshooting information.
