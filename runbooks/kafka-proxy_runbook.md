<!--
    Written in the format prescribed by https://github.com/Financial-Times/runbook.md.
    Any future edits should abide by this format.
-->
# UPP - Kafka REST Proxy

The purpose of the Kafka REST Proxy service is to provide resilient communication between the services in the UPP Kubernetes clusters and Kafka/Zookeeper service via an easier to implement REST API interface.

## Code

kafka-proxy

## Primary URL

https://upp-prod-delivery-glb.upp.ft.com/__kafka-rest-proxy/

## Service Tier

Platinum

## Lifecycle Stage

Production

## Host Platform

AWS

## Architecture

This service runs a Docker image defined in <https://github.com/Financial-Times/kafka-proxy> . The image is based on a fork of an old version of <https://github.com/confluentinc/kafka-rest> .
It connects to the UPP Kafka and Zookeeper services and provides a RESTful API to other services to consume and produces messages from/to Kafka.

## Contains Personal Data

No

## Contains Sensitive Data

No

<!-- Placeholder - remove HTML comment markers to activate
## Can Download Personal Data
Choose Yes or No

...or delete this placeholder if not applicable to this system
-->

<!-- Placeholder - remove HTML comment markers to activate
## Can Contact Individuals
Choose Yes or No

...or delete this placeholder if not applicable to this system
-->

## Failover Architecture Type

ActiveActive

## Failover Process Type

FullyAutomated

## Failback Process Type

FullyAutomated

## Failover Details

The service is deployed in both Publishing and Delivery clusters.

The failover guide for the Delivery clusters is located here:
<https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/delivery-cluster>

The failover guide for the Publishing clusters is located here:
<https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/publishing-cluster>

## Data Recovery Process Type

NotApplicable

## Data Recovery Details

The service does not store data, so it does not require any data recovery steps.

## Release Process Type

PartiallyAutomated

## Rollback Process Type

Manual

## Release Details

A lot of services depend on this service so a failover is required during release.

<!-- Placeholder - remove HTML comment markers to activate
## Heroku Pipeline Name
Enter descriptive text satisfying the following:
This is the name of the Heroku pipeline for this system. If you don't have a pipeline, this is the name of the app in Heroku. A pipeline is a group of Heroku apps that share the same codebase where each app in a pipeline represents the different stages in a continuous delivery workflow, i.e. staging, production.

...or delete this placeholder if not applicable to this system
-->

## Key Management Process Type

Manual

## Key Management Details

To access the service clients need to provide basic auth credentials.
To rotate credentials you need to login to a particular cluster and update varnish-auth secrets.

## Monitoring

The Kafka REST proxy doesn't have monitoring of its own but several services which connect to it have healthchecks for kafka-proxy connectivity, e.g.:

*   Publishing-Prod-EU Publish Availability Monitor service health: <https://upp-prod-publish-eu.upp.ft.com/__health/__pods-health?service-name=publish-availability-monitor>
*   Publishing-Prod-US Publish Availability Monitor service health: <https://upp-prod-publish-us.upp.ft.com/__health/__pods-health?service-name=publish-availability-monitor>
*   Delivery-Prod-EU Methode Article Mapper health: <https://upp-prod-delivery-eu.upp.ft.com/__health/__pods-health?service-name=methode-article-mapper>
*   Delivery-Prod-US Methode Article Mapper health: <https://upp-prod-delivery-us.upp.ft.com/__health/__pods-health?service-name=methode-article-mapper>

## First Line Troubleshooting

<https://github.com/Financial-Times/upp-docs/tree/master/guides/ops/first-line-troubleshooting>

## Second Line Troubleshooting

Please refer to the GitHub repository README for troubleshooting information.

Additionally you can check the old Kafka REST proxy guide <https://sites.google.com/a/ft.com/universal-publishing/ops-guides/panic-guides/kafka-rest-proxy-guide>