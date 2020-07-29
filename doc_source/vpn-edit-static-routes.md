# Editing static routes for a Site\-to\-Site VPN connection<a name="vpn-edit-static-routes"></a>

For a Site\-to\-Site VPN connection on a virtual private gateway that's configured for static routing, you can add, modify, or remove the static routes for your VPN configuration\. 

**To add, modify, or remove a static route**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Choose **Static Routes**, **Edit**\. 

1. Modify your existing static IP prefixes, or choose **Remove** to delete them\. Choose **Add Another Rule** to add a new IP prefix to your configuration\. When you are done, choose **Save**\.

**Note**  
If you have not enabled route propagation for your route table, you must manually update the routes in your route table to reflect the updated static IP prefixes in your Site\-to\-Site VPN connection\. For more information, see [\(Virtual private gateway\) Enable route propagation in your route table](SetUpVPNConnections.md#vpn-configure-routing)\.

For a Site\-to\-Site VPN connection on a transit gateway, you add, modify, or remove the static routes in the transit gateway route table\. For more information, see [Transit gateway route tables](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-route-tables.html)\.

**To add a static route using the command line or API**
+ [CreateVpnConnectionRoute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpnConnectionRoute.html) \(Amazon EC2 Query API\)
+ [create\-vpn\-connection\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection-route.html) \(AWS CLI\)
+ [New\-EC2VpnConnectionRoute](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2VpnConnectionRoute.html) \(AWS Tools for Windows PowerShell\)

**To delete a static route using the command line or API**
+ [DeleteVpnConnectionRoute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DeleteVpnConnectionRoute.html) \(Amazon EC2 Query API\)
+ [delete\-vpn\-connection\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-vpn-connection-route.html) \(AWS CLI\)
+ [Remove\-EC2VpnConnectionRoute](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2VpnConnectionRoute.html) \(AWS Tools for Windows PowerShell\)