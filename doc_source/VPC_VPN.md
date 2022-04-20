# What is AWS Site\-to\-Site VPN?<a name="VPC_VPN"></a>

By default, instances that you launch into an Amazon VPC can't communicate with your own \(remote\) network\. You can enable access to your remote network from your VPC by creating an AWS Site\-to\-Site VPN \(Site\-to\-Site VPN\) connection, and configuring routing to pass traffic through the connection\.

Although the term *VPN connection* is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on\-premises network\. Site\-to\-Site VPN supports Internet Protocol security \(IPsec\) VPN connections\.

Your Site\-to\-Site VPN connection is either an AWS Classic VPN or an AWS VPN\. For more information, see [Site\-to\-Site VPN categories](vpn-categories.md)\.

## Concepts<a name="concepts"></a>

The following are the key concepts for Site\-to\-Site VPN:
+ **VPN connection**: A secure connection between your on\-premises equipment and your VPCs\.
+ **VPN tunnel**: An encrypted link where data can pass from the customer network to or from AWS\.

  Each VPN connection includes two VPN tunnels which you can simultaneously use for high availability\.
+ **Customer gateway**: An AWS resource which provides information to AWS about your customer gateway device\. 
+ **Customer gateway device**: A physical device or software application on your side of the Site\-to\-Site VPN connection\.
+ **Target gateway**: A generic term for the VPN endpoint on the Amazon side of the Site\-to\-Site VPN connection\.
+ **Virtual private gateway**: A virtual private gateway is the VPN endpoint on the Amazon side of your Site\-to\-Site VPN connection that can be attached to a single VPC\.
+ **Transit gateway**: A transit hub that can be used to interconnect multiple VPCs and on\-premises networks, and as a VPN endpoint for the Amazon side of the Site\-to\-Site VPN connection\.

## Working with Site\-to\-Site VPN<a name="site-site-tools"></a>

You can create, access, and manage your Site\-to\-Site VPN resources using any of the following interfaces:
+ **AWS Management Console**— Provides a web interface that you can use to access your Site\-to\-Site VPN resources\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Amazon VPC, and is supported on Windows, macOS, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS SDKs** — Provide language\-specific APIs and takes care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](https://aws.amazon.com/tools/#SDKs)\.
+ **Query API**— Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Amazon VPC, but it requires that your application handle low\-level details such as generating the hash to sign the request, and error handling\. For more information, see the [Amazon EC2 API Reference](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/)\.

## Site\-to\-Site VPN limitations<a name="site-to-site-limitations"></a>

A Site\-to\-Site VPN connection has the following limitations\.
+ IPv6 traffic is not supported for VPN connections on a virtual private gateway\.
+ An AWS VPN connection does not support Path MTU Discovery\.

In addition, take the following into consideration when you use Site\-to\-Site VPN\.
+ When connecting your VPCs to a common on\-premises network, we recommend that you use non\-overlapping CIDR blocks for your networks\.

## Pricing<a name="pricing"></a>

You are charged for each VPN connection hour that your VPN connection is provisioned and available\. For more information, see [AWS Site\-to\-Site VPN and Accelerated Site\-to\-Site VPN Connection pricing](https://aws.amazon.com/vpn/pricing/#AWS_Site-to-Site_VPN_and_Accelerated_Site-to-Site_VPN_Connection_Pricing)\.

You are charged for data transfer out from Amazon EC2 to the internet\. For more information, see [Data Transfer](http://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer) on the Amazon EC2 On\-Demand Pricing page\.

When you create an accelerated VPN connection, we create and manage two accelerators on your behalf\. You are charged an hourly rate and data transfer costs for each accelerator\. For more information, see [AWS Global Accelerator pricing](http://aws.amazon.com/global-accelerator/pricing/)\.