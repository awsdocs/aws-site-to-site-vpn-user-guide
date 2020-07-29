# Providing secure communication between sites using VPN CloudHub<a name="VPN_CloudHub"></a>

If you have multiple AWS Site\-to\-Site VPN connections, you can provide secure communication between sites using the AWS VPN CloudHub\. This enables your remote sites to communicate with each other, and not just with the VPC\. The VPN CloudHub operates on a simple hub\-and\-spoke model that you can use with or without a VPC\. This design is suitable if you have multiple branch offices and existing internet connections and would like to implement a convenient, potentially low\-cost hub\-and\-spoke model for primary or backup connectivity between these remote offices\.

The sites must not have overlapping IP ranges\.

## Overview<a name="vpn-cloudhub-overview"></a>

The following diagram shows the VPN CloudHub architecture, with blue dashed lines indicating network traffic between remote sites being routed over their Site\-to\-Site VPN connections\.

![\[CloudHub diagram\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/AWS_VPN_CloudHub-diagram.png)

For this scenario, do the following:

1. Create a single virtual private gateway\.

1. Create multiple customer gateways, each with the public IP address of the gateway\. You must use a unique Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) for each customer gateway\. 

1. Create a dynamically routed Site\-to\-Site VPN connection from each customer gateway to the common virtual private gateway\. 

1. Configure the customer gateway devices to advertise a site\-specific prefix \(such as 10\.0\.0\.0/24, 10\.0\.1\.0/24\) to the virtual private gateway\. These routing advertisements are received and re\-advertised to each BGP peer, enabling each site to send data to and receive data from the other sites\. This is done using the network statements in the VPN configuration files for the Site\-to\-Site VPN connection\. The network statements differ slightly depending on the type of router you use\.

1. Configure the routes in your subnet route tables to enable instances in your VPC to communicate with your sites\. For more information, see [\(Virtual private gateway\) Enable route propagation in your route table](SetUpVPNConnections.md#vpn-configure-routing)\. You can configure an aggregate route in your route table \(for example, 10\.0\.0\.0/16\)\. Use more specific prefixes between customer gateways devices and the virtual private gateway\.

Sites that use AWS Direct Connect connections to the virtual private gateway can also be part of the AWS VPN CloudHub\. For example, your corporate headquarters in New York can have an AWS Direct Connect connection to the VPC and your branch offices can use Site\-to\-Site VPN connections to the VPC\. The branch offices in Los Angeles and Miami can send and receive data with each other and with your corporate headquarters, all using the AWS VPN CloudHub\. 

## Pricing<a name="vpn-cloudhub-pricing"></a>

To use AWS VPN CloudHub, you pay typical Amazon VPC Site\-to\-Site VPN connection rates\. You are billed the connection rate for each hour that each VPN is connected to the virtual private gateway\. When you send data from one site to another using the AWS VPN CloudHub, there is no cost to send data from your site to the virtual private gateway\. You only pay standard AWS data transfer rates for data that is relayed from the virtual private gateway to your endpoint\. 

For example, if you have a site in Los Angeles and a second site in New York and both sites have a Site\-to\-Site VPN connection to the virtual private gateway, you pay the per hour rate for each Site\-to\-Site VPN connection \(so if the rate was $\.05 per hour, it would be a total of $\.10 per hour\)\. You also pay the standard AWS data transfer rates for all data that you send from Los Angeles to New York \(and vice versa\) that traverses each Site\-to\-Site VPN connection\. Network traffic sent over the Site\-to\-Site VPN connection to the virtual private gateway is free but network traffic sent over the Site\-to\-Site VPN connection from the virtual private gateway to the endpoint is billed at the standard AWS data transfer rate\. 

For more information, see [Site\-to\-Site VPN Connection Pricing](http://aws.amazon.com/vpn/pricing/)\.