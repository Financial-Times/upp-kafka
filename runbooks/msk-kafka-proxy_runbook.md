# UPP - MSK Kafka REST Proxy

The purpose of the MSK Kafka REST Proxy service is to provide resilient communication between the services in the UPP Kubernetes clusters and MSK Kafka/Zookeeper service via an easier to implement REST API interface.

## Code

msk-kafka-proxy

## Primary URL

https://upp-prod-delivery-glb.upp.ft.com/__kafka-rest-proxy-msk/

## Service Tier

Bronze

## Lifecycle Stage

Production

## Host Platform

AWS

## Architecture

This service runs a Docker image defined in https://github.com/Financial-Times/kafka-proxy . The image is based on a fork of an old version of https://github.com/confluentinc/kafka-rest .
It connects to the MSK Kafka and Zookeeper services and provides a RESTful API to other services to consume and produces messages from/to Kafka.

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

The service is deployed in both Publishing and Delivery clusters.

The failover guide for the Delivery clusters is located here:
https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/delivery-cluster

The failover guide for the Publishing clusters is located here:
https://github.com/Financial-Times/upp-docs/tree/master/failover-guides/publishing-cluster

## Data Recovery Process Type

NotApplicable

## Data Recovery Details

The service does not store data, so it does not require any data recovery steps.

## Release Process Type

PartiallyAutomated

## Rollback Process Type

Manual

## Release Details

Manual failover is needed when a new version of the service is deployed to production.
Otherwise, an automated failover is going to take place when releasing.

## Key Management Process Type

Manual

## Key Management Details

To access the service clients need to provide basic auth credentials.
To rotate credentials you need to login to a particular cluster and update varnish-auth secrets.

## Monitoring

The Kafka REST proxy doesn't have monitoring of its own.

## First Line Troubleshooting

https://github.com/Financial-Times/upp-docs/tree/master/guides/ops/first-line-troubleshooting

## Second Line Troubleshooting

Please refer to the GitHub repository README for troubleshooting information.
