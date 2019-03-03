# Site\-to\-Site VPN Routing Options<a name="VPNRoutingTypes"></a>

When you create a Site\-to\-Site VPN connection, you must do the following:
+ Specify the type of routing that you plan to use \(static or dynamic\)
+ Update the route table for your subnet

There are limits on the number of routes that you can add to a route table\. For more information, see the Route Tables section in [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.

## Static and Dynamic Routing<a name="vpn-static-dynamic"></a>

The type of routing that you select can depend on the make and model of your VPN devices\. If your VPN device supports Border Gateway Protocol \(BGP\), specify dynamic routing when you configure your Site\-to\-Site VPN connection\. If your device does not support BGP, specify static routing\. For a list of static and dynamic routing devices that have been tested with Amazon VPC, see the [Amazon Virtual Private Cloud FAQs](http://aws.amazon.com/vpn/faqs/#C9)\.

When you use a BGP device, you don't need to specify static routes to the Site\-to\-Site VPN connection because the device uses BGP to advertise its routes to the virtual private gateway\. If you use a device that supports BGP advertising, then you cannot specify static routes, If you use a device that doesn't support BGP, you must select static routing and enter the routes \(IP prefixes\) for your network that should be communicated to the virtual private gateway\. 

We recommend that you use BGP\-capable devices, when available, because the BGP protocol offers robust liveness detection checks that can assist failover to the second VPN tunnel if the first tunnel goes down\. Devices that don't support BGP may also perform health checks to assist failover to the second tunnel when needed\.

## Route Tables and VPN Route Priority<a name="vpn-route-priority"></a>

Route tables determine where network traffic is directed\. In your route table, you must add a route for your remote network and specify the virtual private gateway as the target\. This enables traffic from your VPC that's destined for your remote network to route via the virtual private gateway and over one of the VPN tunnels\. You can enable route propagation for your route table to automatically propagate your network routes to the table for you\.

Only IP prefixes that are known to the virtual private gateway, whether through BGP advertisements or static route entry, can receive traffic from your VPC\. The virtual private gateway does not route any other traffic destined outside of received BGP advertisements, static route entries, or its attached VPC CIDR\.

When a virtual private gateway receives routing information, it uses path selection to determine how to route traffic to your remote network\. Longest prefix match applies; otherwise, the following rules apply:
+ If any propagated routes from a Site\-to\-Site VPN connection or AWS Direct Connect connection overlap with the local route for your VPC, the local route is most preferred even if the propagated routes are more specific\. 
+ If any propagated routes from a Site\-to\-Site VPN connection or AWS Direct Connect connection have the same destination CIDR block as other existing static routes \(longest prefix match cannot be applied\), we prioritize the static routes whose targets are an Internet gateway, a virtual private gateway, a network interface, an instance ID, a VPC peering connection, a NAT gateway, or a VPC endpoint\.

If you have overlapping routes within a Site\-to\-Site VPN connection and longest prefix match cannot be applied, then we prioritize the routes as follows in the Site\-to\-Site VPN connection, from most preferred to least preferred: 
+ BGP propagated routes from an AWS Direct Connect connection 
+ Manually added static routes for a Site\-to\-Site VPN connection
+ BGP propagated routes from a Site\-to\-Site VPN connection

In this example, your route table has a static route to an internet gateway \(that you added manually\), and a propagated route to a virtual private gateway\. Both routes have a destination of `172.31.0.0/24`\. In this case, all traffic destined for `172.31.0.0/24` is routed to the internet gateway — it is a static route and therefore takes priority over the propagated route\.


| Destination | Target | 
| --- | --- | 
| 10\.0\.0\.0/16 | Local | 
| 172\.31\.0\.0/24 | vgw\-1a2b3c4d \(propagated\) | 
| 172\.31\.0\.0/24 | igw\-11aa22bb | 