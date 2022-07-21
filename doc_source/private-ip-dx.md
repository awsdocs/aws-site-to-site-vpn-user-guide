# Private IP VPN with AWS Direct Connect<a name="private-ip-dx"></a>

With private IP VPN, you can deploy IPsec VPN over AWS Direct Connect, encrypting traffic between your on\-premises network and AWS, without the use of public IP addresses or additional third\-party VPN equipment\.

One of the main use cases for private IP VPN over AWS Direct Connect is helping customers in the financial, healthcare, and federal industries meet regulatory and compliance goals\. Private IP VPN over AWS Direct Connect ensures that traffic between AWS and on\-premises networks is both secure and private, allowing customers to comply with their regulatory and security mandates\.



**Topics**
+ [Benefits of private IP VPN](#private-ip-dx-features)
+ [How private IP VPN works](#private-ip-dx-how)
+ [Prerequisites](#private-ip-dx-prereq)
+ [Create the customer gateway](#private-ip-dx-cgw)
+ [Prepare the transit gateway](#private-ip-dx-tgw)
+ [Create the AWS Direct Connect gateway](#private-ip-dx-dxgw)
+ [Create the transit gateway association](#private-ip-dx-assoc)
+ [Create the VPN connection](#private-ip-dx-create)

## Benefits of private IP VPN<a name="private-ip-dx-features"></a>
+ **Simplified network management and operations:** Without private IP VPN, customers have to deploy third\-party VPN and routers to implement private VPNs over AWS Direct Connect networks\. With private IP VPN capability, customers don’t have to deploy and manage their own VPN infrastructure\. This leads to simplified network operations and reduced costs\.
+ **Improved security posture:** Previously, customers had to use a public AWS Direct Connect virtual interface \(VIF\) for encrypting traffic over AWS Direct Connect, which requires public IP addresses for VPN endpoints\. Using public IPs increases the probability of external \(DOS\) attacks, which in turn compels customers to deploy additional security gear for network protection\. Also, a public VIF opens access between all AWS public services and customer on\-premises networks, increasing the severity of the risk\. The private IP VPN feature allows encryption over AWS Direct Connect transit VIFs \(instead of public VIFs\), coupled with the ability to configure private IPs\. This provides end\-to\-end private connectivity in addition to encryption, improving the overall security posture\.
+ **Higher route scale:** Private IP VPN connections offer higher route limits \(5000 outbound routes and 1000 inbound routes\) as compared to AWS Direct Connect alone, which currently has a limit of 20 outbound and 100 inbound routes\.

## How private IP VPN works<a name="private-ip-dx-how"></a>

Private IP Site\-to\-Site VPN works over an AWS Direct Connect transit virtual interface \(VIF\)\. It uses an AWS Direct Connect gateway and a transit gateway to interconnect your on\-premises networks with AWS VPCs\. A private IP VPN connection has termination points at the transit gateway on the AWS side, and at your customer gateway device on the on\-premises side\. You can assign private IP addresses \(RFC1918\) to both the transit gateway and the customer gateway device ends of the IPsec tunnels\.

You attach a private IP VPN connection to a transit gateway\. You then route traffic between the VPN attachment and any VPCs \(or other networks\) that are also attached to the transit gateway\. You do that by associating a route table with the VPN attachment\. In the reverse direction, you can route traffic from your VPCs to the private IP VPN attachment by using route tables that are associated with the VPCs\.

The route table that's associated with the VPN attachment can be the same or different from the one associated with the underlying AWS Direct Connect attachment\. This gives you the ability to route both encrypted and unencrypted traffic simultaneously between your VPCs and your on\-premises networks\.

## Prerequisites<a name="private-ip-dx-prereq"></a>

The following resources are needed to complete the setup of a private IP VPN over AWS Direct Connect:
+ An AWS Direct Connect connection between your on\-premises network and AWS
+ An AWS Direct Connect gateway with an association with the appropriate transit gateway
+ A transit gateway with an available private IP CIDR block
+ A customer gateway device in your on\-premises network and a corresponding AWS customer gateway

## Create the customer gateway<a name="private-ip-dx-cgw"></a>

A *customer gateway* is a resource that you create in AWS\. It represents the customer gateway device in your on\-premises network\. When you create a customer gateway, you provide information about your device to AWS\. For more details, see [Customer gateway](how_it_works.md#CustomerGateway)\.

**To create a customer gateway using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Customer gateways**, and then **Create customer gateway**\.

1. Complete the following, and then choose **Create customer gateway**:
   + \(Optional\) For **Name tag**, enter a name for your customer gateway\. Doing so creates a tag with a key of `Name` and the value that you specify\.
   + For **BGP ASN**, enter a Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) for your customer gateway\. 
   + For **IP address**, enter the private IP address for your customer gateway device\. 
   + \(Optional\) For **Device**, enter a name for the device that hosts this customer gateway\.

**To create a customer gateway using the command line or API**
+ [CreateCustomerGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateCustomerGateway.html) \(Amazon EC2 Query API\)
+ [create\-customer\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-customer-gateway.html) \(AWS CLI\)

## Prepare the transit gateway<a name="private-ip-dx-tgw"></a>

A transit gateway is a network transit hub that you can use to interconnect your VPCs and on\-premises networks\. You can create a new transit gateway or use an existing one for the private IP VPN connection\. When you create the transit gateway, or modify an existing transit gateway, you specify a private IP CIDR block for the connection\.

**Note**  
When specifying the transit gateway CIDR block to be associated with your Private IP VPN, ensure the CIDR block does not overlap with any IP addresses for any other network attachments on the transit gateway\. If any IP CIDR blocks do overlap, it may cause configuration issues with your customer gateway device\.

For specific AWS console steps to create or modify a transit gateway to use for the private IP VPN, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in the *Amazon VPC Transit Gateways Guide*\.

**To create a transit gateway using the command line or API**
+ [CreateTransitGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateTransitGateway.html) \(Amazon EC2 Query API\)
+ [create\-transit\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway.html) \(AWS CLI\)

## Create the AWS Direct Connect gateway<a name="private-ip-dx-dxgw"></a>

Create an AWS Direct Connect gateway by following the [Creating a Direct Connect gateway](https://docs.aws.amazon.com/directconnect/latest/UserGuide/direct-connect-gateways-intro.html#create-direct-connect-gateway) procedure in the *AWS Direct Connect User Guide*\.

**To create an AWS Direct Connect gateway using the command line or API**
+ [CreateDirectConnectGateway](https://docs.aws.amazon.com/directconnect/latest/APIReference/API_CreateDirectConnectGateway.html) \(AWS Direct Connect Query API\)
+ [create\-direct\-connect\-gateway](https://docs.aws.amazon.com/cli/latest/reference/directconnect/create-direct-connect-gateway.html) \(AWS CLI\)

## Create the transit gateway association<a name="private-ip-dx-assoc"></a>

After creating the AWS Direct Connect gateway, create a transit gateway association for the AWS Direct Connect gateway\. Specify the private IP CIDR for the transit gateway that was identified earlier in the allowed prefixes list\.

For more information, see [Transit Gateway associations](https://docs.aws.amazon.com/directconnect/latest/UserGuide/direct-connect-transit-gateways.html) in the *AWS Direct Connect User Guide*\.

**To create an AWS Direct Connect gateway association using the command line or API**
+ [CreateDirectConnectGatewayAssociation](https://docs.aws.amazon.com/directconnect/latest/APIReference/API_CreateDirectConnectGatewayAssociation.html) \(AWS Direct Connect Query API\)
+ [create\-direct\-connect\-gateway\-association](https://docs.aws.amazon.com/cli/latest/reference/directconnect/create-direct-connect-gateway-association.html) \(AWS CLI\)

## Create the VPN connection<a name="private-ip-dx-create"></a>

**Create a Site\-to\-Site VPN connection using private IP addresses**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN connection**\.

1. \(Optional\) For **Name tag**, enter a name for your Site\-to\-Site VPN connection\. Doing so creates a tag with a key of `Name` and the value that you specify\.

1. For **Target gateway type**, choose **Transit gateway**\. Then, from the drop\-down list under **Transit gateway**, choose the transit gateway that you identified earlier\.

1. For **Customer gateway**, select **Existing**\. Then, from the drop\-down list under **Customer gateway ID**, choose the customer gateway that you created earlier\.

1. Select one of the routing options based on whether your customer gateway device supports Border Gateway Protocol \(BGP\):
   + If your customer gateway device supports BGP, choose **Dynamic \(requires BGP\)**\.
   + If your customer gateway device does not support BGP, choose **Static**\.

1. For **Tunnel Inside IP version**, specify whether the VPN tunnels support IPv4 or IPv6 traffic\.

1. \(Optional\) If you specified **IPv4** for **Tunnel Inside IP Version**, you can optionally specify the IPv4 CIDR ranges for the customer gateway and AWS sides that are allowed to communicate over the VPN tunnels\. The default is `0.0.0.0/0`\.

   If you specified **IPv6** for **Tunnel Inside IP Version**, you can optionally specify the IPv6 CIDR ranges for the customer gateway and AWS sides that are allowed to communicate over the VPN tunnels\. The default for both ranges is `::/0`\.

1. For **Outside IP address type**, choose `PrivateIpv4`\.

1. For **Transport attachment ID**, choose the transit gateway attachment for the appropriate AWS Direct Connect gateway\.

1. Choose **Create VPN connection**\.

**Note**  
The **Enable acceleration** option is not applicable for VPN connections over AWS Direct Connect\.