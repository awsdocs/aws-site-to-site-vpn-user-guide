# Configuring the VPN Tunnels for Your Site\-to\-Site VPN Connection<a name="VPNTunnels"></a>

You use a Site\-to\-Site VPN connection to connect your remote network to a VPC\. Each Site\-to\-Site VPN connection has two tunnels, with each tunnel using a unique virtual private gateway public IP address\. It is important to configure both tunnels for redundancy\. When one tunnel becomes unavailable \(for example, down for maintenance\), network traffic is automatically routed to the available tunnel for that specific Site\-to\-Site VPN connection\.

The following diagram shows the two tunnels of the Site\-to\-Site VPN connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Multiple_VPN_Tunnels_diagram.png)

When you create a Site\-to\-Site VPN connection, you download a configuration file specific to your customer gateway device that contains information for configuring the device, including information for configuring each tunnel\. You can optionally specify some of the tunnel options yourself when you create the Site\-to\-Site VPN connection\. Otherwise, AWS provides default values\.

The following table describes the tunnel options that you can configure\.


| Item | Description | AWS\-provided default value | 
| --- | --- | --- | 
|  Inside tunnel CIDR  |  The range of inside IP addresses for the VPN tunnel\. You can specify a size /30 CIDR block from the `169.254.0.0/16` range\. The CIDR block must be unique across all Site\-to\-Site VPN connections that use the same virtual private gateway\. The following CIDR blocks are reserved and cannot be used:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/VPNTunnels.html)  |  A size /30 CIDR block from the `169.254.0.0/16` range\.  | 
|  Pre\-shared key \(PSK\)  |  The pre\-shared key \(PSK\) to establish the initial IKE Security Association between the virtual private gateway and customer gateway\.  The PSK must be between 8 and 64 characters in length and cannot start with zero \(0\)\. Allowed characters are alphanumeric characters, periods \(\.\), and underscores \(\_\)\.  |  A 32\-character alphanumeric string\.  | 

You cannot modify tunnel options after you create the Site\-to\-Site VPN connection\. To change the inside tunnel IP addresses or the PSKs for an existing connection, you must delete the Site\-to\-Site VPN connection and create a new one\. You cannot configure tunnel options for an AWS Classic VPN connection\.