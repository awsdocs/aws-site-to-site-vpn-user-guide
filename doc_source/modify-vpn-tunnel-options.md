# Modifying Site\-to\-Site VPN tunnel options<a name="modify-vpn-tunnel-options"></a>

You can modify the tunnel options for the VPN tunnels in your Site\-to\-Site VPN connection\. You can modify one VPN tunnel at a time\.

**Important**  
When you modify a VPN tunnel, connectivity over the tunnel is interrupted for up to several minutes\. Ensure that you plan for the expected downtime\.

**To modify the VPN tunnel options using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection, and choose **Actions**, **Modify VPN Tunnel Options**\.

1. For **VPN Tunnel Outside IP Address**, choose the tunnel endpoint IP of the VPN tunnel that you're modifying options for\.

1. Choose or enter new values for the tunnel options\. For more information, see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\.

1. Choose **Save**\.

**To modify the VPN tunnel options using the command line or API**
+  \(AWS CLI\) Use [describe\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-connections.html) to view the current tunnel options, and [modify\-vpn\-tunnel\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel-options.html) to modify the tunnel options\.
+ \(Amazon EC2 Query API\) Use [DescribeVpnConnections](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeVpnConnections.html) to view the current tunnel options, and [ModifyVpnTunnelOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnTunnelOptions.html) to modify the tunnel options\.