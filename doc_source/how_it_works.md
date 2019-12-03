# How AWS Site\-to\-Site VPN Works<a name="how_it_works"></a>

## Components of Your Site\-to\-Site VPN<a name="VPN"></a>

A Site\-to\-Site VPN connection offers two VPN tunnels between a virtual private gateway or a transit gateway on the AWS side and a customer gateway on the remote \(customer\) side\.

A Site\-to\-Site VPN connection consists of the following components\. For more information about Site\-to\-Site VPN limits, see [Site\-to\-Site VPN Limits](vpn-limits.md)\.

**Topics**
+ [Virtual Private Gateway](#VPNGateway)
+ [AWS Transit Gateway](#Transit-Gateway)
+ [Customer Gateway](#CustomerGateway)
+ [Customer Gateway Device](#CustomerGatewayDevice)

### Virtual Private Gateway<a name="VPNGateway"></a>

A *virtual private gateway* is the VPN concentrator on the Amazon side of the Site\-to\-Site VPN connection\. You create a virtual private gateway and attach it to the VPC from which you want to create the Site\-to\-Site VPN connection\.

When you create a virtual private gateway, you can specify the private Autonomous System Number \(ASN\) for the Amazon side of the gateway\. If you don't specify an ASN, the virtual private gateway is created with the default ASN \(64512\)\. You cannot change the ASN after you've created the virtual private gateway\. To check the ASN for your virtual private gateway, view its details in the **Virtual Private Gateways** screen in the Amazon VPC console, or use the [describe\-vpn\-gateways](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-gateways.html) AWS CLI command\.

**Note**  
If you create your virtual private gateway before 2018\-06\-30, the default ASN is 17493 in the Asia Pacific \(Singapore\) region, 10124 in the Asia Pacific \(Tokyo\) region, 9059 in the EU \(Ireland\) region, and 7224 in all other regions\. 

### AWS Transit Gateway<a name="Transit-Gateway"></a>

A transit gateway is a transit hub that you can use to interconnect your virtual private clouds \(VPC\) and on\-premises networks\. For more information, see [Amazon VPC Transit Gateways](https://docs.aws.amazon.com/vpc/latest/tgw/)\. You can create a Site\-to\-Site VPN connection as an attachment on a transit gateway\.

You can modify the target gateway of a Site\-to\-Site VPN connection from a virtual private gateway to a transit gateway\. For more information, see [Modifying a Site\-to\-Site VPN Connection's Target Gateway](modify-vpn-target.md)\.

### Customer Gateway<a name="CustomerGateway"></a>

A *customer gateway* is a resource in AWS that provides information to AWS about your [Customer Gateway Device](#CustomerGatewayDevice)\. For information about customer gateway options, see [Customer Gateway Options for Your Site\-to\-Site VPN Connection](cgw-options.md)\.

To use Amazon VPC with a Site\-to\-Site VPN connection, you or your network administrator must also configure the customer gateway device or application in your remote network\. When you create the Site\-to\-Site VPN connection, we provide you with the required configuration information and your network administrator typically performs this configuration\. For information about the customer gateway requirements and configuration, see the [Your Customer Gateway](https://docs.aws.amazon.com/vpc/latest/adminguide/Introduction.html) in the *AWS Site\-to\-Site VPN Network Administrator Guide*\.

The VPN tunnel comes up when traffic is generated from your side of the Site\-to\-Site VPN connection\. The virtual private gateway is not the initiator; your customer gateway must initiate the tunnels\. If your Site\-to\-Site VPN connection experiences a period of idle time \(usually 10 seconds, depending on your configuration\), the tunnel may go down\. To prevent this, you can use a network monitoring tool to generate keepalive pings; for example, by using IP SLA\. 

### Customer Gateway Device<a name="CustomerGatewayDevice"></a>

A *customer gateway device* is a physical device or software application on your side of the Site\-to\-Site VPN connection\. For more information about configuring your customer gateway device, see the *[AWS Site\-to\-Site VPN Network Administrator Guide](https://docs.aws.amazon.com/vpc/latest/adminguide/Welcome.html)*\.