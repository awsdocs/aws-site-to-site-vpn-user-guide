# Creating an AWS Cloud WAN Site\-to\-Site VPN attachment<a name="create-cwan-vpn-attachment"></a>

Follow the procedure below to create a Site\-to\-Site VPN attachment for AWS Cloud WAN\.

**Creating an AWS Cloud WAN Site\-to\-Site VPN attachment**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Choose **Create VPN Connection**\.

1. \(Optional\) Enter a name for the connection in the **Name tag** text box\.

1. For **Target Gateway Type**, choose **Not Associated**\.

1. For **Customer Gateway**, do one of the following:
   + To use an existing customer gateway, choose **Existing**, and then select the customer gateway ID to use\.
   + To create a customer gateway, choose **New**\.

     For **IP Address**, enter a static public IP address\. For **BGP ASN**, enter the Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) of your customer gateway\. Optionally, for **Certificate ARN**, choose the ARN of your private certificate \(if using certificate\-based authentication\)\. See [Customer gateway options for your Site\-to\-Site VPN connection](cgw-options.md) for more information\. 

1. For **Routing options**, choose whether to use **Dynamic** or **Static**\.

1. For **Tunnel Inside IP Version**, choose whether to use **IPv4** or **IPv6**\.

1. \(Optional\) For **Enable Acceleration**, select the check box to enable acceleration\. For more information, see [Accelerated Site\-to\-Site VPN connections](accelerated-vpn.md)\.

   If you enable acceleration, we create two accelerators that are used by your VPN connection\. Additional charges apply\.

1. Optionally configure local and remote network CIDR

1. For **Tunnel Options**, see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\.

1. Choose **Create VPN Connection**\.

**To create a Site\-to\-Site VPN connection using the command line or API**
+ [CreateVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpnConnection.html) \(Amazon EC2 Query API\)
+ [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) \(AWS CLI\)