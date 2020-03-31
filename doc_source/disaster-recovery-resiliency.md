# Resilience in AWS Site\-to\-Site VPN<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS global infrastructure, Site\-to\-Site VPN offers features to help support your data resiliency and backup needs\.

## Two tunnels per VPN connection<a name="resiliancy-tunnels"></a>

A Site\-to\-Site VPN connection has two tunnels to provide increased availability to your VPC\. If there's a device failure within AWS, your VPN connection automatically fails over to the second tunnel so that your access isn't interrupted\. From time to time, AWS also performs routine maintenance on your virtual private gateway, which may briefly disable one of the two tunnels of your VPN connection\. Your VPN connection automatically fails over to the second tunnel while this maintenance is performed\. When you configure your customer gateway, it's therefore important that you configure both tunnels\.

## Redundancy<a name="resiliancy-redundancy"></a>

To protect against a loss of connectivity in case your customer gateway becomes unavailable, you can set up a second Site\-to\-Site VPN connection\. For more information, see the following topics:
+ [Using redundant Site\-to\-Site VPN connections to provide failover](vpn-redundant-connection.md)
+ [Single data center HA network connectivity](https://aws.amazon.com/answers/networking/aws-single-data-center-ha-network-connectivity/)
+ [Multiple data center HA network connectivity](https://aws.amazon.com/answers/networking/aws-multiple-data-center-ha-network-connectivity/)