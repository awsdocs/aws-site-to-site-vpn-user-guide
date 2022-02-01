# Getting started<a name="SetUpVPNConnections"></a>

Use the following procedure to set up an AWS Site\-to\-Site VPN connection\. During creation you will be asked to specify a virtual private gateway, a transit gateway or "Not Associated" for the target gateway type\. When a Site\-to\-Site VPN connection is created with "Not Associated" specified, you can choose the target gateway type at a later time or it can be used as a Site\-to\-Site VPN attachment for AWS Cloud WAN\. This procedure outlines creating a Site\-to\-Site VPN connection using a virtual private gateway and assumes that you have an existing VPC with one or more subnets\.

To set up a Site\-to\-Site VPN connection using a virtual private gateway, complete the following steps:
+ [Prerequisites](#vpn-prerequisites)
+ Step 1: [Create a customer gateway](#vpn-create-cgw)
+ Step 2: [Create a target gateway](#vpn-create-target-gateway)
+ Step 3: [Configure routing](#vpn-configure-route-tables)
+ Step 4: [Update your security group ](#vpn-configure-security-groups)
+ Step 5: [Create a Site\-to\-Site VPN connection](#vpn-create-vpn-connection)
+ Step 6: [Download the configuration file](#vpn-download-config)
+ Step 7: [Configure the customer gateway device](#vpn-configure-customer-gateway-device)

For steps to create a Site\-to\-Site VPN connection for use with an AWS Cloud WAN, see [Creating an AWS Cloud WAN Site\-to\-Site VPN attachment](create-cwan-vpn-attachment.md)\.

For steps to create a Site\-to\-Site VPN connection on a transit gateway, see [Creating a transit gateway VPN attachment](create-tgw-vpn-attachment.md)\.

## Prerequisites<a name="vpn-prerequisites"></a>

You need the following information to set up and configure the components of a Site\-to\-Site VPN connection\.


| Item | Information | 
| --- | --- | 
| Customer gateway device | The physical or software device on your side of the VPN connection\. You need the vendor \(for example, Cisco\), platform \(for example, ISR Series Routers\), and software version \(for example, IOS 12\.4\)\. | 
| Customer gateway | To create the customer gateway resource in AWS, you need the following information:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html)For more information, see [Customer gateway options for your Site\-to\-Site VPN connection](cgw-options.md)\. | 
|  \(Optional\) The ASN for the AWS side of the BGP session  |  You specify this when you create a virtual private gateway or transit gateway\. If you do not specify a value, the default ASN applies\. For more information, see [Virtual private gateway](how_it_works.md#VPNGateway)\.  | 
| VPN connection | To create the VPN connection, you need the following information:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html) | 

## Create a customer gateway<a name="vpn-create-cgw"></a>

A customer gateway provides information to AWS about your customer gateway device or software application\. For more information, see [Customer gateway](how_it_works.md#CustomerGateway)\.

If you plan to use a private certificate to authenticate your VPN, create a private certificate from a subordinate CA using AWS Certificate Manager Private Certificate Authority\. For information about creating a private certificate, see [Creating and managing a private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreatingManagingCA.html) in the *AWS Certificate Manager Private Certificate Authority User Guide*\.

**Note**  
You must specify either an IP address, or the Amazon Resource Name of the private certificate\.

**To create a customer gateway using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Customer Gateways**, and then **Create Customer Gateway**\.

1. Complete the following and then choose **Create Customer Gateway**:
   + \(Optional\) For **Name**, enter a name for your customer gateway\. Doing so creates a tag with a key of `Name` and the value that you specify\.
   + For **Routing**, select the routing type\.
   + For dynamic routing, for **BGP ASN**, enter the Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\)\.
   + \(Optional\) For **IP Address**, enter the static, internet\-routable IP address for your customer gateway device\. If your customer gateway is behind a NAT device that's enabled for NAT\-T, use the public IP address of the NAT device\.
   + \(Optional\) If you want to use a private certificate, for **Certificate ARN**, choose the Amazon Resource Name of the private certificate\.

**To create a customer gateway using the command line or API**
+ [CreateCustomerGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateCustomerGateway.html) \(Amazon EC2 Query API\)
+ [create\-customer\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-customer-gateway.html) \(AWS CLI\)
+ [New\-EC2CustomerGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2CustomerGateway.html) \(AWS Tools for Windows PowerShell\)

## Create a target gateway<a name="vpn-create-target-gateway"></a>

To establish a VPN connection between your VPC and your on\-premises network, you must create a target gateway on the AWS side of the connection\. The target gateway can be a virtual private gateway or a transit gateway\. 

### Create a virtual private gateway<a name="vpn-create-vpg"></a>

When you create a virtual private gateway, you can optionally specify the private Autonomous System Number \(ASN\) for the Amazon side of the gateway\. This ASN must be different from the BGP ASN that you specified for the customer gateway\.

After you create a virtual private gateway, you must attach it to your VPC\.

**To create a virtual private gateway and attach it to your VPC**

1. In the navigation pane, choose **Virtual Private Gateways**, **Create Virtual Private Gateway**\.

1. \(Optional\) Enter a name for your virtual private gateway\. Doing so creates a tag with a key of `Name` and the value that you specify\.

1. For **ASN**, keep the default selection to use the default Amazon ASN\. Otherwise, choose **Custom ASN** and enter a value\. For a 16\-bit ASN, the value must be in the 64512 to 65534 range\. For a 32\-bit ASN, the value must be in the 4200000000 to 4294967294 range\. 

1. Choose **Create Virtual Private Gateway**\.

1. Select the virtual private gateway that you created, and then choose **Actions**, **Attach to VPC**\.

1. Select your VPC from the list and choose **Yes, Attach**\.

**To create a virtual private gateway using the command line or API**
+ [CreateVpnGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpnGateway.html) \(Amazon EC2 Query API\)
+ [create\-vpn\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-gateway.html) \(AWS CLI\)
+ [New\-EC2VpnGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2VpnGateway.html) \(AWS Tools for Windows PowerShell\)

**To attach a virtual private gateway to a VPC using the command line or API**
+ [AttachVpnGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-AttachVpnGateway.html) \(Amazon EC2 Query API\)
+ [attach\-vpn\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/attach-vpn-gateway.html) \(AWS CLI\)
+ [Add\-EC2VpnGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/Add-EC2VpnGateway.html) \(AWS Tools for Windows PowerShell\)

### Create a transit gateway<a name="vpn-create-tgw"></a>

For more information about creating a transit gateway, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.

## Configure routing<a name="vpn-configure-route-tables"></a>

To enable instances in your VPC to reach your customer gateway, you must configure your route table to include the routes used by your Site\-to\-Site VPN connection and point them to your virtual private gateway or transit gateway\.

### \(Virtual private gateway\) Enable route propagation in your route table<a name="vpn-configure-routing"></a>

You can enable route propagation for your route table to automatically propagate Site\-to\-Site VPN routes\.

For static routing, the static IP prefixes that you specify for your VPN configuration are propagated to the route table when the status of the Site\-to\-Site VPN connection is `UP`\. Similarly, for dynamic routing, the BGP\-advertised routes from your customer gateway are propagated to the route table when the status of the Site\-to\-Site VPN connection is `UP`\.

**Note**  
If your connection is interrupted but the VPN connection remains UP, any propagated routes that are in your route table are not automatically removed\. Keep this in mind if, for example, you want traffic to fail over to a static route\. In that case, you might have to disable route propagation to remove the propagated routes\.

**To enable route propagation using the console**

1. In the navigation pane, choose **Route Tables**, and then select the route table that's associated with the subnet\. By default, this is the main route table for the VPC\.

1. On the **Route Propagation** tab in the details pane, choose **Edit route propagation**, select the virtual private gateway that you created in the previous procedure, and then choose **Save**\.

**Note**  
If you do not enable route propagation, you must manually enter the static routes used by your Site\-to\-Site VPN connection\. To do this, select your route table, choose **Routes**, **Edit**\. For **Destination**, add the static route used by your Site\-to\-Site VPN connection\. For **Target**, select the virtual private gateway ID, and choose **Save**\.

**To disable route propagation using the console**

1. In the navigation pane, choose **Route Tables**, and then select the route table that's associated with the subnet\.

1. Choose **Route Propagation**, **Edit route propagation**\. Clear the **Propagate** check box for the virtual private gateway, and choose **Save**\.

**To enable route propagation using the command line or API**
+ [EnableVgwRoutePropagation](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-EnableVgwRoutePropagation.html) \(Amazon EC2 Query API\)
+ [enable\-vgw\-route\-propagation](https://docs.aws.amazon.com/cli/latest/reference/ec2/enable-vgw-route-propagation.html) \(AWS CLI\)
+ [Enable\-EC2VgwRoutePropagation](https://docs.aws.amazon.com/powershell/latest/reference/items/Enable-EC2VgwRoutePropagation.html) \(AWS Tools for Windows PowerShell\)

**To disable route propagation using the command line or API**
+ [DisableVgwRoutePropagation](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DisableVgwRoutePropagation.html) \(Amazon EC2 Query API\)
+ [disable\-vgw\-route\-propagation](https://docs.aws.amazon.com/cli/latest/reference/ec2/disable-vgw-route-propagation.html) \(AWS CLI\)
+ [Disable\-EC2VgwRoutePropagation](https://docs.aws.amazon.com/powershell/latest/reference/items/Disable-EC2VgwRoutePropagation.html) \(AWS Tools for Windows PowerShell\)

### \(Transit gateway\) Add a route to your route table<a name="vpn-configure-tgw-routes"></a>

If you enabled route table propagation for your transit gateway, the routes for the VPN attachment are propagated to the transit gateway route table\. For more information, see [Routing](https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html#tgw-routing-overview) in *Amazon VPC Transit Gateways*\.

If you attach a VPC to your transit gateway and you want to enable resources in the VPC to reach your customer gateway, you must add a route to your subnet route table to point to the transit gateway\.

**To add a route to a VPC route table**

1. On the navigation pane, choose **Route Tables**\.

1. Choose the route table that is associated with your VPC\.

1. Choose the **Routes** tab, then choose **Edit routes**\.

1. Choose **Add route**\.

1. In the **Destination** column, enter the destination IP address range\. For **Target**, choose the transit gateway\.

1. Choose **Save routes**, and then choose **Close**\.

## Update your security group<a name="vpn-configure-security-groups"></a>

To allow access to instances in your VPC from your network, you must update your security group rules to enable inbound SSH, RDP, and ICMP access\.

**To add rules to your security group to enable inbound SSH, RDP and ICMP access**

1. In the navigation pane, choose **Security Groups**, and then select the default security group for the VPC\.

1. On the **Inbound** tab in the details pane, add rules that allow inbound SSH, RDP, and ICMP access from your network, and then choose **Save**\. For more information about adding inbound rules, see [Adding, removing, and updating rules](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#AddRemoveRules) in the *Amazon VPC User Guide\.*

For more information about working with security groups using the AWS CLI, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

## Create a Site\-to\-Site VPN connection<a name="vpn-create-vpn-connection"></a>

Create the Site\-to\-Site VPN connection using the customer gateway and the virtual private gateway or transit gateway that you created earlier\.

**To create a Site\-to\-Site VPN connection**

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN Connection**\.

1. \(Optional\) For **Name tag**, enter a name for your Site\-to\-Site VPN connection\. Doing so creates a tag with a key of `Name` and the value that you specify\.

1. For **Target Gateway Type**, choose either **Virtual Private Gateway** or **Transit Gateway**\. Then, choose the virtual private gateway or transit gateway that you created earlier\.

1. For **Customer Gateway ID**, select the customer gateway that you created earlier\.

1. Select one of the routing options based on whether your customer gateway device supports Border Gateway Protocol \(BGP\):
   + If your customer gateway device supports BGP, choose **Dynamic \(requires BGP\)**\.
   + If your customer gateway device does not support BGP, choose **Static**\. For **Static IP Prefixes**, specify each IP prefix for the private network of your Site\-to\-Site VPN connection\.

1. \(Optional\) For **Tunnel Inside IP Version**, specify whether the VPN tunnels support IPv4 or IPv6 traffic\. IPv6 traffic is only supported for VPN connections on a transit gateway\.

1. \(Optional\) For **Local IPv4 Network CIDR**, specify the IPv4 CIDR range on the customer gateway \(on\-premises\) side that is allowed to communicate over the VPN tunnels\. The default is `0.0.0.0/0`\.

   For **Remote IPv4 Network CIDR**, specify the IPv4 CIDR range on the AWS side that is allowed to communicate over the VPN tunnels\. The default is `0.0.0.0/0`\.

   If you specified **IPv6** for **Tunnel Inside IP Version**, then specify the IPv6 CIDR ranges on the customer gateway side and AWS side that are allowed to communicate over the VPN tunnels\. The default for both ranges is `::/0`\.

1. \(Optional\) For **Tunnel Options**, you can specify the following information for each tunnel:
   + A size /30 IPv4 CIDR block from the `169.254.0.0/16` range for the inside tunnel IPv4 addresses\.
   + If you specified **IPv6** for **Tunnel Inside IP Version**, a /126 IPv6 CIDR block from the `fd00::/8` range for the inside tunnel IPv6 addresses\.
   + The IKE pre\-shared key \(PSK\)\. The following versions are supported: IKEv1 or IKEv2\.
   + Advanced tunnel information, which includes the following:
     + Encryption algorithms for phases 1 and 2 of the IKE negotiations
     + Integrity algorithms for phases 1 and 2 of the IKE negotiations
     + Diffie\-Hellman groups for phases 1 and 2 of the IKE negotiations
     + IKE version
     + Phase 1 and 2 lifetimes
     + Rekey margin time
     + Rekey fuzz
     + Replay window size
     + Dead peer detection interval
     + Dead peer detection timeout action
     + Startup action

   For more information about these options, see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\.

1. Choose **Create VPN Connection**\. It might take a few minutes to create the Site\-to\-Site VPN connection\.

**To create a Site\-to\-Site VPN connection using the command line or API**
+ [CreateVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpnConnection.html) \(Amazon EC2 Query API\)
+ [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) \(AWS CLI\)
+ [New\-EC2VpnConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2VpnConnection.html) \(AWS Tools for Windows PowerShell\)

## Download the configuration file<a name="vpn-download-config"></a>

After you create the Site\-to\-Site VPN connection, you can download a sample configuration file to use for configuring the customer gateway device\.

**Important**  
The configuration file is an example only and might not match your intended Site\-to\-Site VPN connection settings entirely\. It specifies the minimum requirements for a Site\-to\-Site VPN connection of AES128, SHA1, and Diffie\-Hellman group 2 in most AWS Regions, and AES128, SHA2, and Diffie\-Hellman group 14 in the AWS GovCloud Regions\. It also specifies pre\-shared keys for authentication\. You must modify the example configuration file to take advantage of additional security algorithms, Diffie\-Hellman groups, private certificates, and IPv6 traffic\.   
We have introduced IKEv2 support in the configuration files for many popular customer gateway devices and will continue to add additional files over time\. This list will be updated as more example configuration files are added\. See [Your customer gateway device](your-cgw.md) for a complete list of configuration files with IKEv2 support\.

**To download the configuration file from the AWS Management Console**
**Note**  
To properly load the Download Configuration screen from the AWS Management Console please ensure your IAM role or user has permission for the following two Amazon EC2 APIs: GetVpnConnectionDeviceTypes and GetVpnConnectionDeviceSampleConfiguration\.

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select your VPN connection and choose **Download Configuration**\.

1. Select the `vendor`, `platform`, `software` and `IKE version` that correspond to your customer gateway device\. If your device is not listed, choose `Generic`\.

1. Choose **Download**\.

**To download a sample configuration file using the AWS command line or API**
+ [GetVpnConnectionDeviceTypes](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-GetVpnConnectionDeviceTypes.html) \(Amazon EC2 Query API\)
+ [GetVpnConnectionDeviceSampleConfiguration](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-GetVpnConnectionDeviceSampleConfiguration.html) \(Amazon EC2 Query API\)
+ [get\-vpn\-connection\-device\-types](https://docs.aws.amazon.com/cli/latest/reference/ec2/get-vpn-connection-device-types.html) \(AWS CLI\)
+ [get\-vpn\-connection\-device\-sample\-configuration](https://docs.aws.amazon.com/cli/latest/reference/ec2/get-vpn-connection-device-sample-configuration.html) \(AWS CLI\)

## Configure the customer gateway device<a name="vpn-configure-customer-gateway-device"></a>

Use the sample configuration file to configure your customer gateway device\. The customer gateway device is the physical or software appliance on your side of the Site\-to\-Site VPN connection\. For more information, see [Your customer gateway device](your-cgw.md)\.