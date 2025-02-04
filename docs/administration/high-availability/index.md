---
title: High Availability
description: Octopus High Availability (HA) enables you to run multiple Octopus Server nodes, distributing load and tasks between them.
hideInThisSection: true
position: 10
---

Octopus: High Availability (HA) enables you to run multiple Octopus Server nodes, distributing load and tasks between them. We designed it for enterprises that need to deploy around the clock and rely on the Octopus Server being available.

![High availability diagram](images/high-availability.svg "width=500")

An Octopus High Availability configuration requires four main components:

- **A load balancer**
  This will direct user traffic bound for the Octopus web interface between the different Octopus Server nodes.
- **Octopus Server nodes**
  These run the Octopus Server service. They serve user traffic and orchestrate deployments.
- **A database**
  Most data used by the Octopus Server nodes is stored in this database.
- **Shared storage**
  Some larger files - like [packages](/docs/packaging-applications/package-repositories/index.md), artifacts, and deployment task logs - aren't suitable to be stored in the database, and so must be stored in a shared folder available to all nodes.

## Licensing

Each Octopus Deploy SQL Server database is a unique **Instance**.  Nodes are the Octopus Server service that connects to the database.  High Availability occurs when two or more nodes connect to the same Octopus Deploy database.  An HA Cluster refers to all components, the load balancer, nodes, database, and shared storage.

For self-hosted customers, High Availability is available to the following licenses type:

- High Avalability (discontinued license type): limited to 2, 3, or 4 nodes.
- Data Center (discontinued license type): unlimited nodes
- Server: unlimited nodes

All other license types, such as Standard, Enterprise, Team, and Professional, are limited to a single node and cannot be used for High Availablity.

The node limit is included in the license key in the NodeLimit node.

```XML
<NodeLimit>Unlimited</NodeLimit>
```

If you do not have that node in your license key then you are limited to a single node.  If you recently purchased a license key and it is missing that node then reach out to [sales@octopus.com](mailto:sales@octopus.com).

## How High Availablity Works

In broad terms, HA allows for load to be distributed between multiple Octopus Server nodes.  How that load is distributed, specifically tasks, is more complex than "it's load balanced."

Learn more in our [How High Availability Works](/docs/administration/high-availability/how-high-availability-works.md) section.

## Designing Octopus High Availability

There are several ways to configure High Availability for Octopus and this differs based on where you host Octopus. We have created guides that will help you design the best solution for your installation. This section walks through the different options and considerations for setting up Octopus and how you can incorporate each of the components making them highly-available, whether its using on-premises servers or cloud infrastructure such as Azure, AWS or GCP:

- [Designing Octopus for High Availability On-Premises](/docs/administration/high-availability/design/octopus-for-high-availability-on-premises.md)
- [Designing Octopus for High Availability in Azure](/docs/administration/high-availability/design/octopus-for-high-availability-on-azure.md)
- [Designing Octopus for High Availability in AWS](/docs/administration/high-availability/design/octopus-for-high-availability-on-aws.md)
- [Designing Octopus for High Availability in GCP](/docs/administration/high-availability/design/octopus-for-high-availability-on-gcp.md)

## Configuring Octopus High Availability

When you have selected the approach you will use for Octopus High Availability and provisioned your infrastructure, the next step is to configure it. This section includes guides on configuring Octopus for High Availability with and without Active Directory:

- [Configuring High Availability: with Active Directory](/docs/administration/high-availability/configure/octopus-with-active-directory.md)
- [Configuring High Availability: without Active Directory](/docs/administration/high-availability/configure/octopus-without-active-directory.md)

## Migrating to High Availability

Most organizations start with a stand-alone Octopus installation as part of a Proof of Concept. We make it straight-forward to take your existing Octopus installation and migrate it to a highly-available configuration.

Learn more in our [Migrating to High Availability](/docs/administration/high-availability/migrate/index.md) section.

## Maintaining High Availability nodes

One great benefit of Octopus High Availability is the ability to update and restart one or more nodes, while still allowing the rest of the Octopus Deploy cluster to keep serving requests and performing deployments. 

This section contains useful information on how to maintain the nodes in your Octopus High Availability cluster, along with specific things to know when running an Octopus High Availability instance:

- [Maintaining High Availability nodes](/docs/administration/high-availability/maintain/maintain-high-availability-nodes.md)
- [Polling Tentacles with Octopus High Availability](/docs/administration/high-availability/maintain/polling-tentacles-with-ha.md)

## Load balancing

There are plenty of options when it comes to choosing a load balancer to direct user traffic between each of the Octopus Server nodes. You can also use Apache or NGINX as a reverse load-balancing proxy. For more information on setting up a load balancer with Octopus High Availability we have the following guides:

- [Configure Netscaler](/docs/administration/high-availability/load-balancing/configuring-netscaler.md)
- [Using NGINX as a reverse proxy with Octopus](/docs/security/exposing-octopus/use-nginx-as-reverse-proxy.md)
- [Using IIS as a reverse proxy with Octopus](/docs/security/exposing-octopus/use-iis-as-reverse-proxy.md)

## Troubleshooting

If you're running into issues with your Octopus High Availability then please use our [Troubleshooting High Availability](/docs/administration/high-availability/troubleshooting/index.md) guide.
