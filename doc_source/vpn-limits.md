# Site\-to\-Site VPN Limits<a name="vpn-limits"></a>

Your AWS account has the following limits, also known as quotas, related to Site\-to\-Site VPN\. To request an increase, use the [limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=)\.
+ Customer gateways per Region: 50
+ Virtual private gateways per Region: 5

  You can attach only one virtual private gateway to a VPC at a time\. 
+ BGP advertised routes per route table \(propagated routes\): 100

  This limit cannot be increased\. If you require more than 100 prefixes, advertise a default route\.
+ Site\-to\-Site VPN connections per Region: 50
+ Site\-to\-Site VPN connections per virtual private gateway: 10

For limits related to using VPN connections with a transit gateway, see [Limits for Your Transit Gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-limits.html) in the *Amazon VPC Transit Gateways Guide*\.

For additional VPC limits, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.