# Site\-to\-Site VPN single and multiple connection examples<a name="Examples"></a>

The following diagrams illustrate single and multiple Site\-to\-Site VPN connections\. 

## Single Site\-to\-Site VPN connection<a name="SingleVPN"></a>

The VPC has an attached virtual private gateway, and your on\-premises \(remote\) network includes a customer gateway device, which you must configure to enable the Site\-to\-Site VPN connection\. You set up the routing so that any traffic from the VPC bound for your network is routed to the virtual private gateway\.

![\[VPN layout\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/vpn-basic-diagram.png)

For steps to set up this scenario, see [Getting started](SetUpVPNConnections.md)\.

## Single Site\-to\-Site VPN connection with a transit gateway<a name="SingleVPN-transit-gateway"></a>

The VPC has an attached transit gateway, and your on\-premises \(remote\) network includes a customer gateway device, which you must configure to enable the Site\-to\-Site VPN connection\. You set up the routing so that any traffic from the VPC bound for your network is routed to the transit gateway\.

![\[Single Site-to-Site VPN connection with a transit gateway\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/site-site-transit-gateway-basic.png)

For steps to set up this scenario, see [Getting started](SetUpVPNConnections.md)\.

## Multiple Site\-to\-Site VPN connections<a name="MultipleVPN"></a>

The VPC has an attached virtual private gateway, and you have multiple Site\-to\-Site VPN connections to multiple on\-premises locations\. You set up the routing so that any traffic from the VPC bound for your networks is routed to the virtual private gateway\.

You can also use this scenario to create Site\-to\-Site VPN connections to multiple geographic locations and provide secure communication between sites\. For more information, see [Providing secure communication between sites using VPN CloudHub](VPN_CloudHub.md)\.

![\[Multiple Site-to-Site VPN layout\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/branch-offices-diagram.png)

When you create multiple Site\-to\-Site VPN connections to a single VPC, you can configure a second customer gateway to create a redundant connection to the same external location\. For more information, see [Using redundant Site\-to\-Site VPN connections to provide failover](vpn-redundant-connection.md)\.

## Multiple Site\-to\-Site VPN connections with a transit gateway<a name="MultipleVPN-transit-gateway"></a>

The VPC has an attached transit gateway, and you have multiple Site\-to\-Site VPN connections to multiple on\-premises locations\. You set up the routing so that any traffic from the VPC bound for your networks is routed to the transit gateway\.

You can also use this scenario to create Site\-to\-Site VPN connections to multiple geographic locations and provide secure communication between sites\.

![\[Multiple Site-to-Site VPN connections with a transit gateway\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/branch-off-transit-gateway.png)

When you create multiple Site\-to\-Site VPN connections to a single transit gateway, you can configure a second customer gateway to create a redundant connection to the same external location\. 

## Site\-to\-Site VPN connection with AWS Direct Connect<a name="vpn-direct-connect"></a>

The VPC has an attached virtual private gateway, and connects to your on\-premises \(remote\) network through AWS Direct Connect\. You can configure an AWS Direct Connect public virtual interface to establish a dedicated network connection between your network to public AWS resources through a virtual private gateway\. You set up the routing so that any traffic from the VPC bound for your network routes to the virtual private gateway and the AWS Direct Connect connection\. 

**Note**  
When both AWS Direct Connect and the VPN connection are set up on the same virtual private gateway, adding or removing objects might cause the virtual private gateway to enter the ‘attaching’ state\. This indicates a change is being made to internal routing that will switch between AWS Direct Connect and the VPN connection to minimize interruptions and packet loss\. When this is complete, the virtual private gateway returns to the ‘attached’ state\.

![\[Site-to-Site VPN connection with AWS Direct Connect\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/vpn-direct-connect.png)

## Private IP Site\-to\-Site VPN connection with AWS Direct Connect<a name="private-ip-direct-connect"></a>

With a private IP Site\-to\-Site VPN you can encrypt AWS Direct Connect traffic between your on\-premises network and AWS without the use of public IP addresses\. Private IP VPN over AWS Direct Connect ensures that traffic between AWS and on\-premises networks is both secure and private, allowing customers to comply with regulatory and security mandates\.

![\[Private IP Site-to-Site VPN connection with AWS Direct Connect\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/private-ip-dx.png)