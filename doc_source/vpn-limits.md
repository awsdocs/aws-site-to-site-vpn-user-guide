# Site\-to\-Site VPN quotas<a name="vpn-limits"></a>

Your AWS account has the following quotas, formerly referred to as limits, related to Site\-to\-Site VPN\. To request an increase, use the [limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=)\.
+ Customer gateways per Region: 50
+ Virtual private gateways per Region: 5

  You can attach only one virtual private gateway to a VPC at a time\. 
+ Dynamic routes advertised from a customer gateway device to a Site\-to\-Site VPN connection \(on a transit gateway or virtual private gateway\): 100

  This quota cannot be increased\.
+ Routes advertised from a Site\-to\-Site VPN connection to a customer gateway device: 1,000

  For VPN connections on a virtual private gateway, advertised route sources include VPC routes, other VPN routes, and routes from AWS Direct Connect virtual interfaces\. For VPN connections on a transit gateway, advertised routes come from the route table that's associated with the VPN attachment\.

  This quota cannot be increased\. 
+ Site\-to\-Site VPN connections per Region: 50
+ Site\-to\-Site VPN connections per virtual private gateway: 10

For quotas related to transit gateways, including the number of attachments on a transit gateway, see [Quotas for your transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-limits.html) in the *Amazon VPC Transit Gateways Guide*\.

For additional VPC quotas, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.