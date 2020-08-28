# Site\-to\-Site VPN routing options<a name="VPNRoutingTypes"></a>

When you create a Site\-to\-Site VPN connection, you must do the following:
+ Specify the type of routing that you plan to use \(static or dynamic\)
+ Update the [route table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) for your subnet

There are quotas on the number of routes that you can add to a route table\. For more information, see the Route Tables section in [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.

**Topics**
+ [Static and dynamic routing](#vpn-static-dynamic)
+ [Route tables and VPN route priority](#vpn-route-priority)
+ [Routing during VPN tunnel endpoint updates](#routing-vpn-tunnel-updates)
+ [IPv4 and IPv6 traffic](ipv4-ipv6.md)

## Static and dynamic routing<a name="vpn-static-dynamic"></a>

The type of routing that you select can depend on the make and model of your customer gateway device\. If your customer gateway device supports Border Gateway Protocol \(BGP\), specify dynamic routing when you configure your Site\-to\-Site VPN connection\. If your customer gateway device does not support BGP, specify static routing\. For a list of static and dynamic routing devices that have been tested with Site\-to\-Site VPN, see [Customer gateway devices that we've tested](your-cgw.md#DevicesTested)\.

If you use a device that supports BGP advertising, you don't specify static routes to the Site\-to\-Site VPN connection because the device uses BGP to advertise its routes to the virtual private gateway\. If you use a device that doesn't support BGP advertising, you must select static routing and enter the routes \(IP prefixes\) for your network that should be communicated to the virtual private gateway\. 

We recommend that you use BGP\-capable devices, when available, because the BGP protocol offers robust liveness detection checks that can assist failover to the second VPN tunnel if the first tunnel goes down\. Devices that don't support BGP may also perform health checks to assist failover to the second tunnel when needed\.

You must configure your customer gateway device to route traffic from your on\-premises network to the Site\-to\-Site VPN connection\. The configuration depends on the make and model of your device\. For more information, see [Your customer gateway device](your-cgw.md)\.

## Route tables and VPN route priority<a name="vpn-route-priority"></a>

[Route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) determine where network traffic from your VPC is directed\. In your VPC route table, you must add a route for your remote network and specify the virtual private gateway as the target\. This enables traffic from your VPC that's destined for your remote network to route via the virtual private gateway and over one of the VPN tunnels\. You can enable route propagation for your route table to automatically propagate your network routes to the table for you\. 

We use the most specific route in your route table that matches the traffic to determine how to route the traffic \(longest prefix match\)\. If your route table has overlapping or matching routes, the following rules apply:
+ If propagated routes from a Site\-to\-Site VPN connection or AWS Direct Connect connection overlap with the local route for your VPC, the local route is most preferred even if the propagated routes are more specific\. 
+ If propagated routes from a Site\-to\-Site VPN connection or AWS Direct Connect connection have the same destination CIDR block as other existing static routes \(longest prefix match cannot be applied\), we prioritize the static routes whose targets are an internet gateway, a virtual private gateway, a network interface, an instance ID, a VPC peering connection, a NAT gateway, a transit gateway, or a gateway VPC endpoint\.

For example, the following route table has a static route to an internet gateway, and a propagated route to a virtual private gateway\. Both routes have a destination of `172.31.0.0/24`\. In this case, all traffic destined for `172.31.0.0/24` is routed to the internet gateway — it is a static route and therefore takes priority over the propagated route\.


| Destination | Target | 
| --- | --- | 
| 10\.0\.0\.0/16 | Local | 
| 172\.31\.0\.0/24 | vgw\-11223344556677889 \(propagated\) | 
| 172\.31\.0\.0/24 | igw\-12345678901234567 \(static\) | 

Only IP prefixes that are known to the virtual private gateway, whether through BGP advertisements or a static route entry, can receive traffic from your VPC\. The virtual private gateway does not route any other traffic destined outside of received BGP advertisements, static route entries, or its attached VPC CIDR\. Virtual private gateways do not support IPv6 traffic\.

When a virtual private gateway receives routing information, it uses path selection to determine how to route traffic\. Longest prefix match applies\. If the prefixes are the same, then the virtual private gateway prioritizes routes as follows, from most preferred to least preferred: 
+ BGP propagated routes from an AWS Direct Connect connection 
+ Manually added static routes for a Site\-to\-Site VPN connection
+ BGP propagated routes from a Site\-to\-Site VPN connection
+ For matching prefixes where each Site\-to\-Site VPN connection uses BGP, the AS PATH is compared and the prefix with the shortest AS PATH is preferred\.
**Note**  
We do not recommend using AS PATH prepending, to ensure that both tunnels have equal AS PATH\.
+ When the AS PATHs are the same length and if the first AS in the AS\_SEQUENCE is the same across multiple paths, multi\-exit discriminators \(MEDs\) are compared\. The path with the lowest MED value is preferred\.

Route priority is affected during [VPN tunnel endpoint updates](#routing-vpn-tunnel-updates)\.

For a Site\-to\-Site VPN connection on a virtual private gateway, AWS selects one of the two redundant tunnels as the primary egress path\. This selection may change at times, and we strongly recommend that you configure both tunnels for high availability\.

For Site\-to\-Site VPN connections that use BGP, the primary tunnel can be identified by the multi\-exit discriminator \(MED\) value\. We recommend advertising more specific BGP routes to influence routing decisions\. 

For Site\-to\-Site VPN connections that use static routing, the primary tunnel can be identified by traffic statistics or metrics\. 

To use both tunnels, we recommend exploring Equal Cost Multipath \(ECMP\), which is supported for Site\-to\-Site VPN connections on a transit gateway\. For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.

## Routing during VPN tunnel endpoint updates<a name="routing-vpn-tunnel-updates"></a>

A Site\-to\-Site VPN connection consists of two VPN tunnels between a customer gateway device and a virtual private gateway or a transit gateway\. We recommend that you configure both tunnels for redundancy\. Your VPN connection may experience a brief loss of redundancy when we perform tunnel endpoint updates on one of the two tunnels\. Tunnel endpoint updates can occur for several reasons, including health reasons, software upgrades, and retirement of underlying hardware\. 

When we perform updates on one VPN tunnel, we set a lower outbound multi\-exit discriminator \(MED\) value on the other tunnel\. If you have configured your customer gateway device to use both tunnels, your VPN connection uses the other \(up\) tunnel during the tunnel endpoint update process\.

**Note**  
To ensure that the up tunnel with the lower MED is preferred, ensure that your customer gateway device uses the same Weight and Local Preference values for both tunnels \(Weight and Local Preference have higher priority than MED\)\.