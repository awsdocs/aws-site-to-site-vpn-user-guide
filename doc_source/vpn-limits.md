# Site\-to\-Site VPN quotas<a name="vpn-limits"></a>

Your AWS account has the following quotas, formerly referred to as limits, related to Site\-to\-Site VPN\. To request an increase, use the [limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=)\.
+ Customer gateways per Region: 50
+ Virtual private gateways per Region: 5

  You can attach only one virtual private gateway to a VPC at a time\. 
+ Routes advertised to a Site\-to\-Site VPN connection from a customer gateway device: 100
  Routes (dynamic BGP/propagated) advertised to a Site\-to\-Site VPN connection (toward a virtual private gateway or transit gateway) from a customer gateway device: 100
 
  This quota cannot be increased\.
+ Routes advertised by a Site\-to\-Site VPN connection to a customer gateway device: 1000

  This quota cannot be increased\.
+ Site\-to\-Site VPN connections per Region: 50
+ Site\-to\-Site VPN connections per virtual private gateway: 10

For quotas related to transit gateways, including the number of attachments on a transit gateway, see [Quotas for your transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-limits.html) in the *Amazon VPC Transit Gateways Guide*\.

For additional VPC quotas, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.
