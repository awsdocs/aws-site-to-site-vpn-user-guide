# What is AWS Site\-to\-Site VPN?<a name="VPC_VPN"></a>

By default, instances that you launch into an Amazon VPC can't communicate with your own \(remote\) network\. You can enable access to your remote network from your VPC by attaching a virtual private gateway to the VPC, creating a custom route table, updating your security group rules, and creating an AWS Site\-to\-Site VPN \(Site\-to\-Site VPN\) connection\.

Although the term *VPN connection* is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on\-premises network\. Site\-to\-Site VPN supports Internet Protocol security \(IPsec\) VPN connections\.

Your Site\-to\-Site VPN connection is either an AWS Classic VPN or an AWS VPN\. For more information, see [AWS Site\-to\-Site VPN Categories](#vpn-categories)\.

**Important**  
We currently do not support IPv6 traffic through a Site\-to\-Site VPN connection\.

**Topics**
+ [Components of Your Site\-to\-Site VPN](#VPN)
+ [AWS Site\-to\-Site VPN Categories](#vpn-categories)
+ [Site\-to\-Site VPN Configuration Examples](Examples.md)
+ [Site\-to\-Site VPN Routing Options](VPNRoutingTypes.md)
+ [Configuring the VPN Tunnels for Your Site\-to\-Site VPN Connection](VPNTunnels.md)
+ [Using Redundant Site\-to\-Site VPN Connections to Provide Failover](VPNConnections.md)

## Components of Your Site\-to\-Site VPN<a name="VPN"></a>

A Site\-to\-Site VPN connection consists of the following components\. For more information about Site\-to\-Site VPN limits, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.

### Virtual Private Gateway<a name="VPNGateway"></a>

A *virtual private gateway* is the VPN concentrator on the Amazon side of the Site\-to\-Site VPN connection\. You create a virtual private gateway and attach it to the VPC from which you want to create the Site\-to\-Site VPN connection\.

When you create a virtual private gateway, you can specify the private Autonomous System Number \(ASN\) for the Amazon side of the gateway\. If you don't specify an ASN, the virtual private gateway is created with the default ASN \(64512\)\. You cannot change the ASN after you've created the virtual private gateway\. To check the ASN for your virtual private gateway, view its details in the **Virtual Private Gateways** screen in the Amazon VPC console, or use the [describe\-vpn\-gateways](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-gateways.html) AWS CLI command\.

**Note**  
If you create your virtual private gateway before 2018\-06\-30, the default ASN is 17493 in the Asia Pacific \(Singapore\) region, 10124 in the Asia Pacific \(Tokyo\) region, 9059 in the EU \(Ireland\) region, and 7224 in all other regions\. 

### Customer Gateway<a name="CustomerGateway"></a>

A *customer gateway* is a physical device or software application on your side of the Site\-to\-Site VPN connection\.

To create a Site\-to\-Site VPN connection, you must create a customer gateway resource in AWS, which provides information to AWS about your customer gateway device\. The following table describes the information you'll need to create a customer gateway resource\.


| Item | Description | 
| --- | --- | 
|  Internet\-routable IP address \(static\) of the customer gateway's external interface\.   |  The public IP address value must be static\. If your customer gateway is behind a network address translation \(NAT\) device that's enabled for NAT traversal \(NAT\-T\), use the public IP address of your NAT device, and adjust your firewall rules to unblock UDP port 4500\.  | 
|  The type of routing—static or dynamic\.   | For more information, see [Site\-to\-Site VPN Routing Options](VPNRoutingTypes.md)\. | 
|  \(Dynamic routing only\) Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) of the customer gateway\.  |  You can use an existing ASN assigned to your network\. If you don't have one, you can use a private ASN \(in the 64512–65534 range\)\.  If you use the VPC wizard in the console to set up your VPC, we automatically use 65000 as the ASN\.  | 

To use Amazon VPC with a Site\-to\-Site VPN connection, you or your network administrator must also configure the customer gateway device or application in your remote network\. When you create the Site\-to\-Site VPN connection, we provide you with the required configuration information and your network administrator typically performs this configuration\. For information about the customer gateway requirements and configuration, see the [Your Customer Gateway](https://docs.aws.amazon.com/vpc/latest/adminguide/Introduction.html) in the *Amazon VPC Network Administrator Guide*\.

The VPN tunnel comes up when traffic is generated from your side of the Site\-to\-Site VPN connection\. The virtual private gateway is not the initiator; your customer gateway must initiate the tunnels\. If your Site\-to\-Site VPN connection experiences a period of idle time \(usually 10 seconds, depending on your configuration\), the tunnel may go down\. To prevent this, you can use a network monitoring tool to generate keepalive pings; for example, by using IP SLA\. 

For a list of customer gateways that we have tested with Amazon VPC, see [Amazon Virtual Private Cloud FAQs](http://aws.amazon.com/vpn/faqs/#C9)\.

## AWS Site\-to\-Site VPN Categories<a name="vpn-categories"></a>

Your Site\-to\-Site VPN connection is either an AWS Classic VPN connection or an AWS VPN connection\. Any new Site\-to\-Site VPN connection that you create is an AWS VPN connection\. The following features are supported on AWS VPN connections only:
+ Internet Key Exchange version 2 \(IKEv2\)
+ NAT traversal
+ 4\-byte ASN \(in addition to 2\-byte ASN\)
+ CloudWatch metrics
+ Reusable IP addresses for your customer gateways
+ Additional encryption options; including AES 256\-bit encryption, SHA\-2 hashing, and additional Diffie\-Hellman groups
+ Configurable tunnel options
+ Custom private ASN for the Amazon side of a BGP session

You can find out the category of your Site\-to\-Site VPN connection by using the Amazon VPC console or a command line tool\. 

**To identify the Site\-to\-Site VPN category using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection, and check the value for **Category** in the details pane\. A value of `VPN` indicates an AWS VPN connection\. A value of `VPN-Classic` indicates an AWS Classic VPN connection\.

**To identify the Site\-to\-Site VPN category using a command line tool**
+ You can use the [describe\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-connections.html) AWS CLI command\. In the output that's returned, take note of the `Category` value\. A value of `VPN` indicates an AWS VPN connection\. A value of `VPN-Classic` indicates an AWS Classic VPN connection\.

  In the following example, the Site\-to\-Site VPN connection is an AWS VPN connection\.

  ```
  aws ec2 describe-vpn-connections --vpn-connection-ids vpn-1a2b3c4d
  ```

  ```
  {
      "VpnConnections": [
          {
              "VpnConnectionId": "vpn-1a2b3c4d", 
  
              ...
  
              "State": "available", 
              "VpnGatewayId": "vgw-11aa22bb", 
              "CustomerGatewayId": "cgw-ab12cd34", 
              "Type": "ipsec.1",
              "Category": "VPN"
          }
      ]
  }
  ```

Alternatively, use one of the following commands:
+ [DescribeVpnConnections](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeVpnConnections.html) \(Amazon EC2 Query API\)
+ [Get\-EC2VpnConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2VpnConnection.html) \(Tools for Windows PowerShell\)

### Migrating to AWS VPN<a name="aws-vpn-migrate"></a>

If your existing Site\-to\-Site VPN connection is an AWS Classic VPN connection, you can migrate to an AWS VPN connection by creating a new virtual private gateway and Site\-to\-Site VPN connection, detaching the old virtual private gateway from your VPC, and attaching the new virtual private gateway to your VPC\.

If your existing virtual private gateway is associated with multiple Site\-to\-Site VPN connections, you must recreate each Site\-to\-Site VPN connection for the new virtual private gateway\. If there are multiple AWS Direct Connect private virtual interfaces attached to your virtual private gateway, you must recreate each private virtual interface for the new virtual private gateway\. For more information, see [Creating a Virtual Interface](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-vif.html) in the *AWS Direct Connect User Guide*\.

If your existing Site\-to\-Site VPN connection is an AWS VPN connection, you cannot migrate to an AWS Classic VPN connection\.

**Note**  
During this procedure, connectivity over the current VPC connection is interrupted when you disable route propagation and detach the old virtual private gateway from your VPC\. Connectivity is restored when the new virtual private gateway is attached to your VPC and the new Site\-to\-Site VPN connection is active\. Ensure that you plan for the expected downtime\.

**To migrate to an AWS VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Virtual Private Gateways**, **Create Virtual Private Gateway** and create a virtual private gateway\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN Connection**\. Specify the following information, and choose **Yes, Create**\.
   + **Virtual Private Gateway**: Select the virtual private gateway that you created in the previous step\.
   + **Customer Gateway**: Choose **Existing**, and select the existing customer gateway for your current AWS Classic VPN connection\.
   + Specify the routing options as required\.

1. Select the new Site\-to\-Site VPN connection and choose **Download Configuration**\. Download the appropriate configuration file for your customer gateway device\.

1. Use the configuration file to configure VPN tunnels on your customer gateway device\. For examples, see the *[Amazon VPC Network Administrator Guide](https://docs.aws.amazon.com/vpc/latest/adminguide/)*\. Do not enable the tunnels yet\. Contact your vendor if you need guidance on keeping the newly configured tunnels disabled\. 

1. \(Optional\) Create test VPC and attach the virtual private gateway to the test VPC\. Change the encryption domain/source destination addresses as required, and test connectivity from a host in your local network to a test instance in the test VPC\.

1. If you are using route propagation for your route table, choose **Route Tables** in the navigation pane\. Select the route table for your VPC, and choose **Route Propagation**, **Edit**\. Clear the check box for the old virtual private gateway and choose **Save**\.
**Note**  
From this step onwards, connectivity is interrupted until the new virtual private gateway is attached and the new Site\-to\-Site VPN connection is active\.

1. In the navigation pane, choose **Virtual Private Gateways**\. Select the old virtual private gateway and choose **Actions**, **Detach from VPC**, **Yes, Detach**\. Select the new virtual private gateway, and choose **Actions**, **Attach to VPC**\. Specify the VPC for your Site\-to\-Site VPN connection, and choose **Yes, Attach**\. 

1. In the navigation pane, choose **Route Tables**\. Select the route table for your VPC and do one of the following: 
   + If you are using route propagation, choose **Route Propagation**, **Edit**\. Select the new virtual private gateway that's attached to the VPC and choose **Save**\.
   + If you are using static routes, choose **Routes**, **Edit**\. Modify the route to point to the new virtual private gateway, and choose **Save**\.

1. Enable the new tunnels on your customer gateway device and disable the old tunnels\. To bring the tunnel up, you must initiate the connection from your local network\.

   If applicable, check your route table to ensure that the routes are being propagated\. The routes propagate to the route table when the status of the VPN tunnel is `UP`\. 
**Note**  
If you need to revert to your previous configuration, detach the new virtual private gateway and follow steps 8 and 9 to re\-attach the old virtual private gateway and update your routes\.

1. If you no longer need your AWS Classic VPN connection and do not want to continue incurring charges for it, remove the previous tunnel configurations from your customer gateway device, and delete the Site\-to\-Site VPN connection\. To do this, go to **Site\-to\-Site VPN Connections**, select the Site\-to\-Site VPN connection, and choose **Delete**\.
**Important**  
After you've deleted the AWS Classic VPN connection, you cannot revert or migrate your new AWS VPN connection back to an AWS Classic VPN connection\.