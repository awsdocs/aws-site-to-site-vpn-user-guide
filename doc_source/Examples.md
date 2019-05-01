# Site\-to\-Site VPN Configuration Examples<a name="Examples"></a>

The following diagrams illustrate single and multiple Site\-to\-Site VPN connections\. The VPC has an attached virtual private gateway, and your remote network includes a customer gateway, which you must configure to enable the Site\-to\-Site VPN connection\. You set up the routing so that any traffic from the VPC bound for your network is routed to the virtual private gateway\.

When you create multiple Site\-to\-Site VPN connections to a single VPC, you can configure a second customer gateway to create a redundant connection to the same external location\. You can also use it to create Site\-to\-Site VPN connections to multiple geographic locations\.

## Single Site\-to\-Site VPN Connection<a name="SingleVPN"></a>

![\[VPN layout\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/VPN_Basic_Diagram.png)

## Single Site\-to\-Site VPN Connection with a Transit Gateway<a name="SingleVPN-transit-gateway"></a>

![\[Single Site-to-Site VPN Connection with a Transit Gateway\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/site-site-transit-gateway-basic.png)

## Multiple Site\-to\-Site VPN Connections<a name="MultipleVPN"></a>

![\[Multiple Site-to-Site VPN layout\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Branch_Offices_diagram.png)

## Multiple Site\-to\-Site VPN Connections with a Transit Gateway<a name="MultipleVPN-transit-gateway"></a>

![\[Multiple Site-to-Site VPN connections with a Transit Gateway\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/branch-off-transit-gateway.png)