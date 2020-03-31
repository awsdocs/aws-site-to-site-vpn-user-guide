# How AWS Site\-to\-Site VPN works<a name="how_it_works"></a>

## Components of your Site\-to\-Site VPN<a name="VPN"></a>

A Site\-to\-Site VPN connection offers two VPN tunnels between a virtual private gateway or a transit gateway on the AWS side, and a customer gateway on the remote \(on\-premises\) side\.

A Site\-to\-Site VPN connection consists of the following components\. For more information about Site\-to\-Site VPN quotas, see [Site\-to\-Site VPN quotas](vpn-limits.md)\.

**Topics**
+ [Virtual private gateway](#VPNGateway)
+ [Transit gateway](#Transit-Gateway)
+ [Customer gateway](#CustomerGateway)
+ [Customer gateway device](#CustomerGatewayDevice)

### Virtual private gateway<a name="VPNGateway"></a>

A *virtual private gateway* is the VPN concentrator on the Amazon side of the Site\-to\-Site VPN connection\. You create a virtual private gateway and attach it to the VPC from which you want to create the Site\-to\-Site VPN connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/vpn-how-it-works-vgw.png)

When you create a virtual private gateway, you can specify the private Autonomous System Number \(ASN\) for the Amazon side of the gateway\. If you don't specify an ASN, the virtual private gateway is created with the default ASN \(64512\)\. You cannot change the ASN after you've created the virtual private gateway\. To check the ASN for your virtual private gateway, view its details in the **Virtual Private Gateways** screen in the Amazon VPC console, or use the [describe\-vpn\-gateways](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-gateways.html) AWS CLI command\.

**Note**  
If you create your virtual private gateway before 2018\-06\-30, the default ASN is 17493 in the Asia Pacific \(Singapore\) region, 10124 in the Asia Pacific \(Tokyo\) region, 9059 in the Europe \(Ireland\) region, and 7224 in all other regions\. 

### Transit gateway<a name="Transit-Gateway"></a>

A transit gateway is a transit hub that you can use to interconnect your virtual private clouds \(VPC\) and on\-premises networks\. For more information, see [Amazon VPC Transit Gateways](https://docs.aws.amazon.com/vpc/latest/tgw/)\. You can create a Site\-to\-Site VPN connection as an attachment on a transit gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/vpn-how-it-works-tgw.png)

You can modify the target gateway of a Site\-to\-Site VPN connection from a virtual private gateway to a transit gateway\. For more information, see [Modifying a Site\-to\-Site VPN connection's target gateway](modify-vpn-target.md)\.

### Customer gateway<a name="CustomerGateway"></a>

A *customer gateway* is a resource in AWS that provides information to AWS about your [Customer gateway device](#CustomerGatewayDevice)\. For information about customer gateway options, see [Customer gateway options for your Site\-to\-Site VPN connection](cgw-options.md)\.

To use Amazon VPC with a Site\-to\-Site VPN connection, you or your network administrator must also configure the customer gateway device or application in your remote network\. When you create the Site\-to\-Site VPN connection, we provide you with the required configuration information and your network administrator typically performs this configuration\. For information about the customer gateway requirements and configuration, see [Your customer gateway device](your-cgw.md)\.

The VPN tunnel comes up when traffic is generated from your side of the Site\-to\-Site VPN connection\. The virtual private gateway is not the initiator; your customer gateway must initiate the tunnels\. If your Site\-to\-Site VPN connection experiences a period of idle time \(usually 10 seconds, depending on your configuration\), the tunnel may go down\. To prevent this, you can use a network monitoring tool to generate keepalive pings; for example, by using IP SLA\. 

VPN endpoints support rekey and can start renegotiations when phase 1 is about to expire if the customer gateway device hasn't sent any renegotiation traffic\.

### Customer gateway device<a name="CustomerGatewayDevice"></a>

A *customer gateway device* is a physical device or software application on your side of the Site\-to\-Site VPN connection\. For more information about configuring your customer gateway device, see [Your customer gateway device](your-cgw.md)\.