# Creating a transit gateway VPN attachment<a name="create-tgw-vpn-attachment"></a>

To create a VPN attachment on a transit gateway, you must specify the customer gateway\. For more information about creating a transit gateway, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.

**To create a VPN attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Choose **Create VPN Connection**\.

1. For **Target Gateway Type**, choose **Transit Gateway**, and choose the transit gateway on which to create the attachment\.

1. For **Customer Gateway**, do one of the following:
   + To use an existing customer gateway, choose **Existing**, and then select the gateway to use\.

     If your customer gateway is behind a network address translation \(NAT\) device that's enabled for NAT traversal \(NAT\-T\), use the public IP address of your NAT device, and adjust your firewall rules to unblock UDP port 4500\.
   + To create a customer gateway, choose **New**\. 

     For **IP Address**, enter a static public IP address\. For **BGP ASN**, enter the Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) of your customer gateway\. For **Certificate ARN**, choose the ARN of your private certificate \(if using certificate\-based authentication\)\.

     For **Routing options**, choose whether to use **Dynamic** or **Static**\.

1. \(Optional\) For **Enable Acceleration**, select the check box to enable acceleration\. For more information, see [Accelerated Site\-to\-Site VPN connections](accelerated-vpn.md)\.

   If you enable acceleration, we create two accelerators that are used by your VPN connection\. Additional charges apply\.

1. For **Tunnel Options**, see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\.

1. Choose **Create VPN Connection**\.

**To create a VPN attachment using the AWS CLI**  
Use the [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) command and specify the transit gateway ID for the `--transit-gateway-id` option\.