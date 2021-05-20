# Site\-to\-Site VPN quotas<a name="vpn-limits"></a>

Your AWS account has the following quotas, formerly referred to as limits, related to Site\-to\-Site VPN\. To request an increase, use the [limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=)\.

## Site\-to\-Site VPN resources<a name="vpn-quotas-resources"></a>
+ Customer gateways per Region: 50
+ Virtual private gateways per Region: 5

  You can attach only one virtual private gateway to a VPC at a time\. To connect the same Site\-to\-Site VPN connection to multiple VPCs, we recommend that you explore using a transit gateway instead\. For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.
+ Site\-to\-Site VPN connections per Region: 50
+ Site\-to\-Site VPN connections per virtual private gateway: 10

## Routes<a name="vpn-quotas-routes"></a>
+ Dynamic routes advertised from a customer gateway device to a Site\-to\-Site VPN connection on a virtual private gateway: 100

  This quota cannot be increased\.
+ Routes advertised from a Site\-to\-Site VPN connection on a virtual private gateway to a customer gateway device: 1,000

  Advertised route sources include VPC routes, other VPN routes, and routes from AWS Direct Connect virtual interfaces\.

  This quota cannot be increased\.
+ Dynamic routes advertised from a customer gateway device to a Site\-to\-Site VPN connection on a transit gateway: 1,000
+ Routes advertised from a Site\-to\-Site VPN connection on a transit gateway to a customer gateway device: 5,000

  Advertised routes come from the route table that's associated with the VPN attachment\.

## Bandwidth and throughput<a name="vpn-quotas-bandwidth"></a>
+ Maximum bandwidth per VPN tunnel: 1\.25 Gbps

  This quota cannot be increased\. For Site\-to\-Site VPN connections on a transit gateway, you can use ECMP to get higher VPN bandwidth by aggregating multiple VPN tunnels\. To use ECMP, the VPN connection must be configured for dynamic routing\. ECMP is not supported on VPN connections that use static routing\. For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html)\.
+ Maximum packets per second \(PPS\) per VPN tunnel: 140,000

## Maximum transmission unit \(MTU\)<a name="vpn-quotas-mtu"></a>
+ You must set the MTU of the logical interface for your customer gateway device to 1399 bytes\. For more information, see [Requirements for your customer gateway device](your-cgw.md#CGRequirements)\. 

  Jumbo frames are not supported\. For more information, see [Jumbo frames](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html#jumbo_frame_instances) in the *Amazon EC2 User Guide for Linux Instances*\.
+ We recommend that you set the maximum segment size \(MSS\) on your customer gateway device to 1359 when using the SHA2\-384 or SHA2\-512 hashing algorithms\.

**Note**  
A Site\-to\-Site VPN connection does not support Path MTU Discovery\.

## Additional quota resources<a name="vpn-quotas-additional"></a>

For quotas related to transit gateways, including the number of attachments on a transit gateway, see [Quotas for your transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-limits.html) in the *Amazon VPC Transit Gateways Guide*\.

For additional VPC quotas, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.