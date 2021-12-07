# UPP - ZooKeeper

UPP ZooKeeper is the deployment of Apache ZooKeeper within the UPP clusters used by UPP Kafka. Apache ZooKeeper is an open-source service for distributed systems offering a hierarchical key-value store, which is used to provide a distributed configuration service, synchronization service, and naming registry for large distributed systems.

## Primary URL

<https://github.com/Financial-Times/upp-kafka>

## Service Tier

Platinum

## Lifecycle Stage

Production

## Host Platform

AWS

## Architecture

UPP ZooKeeper is deployed as a part of the Kafka - ZooKeeper system. A single Kafka broker is deployed along with a single ZooKeeper instance in all UPP Publishing and Delivery K8s clusters. Apache Kafka needs Apache ZooKeeper for various metadata storage purposes and cluster management (although the "cluster" in UPP has a single member, ZooKeeper is still required). The service runs and old version of ZooKeeper (3.4.13) using a public Docker image: <https://hub.docker.com/r/wurstmeister/zookeeper>

## Contains Personal Data

No

## Contains Sensitive Data

No

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

If the data on the persistent storage is lost there is no recovery process as the metadata saved in ZooKeeper is of temporary nature and will be recreated when the Kafka broker and clients are working again.

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

UPP ZooKeeper doesn't have monitoring of its own but the UPP Kafka Lagcheck service is depending on Kafka and ZooKeeper and one can check its health status:
- Publishing EU UPP Kafka Lagcheck service health: <https://upp-prod-publish-eu.upp.ft.com/__health/__pods-health?service-name=kafka-lagcheck>
- Publishing US UPP Kafka Lagcheck service health: <https://upp-prod-publish-us.upp.ft.com/__health/__pods-health?service-name=kafka-lagcheck>
- Delivery EU UPP Kafka Lagcheck service health: <https://upp-prod-delivery-eu.upp.ft.com/__health/__pods-health?service-name=kafka-lagcheck>
- Delivery US UPP Kafka Lagcheck service health: <https://upp-prod-delivery-us.upp.ft.com/__health/__pods-health?service-name=kafka-lagcheck>

## First Line Troubleshooting

<https://github.com/Financial-Times/upp-docs/tree/master/guides/ops/first-line-troubleshooting>

## Second Line Troubleshooting

Kafka panic guide: <https://sites.google.com/a/ft.com/universal-publishing/ops-guides/panic-guides/kafka-panic-guide>

Also one can refer to the GitHub repository README for additional troubleshooting information.
