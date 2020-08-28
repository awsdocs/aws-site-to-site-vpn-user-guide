# IPv4 and IPv6 traffic<a name="ipv4-ipv6"></a>

Your Site\-to\-Site VPN connection on a transit gateway can support either IPv4 traffic or IPv6 traffic inside the VPN tunnels\. By default, a Site\-to\-Site VPN connection supports IPv4 traffic inside the VPN tunnels\. You can configure a new Site\-to\-Site VPN connection to support IPv6 traffic inside the VPN tunnels\. Then, if your VPC and your on\-premises network are configured for IPv6 addressing, you can send IPv6 traffic over the VPN connection\.

If you enable IPv6 for the VPN tunnels for your Site\-to\-Site VPN connection, each tunnel has two CIDR blocks\. One is a size /30 IPv4 CIDR block, and the other is a size /126 IPv6 CIDR block\.

The following rules apply:
+ IPv6 addresses are only supported for the inside IP addresses of the VPN tunnels\. The outside tunnel IP addresses for the AWS endpoints are IPv4 addresses, and the public IP address of your customer gateway must be an IPv4 address\.
+ Site\-to\-Site VPN connections on a virtual private gateway do not support IPv6\.
+ You cannot enable IPv6 support for an existing Site\-to\-Site VPN connection\.
+ A Site\-to\-Site VPN connection cannot support both IPv4 and IPv6 traffic\.

For more information about creating a VPN connection, see [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)\.