# Creating a transit gateway VPN attachment<a name="create-tgw-vpn-attachment"></a>

To create a VPN attachment on a transit gateway, you must specify the transit gateway and the customer gateway\. The transit gateway will need to be created before following this procedure\. For more information about creating a transit gateway, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html) in *Amazon VPC Transit Gateways*\.

**To create a VPN attachment on a transit gateway using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Choose **Create VPN connection**\.

1. \(Optional\) For **Name tag**, enter a name for your Site\-to\-Site VPN connection\. Doing so creates a tag with a key of `Name` and the value that you specify\.

1. For **Target gateway type**, choose **Transit gateway**, and choose the transit gateway on which to create the attachment\.

1. For **Customer gateway**, do one of the following:
   + To use an existing customer gateway, choose **Existing**, and then select the gateway to use\.

     If your customer gateway is behind a network address translation \(NAT\) device that's enabled for NAT traversal \(NAT\-T\), use the public IP address of your NAT device, and adjust your firewall rules to unblock UDP port 4500\.
   + To create a customer gateway, choose **New**\.

     For **IP Address**, enter a static public IP address\. For **Certificate ARN**, choose the ARN of your private certificate \(if using certificate\-based authentication\)\. For **BGP ASN**, enter the Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) of your customer gateway\. See [Customer gateway options for your Site\-to\-Site VPN connection](cgw-options.md) for more information\. 

1. For **Routing options**, choose whether to use **Dynamic** or **Static**\.

1. For **Tunnel Inside IP Version**, specify whether the VPN tunnels support IPv4 or IPv6 traffic\. IPv6 traffic is only supported for VPN connections on a transit gateway\.

1. \(Optional\) For **Enable acceleration**, select the check box to enable acceleration\. For more information, see [Accelerated Site\-to\-Site VPN connections](accelerated-vpn.md)\.

   If you enable acceleration, we create two accelerators that are used by your VPN connection\. Additional charges apply\.

1. \(Optional\) For **Local IPv4 network CIDR**, specify the IPv4 CIDR range on the customer gateway \(on\-premises\) side that is allowed to communicate over the VPN tunnels\. The default is `0.0.0.0/0`\.

   For **Remote IPv4 network CIDR**, specify the IPv4 CIDR range on the AWS side that is allowed to communicate over the VPN tunnels\. The default is `0.0.0.0/0`\.

   If you specified **IPv6** for **Tunnel inside IP version**, then specify the IPv6 CIDR ranges on the customer gateway side and AWS side that are allowed to communicate over the VPN tunnels\. The default for both ranges is `::/0`\.

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

1. Choose **Create VPN Connection**\.

**To create a VPN attachment using the AWS CLI**  
Use the [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) command and specify the transit gateway ID for the `--transit-gateway-id` option\.