# Data protection in AWS Site\-to\-Site VPN<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS Site\-to\-Site VPN\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual users with AWS IAM Identity Center \(successor to AWS Single Sign\-On\) or AWS Identity and Access Management \(IAM\)\. That way, each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing sensitive data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form text fields such as a **Name** field\. This includes when you work with Site\-to\-Site VPN or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form text fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.



## Internetwork traffic privacy<a name="internetwork-traffic-privacy"></a>

A Site\-to\-Site VPN connection privately connects your VPC to your on\-premises network\. Data that's transferred between your VPC and your network routes over an encrypted VPN connection to help maintain the confidentiality and integrity of the data in transit\. Amazon supports Internet Protocol security \(IPsec\) VPN connections\. IPsec is a protocol suite for securing IP communications by authenticating and encrypting each IP packet in a data stream\. 

Each Site\-to\-Site VPN connection consists of two encrypted IPsec VPN tunnels that link AWS and your network\. Traffic in each tunnel can be encrypted with AES128 or AES256 and use Diffie\-Hellman groups for key exchange, providing Perfect Forward Secrecy\. AWS authenticates with SHA1 or SHA2 hashing functions\. 

Instances in your VPC do not require a public IP address to connect to resources on the other side of your Site\-to\-Site VPN connection\. Instances can route their internet traffic through the Site\-to\-Site VPN connection to your on\-premises network\. They can then access the internet through your existing outbound traffic points and your network security and monitoring devices\.

See the following topics for more information:
+ [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md): Provides information about the IPsec and Internet Key Exchange \(IKE\) options that are available for each tunnel\.
+ [Site\-to\-Site VPN tunnel authentication options](vpn-tunnel-authentication-options.md): Provides information about the authentication options for your VPN tunnel endpoints\.
+ [Requirements for your customer gateway device](your-cgw.md#CGRequirements): Provides information about the requirements for the customer gateway device on your side of the VPN connection\.
+ [Providing secure communication between sites using VPN CloudHub](VPN_CloudHub.md): If you have multiple Site\-to\-Site VPN connections, you can provide secure communication between your on\-premises sites by using the AWS VPN CloudHub\. 