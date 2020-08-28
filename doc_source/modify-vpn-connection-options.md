# Modifying Site\-to\-Site VPN connection options<a name="modify-vpn-connection-options"></a>

You can modify the connection options for your Site\-to\-Site VPN connection\. You can modify the following options:
+ The IPv4 CIDR ranges on the local \(customer gateway\) side and the remote \(AWS\) side of the VPN connection that can communicate over the VPN tunnels\. The default is `0.0.0.0/0` for both ranges\.
+ The IPv6 CIDR ranges on the local \(customer gateway\) and the remote \(AWS\) side of the VPN connection that can communicate over the VPN tunnels\. The default is `::/0` for both ranges\.

When you modify the VPN connection options, the VPN endpoint IP addresses on the AWS side do not change, and the tunnel options do not change\. Your VPN connection will be temporarily unavailable for a brief period while the VPN connection is updated\.

**To modify the VPN connection options using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select your VPN connection, and choose **Actions**, **Modify VPN Connection Options**\.

1. Enter new values for the options that you want to modify\.

1. Choose **Save**\.

**To modify the VPN connection options using the command line or API**
+ [modify\-vpn\-connection\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-connection-options.html) \(AWS CLI\)
+ [ModifyVpnConnectionOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnConnectionOptions.html) \(Amazon EC2 Query API\)