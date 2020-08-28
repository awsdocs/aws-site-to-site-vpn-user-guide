# Site\-to\-Site VPN categories<a name="vpn-categories"></a>

Your Site\-to\-Site VPN connection is either an AWS Classic VPN connection or an AWS VPN connection\. Any new Site\-to\-Site VPN connection that you create is an AWS VPN connection\. The following features are supported on AWS VPN connections only:
+ Internet Key Exchange version 2 \(IKEv2\)
+ NAT traversal
+ 4\-byte ASN \(in addition to 2\-byte ASN\)
+ CloudWatch metrics
+ Reusable IP addresses for your customer gateways
+ Additional encryption options; including AES 256\-bit encryption, SHA\-2 hashing, and additional Diffie\-Hellman groups
+ Configurable tunnel options
+ Custom private ASN for the Amazon side of a BGP session
+ Private Certificate from a subordinate CA from AWS Certificate Manager Private Certificate Authority
+ Support for IPv6 traffic

For information about identifying and migrating your connection, see [Identifying a Site\-to\-Site VPN connection](identify-vpn.md) and [Migrating from AWS Classic VPN to AWS VPN](aws-vpn-migrate.md)\.