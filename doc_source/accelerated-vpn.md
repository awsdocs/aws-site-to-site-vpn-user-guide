# Accelerated Site\-to\-Site VPN connections<a name="accelerated-vpn"></a>

You can optionally enable acceleration for your Site\-to\-Site VPN connection\. An accelerated Site\-to\-Site VPN connection \(accelerated VPN connection\) uses AWS Global Accelerator to route traffic from your on\-premises network to an AWS edge location that is closest to your customer gateway device\. AWS Global Accelerator optimizes the network path, using the congestion\-free AWS global network to route traffic to the endpoint that provides the best application performance \(for more information, see [AWS Global Accelerator](https://aws.amazon.com/global-accelerator/)\)\. You can use an accelerated VPN connection to avoid network disruptions that might occur when traffic is routed over the public internet\.

When you create an accelerated VPN connection, we create and manage two accelerators on your behalf, one for each VPN tunnel\. You cannot view or manage these accelerators yourself by using the AWS Global Accelerator console or APIs\.

For information about the AWS Regions that support Accelerated VPN connections, see the [AWS Accelerated Site\-to\-Site VPN FAQs](http://aws.amazon.com/vpn/faqs/)\.

## Enabling acceleration<a name="accelerated-vpn-enabling"></a>

By default, when you create a Site\-to\-Site VPN connection, acceleration is disabled\. You can optionally enable acceleration when you create a new Site\-to\-Site VPN attachment on a transit gateway\. For more information and steps, see [Creating a transit gateway VPN attachment](create-tgw-vpn-attachment.md)\.

Accelerated VPN connections use a separate pool of IP addresses for the tunnel endpoint IP addresses\. The IP addresses for the two VPN tunnels are selected from two separate [network zones](https://docs.aws.amazon.com/global-accelerator/latest/dg/introduction-components.html)\.

## Rules and restrictions<a name="accelerated-vpn-rules"></a>

To use an accelerated VPN connection, the following rules apply:
+ Acceleration is only supported for Site\-to\-Site VPN connections that are attached to a transit gateway\. Virtual private gateways do not support accelerated VPN connections\.
+ An Accelerated Site\-to\-Site VPN connection cannot be used with an AWS Direct Connect public virtual interface\.
+ You cannot turn on or turn off acceleration for an existing Site\-to\-Site VPN connection\. Instead, you can create a new Site\-to\-Site VPN connection with acceleration on or off as needed\. Then, configure your customer gateway device to use the new Site\-to\-Site VPN connection and delete the old Site\-to\-Site VPN connection\. 
+ NAT\-traversal \(NAT\-T\) is required for an accelerated VPN connection and is enabled by default\. If you downloaded a [configuration file](SetUpVPNConnections.md#vpn-download-config) from the Amazon VPC console, check the NAT\-T setting and adjust it if necessary\.
+ IKE rekeys for accelerated VPN tunnels must be initiated from the customer gateway device to keep the tunnels up\.
+ Site\-to\-Site VPN connections that use certificate\-based authentication might not be compatible with AWS Global Accelerator, due to limited support for packet fragmentation in Global Accelerator\. For more information, see [How AWS Global Accelerator works](https://docs.aws.amazon.com/global-accelerator/latest/dg/introduction-how-it-works.html)\. If you require an accelerated VPN connection that uses certificate\-based authentication, then your customer gateway device must support IKE fragmentation\. Otherwise, do not enable your VPN for acceleration\.