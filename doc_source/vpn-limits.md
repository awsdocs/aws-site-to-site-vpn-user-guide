# Site\-to\-Site VPN quotas<a name="vpn-limits"></a>

Your AWS account has the following quotas, formerly referred to as limits, related to Site\-to\-Site VPN\. Unless otherwise noted, each quota is Region\-specific\. You can request increases for some quotas, and other quotas cannot be increased\.

To request a quota increase for an adjustable quota, choose **Yes** in the **Adjustable** column\. For more information, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

## Site\-to\-Site VPN resources<a name="vpn-quotas-resources"></a>


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Customer gateways per Region | 50 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-4FB7FF5D) | 
| Virtual private gateways per Region | 5 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-7029FAB6) | 
| Site\-to\-Site VPN connections per Region | 50 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-3E6EC3A3) | 
| Accelerated Site\-to\-Site VPN connections per Region | 10 | Yes | 
| Site\-to\-Site VPN connections per virtual private gateway | 10 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-B91E5754) | 

You can attach one virtual private gateway to a VPC at a time\. To connect the same Site\-to\-Site VPN connection to multiple VPCs, we recommend that you explore using a transit gateway instead\. For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.

Site\-to\-Site VPN connections on a transit gateway are subject to the total transit gateway attachments limit\. For more information, see [Transit gateway quotas](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-quotas.html)\.

## Routes<a name="vpn-quotas-routes"></a>

Advertised route sources include VPC routes, other VPN routes, and routes from AWS Direct Connect virtual interfaces\. Advertised routes come from the route table that's associated with the VPN attachment\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Dynamic routes advertised from a customer gateway device to a Site\-to\-Site VPN connection on a virtual private gateway | 100 | No | 
| Routes advertised from a Site\-to\-Site VPN connection on a virtual private gateway to a customer gateway device | 1,000 | No | 
| Dynamic routes advertised from a customer gateway device to a Site\-to\-Site VPN connection on a transit gateway | 1,000 | No | 
| Routes advertised from a Site\-to\-Site VPN connection on a transit gateway to a customer gateway device | 5,000 | No | 
| Static routes from a customer gateway device to a Site\-to\-Site VPN connection on a virtual private gateway | 100 | No | 

## Bandwidth and throughput<a name="vpn-quotas-bandwidth"></a>

There are many factors that can affect realized bandwidth through a Site\-to\-Site VPN connection, including but not limited to: packet size, traffic mix \(TCP/UDP\), shaping or throttling policies on intermediate networks, internet weather, and specific application requirements\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Maximum bandwidth per VPN tunnel | Up to 1\.25 Gbps | No | 
| Maximum packets per second \(PPS\) per VPN tunnel | Up to 140,000 | No | 

For Site\-to\-Site VPN connections on a transit gateway, you can use ECMP to get higher VPN bandwidth by aggregating multiple VPN tunnels\. To use ECMP, the VPN connection must be configured for dynamic routing\. ECMP is not supported on VPN connections that use static routing\. For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html)\.

## Maximum transmission unit \(MTU\)<a name="vpn-quotas-mtu"></a>

Site\-to\-Site VPN supports a maximum transmission unit \(MTU\) of 1446 bytes and a corresponding maximum segment size \(MSS\) of 1406 bytes\. However, certain algorithms that use larger TCP headers can effectively reduce that maximum value\. To avoid fragmentation, we recommend that you set the MTU and MSS based on the algorithms selected\. For more details on MTU, MSS, and the optimal values, see [Best practices for your customer gateway device](your-cgw.md#cgw-best-practice)\.

Jumbo frames are not supported\. For more information, see [Jumbo frames](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html#jumbo_frame_instances) in the *Amazon EC2 User Guide for Linux Instances*\.

A Site\-to\-Site VPN connection does not support Path MTU Discovery\.

## Additional quota resources<a name="vpn-quotas-additional"></a>

For quotas related to transit gateways, including the number of attachments on a transit gateway, see [Quotas for your transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-limits.html) in the *Amazon VPC Transit Gateways Guide*\.

For additional VPC quotas, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.