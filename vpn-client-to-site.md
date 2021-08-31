---

copyright:
  years: 2021
lastupdated: "2021-8-26"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:new_window: target="_blank"}
{:beta: .beta}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}

# About client-to-site VPN servers (Beta)
{: #vpn-client-to-site-overview}

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

Until now, the IBM Cloud VPN for VPC service supported only site-to-site connectivity, which connects your on-premises network to the IBM Cloud VPC network. This beta release adds client-to-site connectivity, which allows clients from the internet, such as a laptop, to connect to the VPC network while still maintaining secure connectivity. This solution is useful for telecommuters who want to connect to the IBM Cloud from a remote location, such as a home office.

Highlights include:

* TLS1.2/1.3-based secure/encrypted connectivity over the internet
* Supports both stand-alone (pilot) and high availability (production) deployments
* Privately interconnect Classic IaaS and VPCs on IBM Public Cloud
* Availability in all [MZRs world wide](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region) except `br-sao`
* High availability spanning across zones, providing better performance and resiliency
* Provides added layers of security with integrated authentication methods

## Architecture
{: #vpn-client-to-site-architecture}

Figure 1 illustrates an example VPN server setup to connect resources in and out of the VPC. The VPN server is provisioned on two subnets within a user's VPC. There are also two VPN server members that work in Active/Active High Availability (HA) mode. All of the VPN server members can communicate with target resources. There is one public IP address assigned to each of the members, and a DNS hostname record is created for the VPN server. The hostname is resolved to the VPN member's public IP address. VPN clients get two public IP addresses via DNS resolution and try two IP addresses randomly to connect with one of the VPN members. The VPN client attempts to reconnect and switch to an active VPN server member if one member is down.

A DNS service is deployed as part of the VPN service. The DNS name provided ends with `appdomain.cloud`.
{: note}

![Client-to-site VPN server architecture](images/vpn-server-arch.png "Client-to-site VPN server architecture"){: caption="Figure 1. Client-to-site VPN server architecture" caption-side="bottom"}

## Getting started
{: #vpn-client-to-site-getting-started}

To get started using the Client VPN for VPC beta, follow these steps:

1. Review [Planning considerations for VPN servers](/docs/vpc?topic=vpc-client-to-site-vpn-planning).
1. Complete all prerequisites in [Before you begin](/docs/vpc?topic=vpc-vpn-create-server#vpn-client-to-site-prerequisites).
1. Provision a stand-alone VPN server in a subnet, or provision a high availability VPN server in the two subnets. For instructions, see [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server).
1. [Create VPN routes](/docs/vpc?topic=vpc-vpn-client-to-site-routes).
1. [Set up a VPN client environment and connect to the VPN server](/docs/vpc?topic=vpc-vpn-client-environment-setup).

## VPN server use cases
{: #vpn-client-to-site-use-cases}

Here are some ways that you can implement the IBM Cloud Client VPN for VPC service:

### Use case 1: Access VPC within a deployed MZR
{: #access-vpc-within-deployed-mzr}

The VPN server is deployed in a selected Multi-zone Region (MZR) and VPC. All virtual server instances are accessible from the VPN client in the single VPC.

![Network topology: A VPN client can access a virtual server instance within a deployed MZR through the VPN server](images/vpn-server-usecase-vms.png "A VPN client can access a virtual server instance within a deployed MZR through the VPN server"){: caption="Figure 2. Network topology: A VPN client can access a virtual server instance within a deployed MZR through the VPN server" caption-side="bottom"}

### Use case 2: A VPN client can access the internet through the VPN server
{: #access-internet-use-case}

When the administrator enforces the VPN server in full-tunnel mode, all traffic from customer devices is sent to the VPN server, including internet traffic. The VPN server forwards the internet traffic to the IBM Cloud infrastructure Edge node and finally reaches the internet.

![Network topology: A VPN client can access the internet through the VPN server](images/vpn-connect-to-internet.png "A VPN client can access the internet through the VPN server"){: caption="Figure 3. Network topology: A VPN client can access the internet through the VPN server" caption-side="bottom"}

### Use case 3: Integrating with a transit gateway
{: #integrate-transit-vpn-gateway}

Generally, it is recommended to provision VPC resources in multiple regions for redundancy. To access the resources in all regions from personal devices, one approach is to create one client-to-site VPN server per VPC, per region, and establish the VPN connection to all VPN servers. You must also maintain multiple VPN servers with this approach. This might be an inconvenience, but it is a more secure method. Another approach is to use a transit gateway to connect all these VPCs; as a result, only one VPN server is required to access the VPCs.

![Network topology: Integrating with a transit gateway](images/vpn-server-use-case-tgw-integration.png "Integrating with a transit gateway"){: caption="Figure 4. Network topology: Integrating with a transit gateway" caption-side="bottom"}

### Use case 4: Integrating with a site-to-site VPN gateway
{: #integrating-with-site-to-site-vpn-gateway}

Integrate with a site-to-site VPN gateway if you want to access your on-premises private network at the same time as when you connect to IBM VPCs. This use case removes the requirement to maintain multiple VPN servers simultaneously. You can access your on-premises private network from a client-to-site VPN server directly.  

![Network topology: Integrating with a site-to-site VPN gateway](images/vpn-server-use-case-vpn-gateway.png "Integrating with a site-to-site VPN gateway"){: caption="Figure 5. Network topology: Integrating with a site-to-site VPN gateway" caption-side="bottom"}

## Related links

* [Quotas](/docs/vpc?topic=vpc-quotas#vpn-server-quotas)
* [Required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls#vpn-server-authorizations-required-for-api-and-cli-calls)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-vpn-server)
* [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpn-server-clis)
* [VPC API reference (Beta version)](/apidocs/vpc-beta)
* [FAQs for client-to-site VPN servers](/docs/vpc?topic=vpc-faqs-vpn-server)
* [Troubleshooting client-to-site VPN servers](/docs/vpc?topic=vpc-why-do-i-get-an-authentication-error-user-authentication-failed-when-connecting-to-vpn-server)