---

copyright:
  years: 2021
lastupdated: "2021-08-26"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:important: .important}
{:deprecated: .deprecated}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic”}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Disconnecting VPN clients (Beta)
{: #vpn-client-to-site-connections}

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

Connections are VPN sessions that are established by VPN clients. After a VPN client connects to the VPN server, you can view all VPN clients that connected to the server in the last hour. VPN client information includes the client IP, user ID, status, remote IP, remote port, and session start/end time (if applicable). 

## Disconnecting VPN clients using the UI
{: #vpn-client-to-site-ending-connections}
{: ui}

To disconnect a VPN client from the VPN server, follow these steps:

1. Navigate to the [VPNs for VPC](https://cloud.ibm.com/vpc-ext/network/vpngateways){: external} page and click the **Client-to-site servers** tab. 
1. Click the name of the VPN server to display its details.
1. Scroll to the Clients section to view VPN clients that connected in the last hour.
1. Click the overflow menu ![overflow menu](images/overflow.png) next to the client you want to disconnect, then click **Disconnect**. The disconnected VPN client is automatically deleted after one hour.

   ![Disconnect a VPN client](images/vpn-client-section.png "VPN client connections")

   You can also specify to **Delete** the VPN client, which deletes the client session immediately.
   {: note}

## Disconnecting VPN clients using the CLI
{: #vpn-client-to-site-ending-connections-cli}
{: cli}

To disconnect a VPN client by using the CLI, enter the following command:

The disconnected VPN client is automatically deleted after one hour. To automatically delete a VPN client, use the **ibmcloud is vpn-server-client-delete** command.
{: note}

```
ibmcloud is vpn-server-client-disconnect VPN_SERVER_ID (CLIENT_ID1 CLIENT_ID2 ...) [-f, --force] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: is the ID of the VPN server.
- **CLIENT_ID1**: is the ID of the VPN route.
- **CLIENT_ID2**: is the ID of the VPN route. 
- **--force, -f**: is the force operation without confirmation.
- **--quiet, -q**: suppresses verbose output.

For example:

```
ic is vpn-server-client-disconnect r134-46ca4654-fe57-431c-9f5a-1c82773b6e83 86b1f0cc-6e83-45e5-bd78-1bef291be6e7
This will disconnect VPN client 86b1f0cc-16b0-45e5-bd78-1bef291be6e7 and cannot be undone. Continue [y/N] ?> y
Disconnect VPN client 86b1f0cc-16b0-45e5-bd78-1bef291be6e7 under account IBM as user terry@ibm.com...
OK
Disconnection request for VPN client 86b1f0cc-6e83-45e5-bd78-1bef291be6e7 has been accepted.
```
{: screen}

## Disconnecting VPN clients using the API
{: #vpn-client-to-site-ending-connections-api}
{: api}

To disconnect a VPN client by using the API, follow these steps:

The disconnected VPN client is automatically deleted after one hour.
{: note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands; for example:

   ```sh
   export vpnServerID=<your_vpn_server_id>
   export vpnClientID=<your_vpn_client_id>
   ```
   {: pre}

1. When all variables are initiated, disconnect the VPN client:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/vpn_servers/$vpnServerID/clients/$vpnClientID/disconnect?version=$api_version&maturity=beta&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}