# Changing the customer gateway for a Site\-to\-Site VPN connection<a name="change-vpn-cgw"></a>

You can change the customer gateway of your Site\-to\-Site VPN connection by using the Amazon VPC console or a command line tool\. 

After you change the customer gateway, your Site\-to\-Site VPN connection will be temporarily unavailable for a brief period while we provision the new endpoints\.

**To change the customer gateway using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection, and then choose **Actions**, **Modify VPN Connection**\.

1. For **Target Type**, choose **Customer Gateway**\.

1. For **Target Customer Gateway ID**, choose the ID for the customer gateway that you want to use for the connection\.

**To change the customer gateway using the command line or API**
+ [ModifyVpnTunnelC](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiR_ModifyVpnTunnelC.html) \(Amazon EC2 Query API\)
+ [modify\-vpn\-tunnel](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel.html) \(AWS CLI\)