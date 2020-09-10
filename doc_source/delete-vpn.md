# Deleting a Site\-to\-Site VPN connection<a name="delete-vpn"></a>

If you no longer need an AWS Site\-to\-Site VPN connection, you can delete it\. When you delete a Site\-to\-Site VPN connection, we do not delete the customer gateway or virtual private gateway that was associated with the Site\-to\-Site VPN connection\. If you no longer need the customer gateway and virtual private gateway, you can delete them\.

**Important**  
If you delete your Site\-to\-Site VPN connection and then create a new one, you have to download a new configuration file and reconfigure the customer gateway device\.

**Topics**
+ [Deleting a Site\-to\-Site VPN connection](#delete-vpn-connection)
+ [Deleting a customer gateway](#delete-cgw)
+ [Detaching and deleting a virtual private gateway](#delete-vgw)

## Deleting a Site\-to\-Site VPN connection<a name="delete-vpn-connection"></a>

After you delete your Site\-to\-Site VPN connection, it remains visible for a short while with a state of `deleted`, and then the entry is automatically removed\.

**To delete a Site\-to\-Site VPN connection using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection and choose **Actions**, **Delete**\.

1. Choose **Delete**\.

**To delete a Site\-to\-Site VPN connection using the command line or API**
+ [DeleteVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DeleteVpnConnection.html) \(Amazon EC2 Query API\)
+ [delete\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-vpn-connection.html) \(AWS CLI\)
+ [Remove\-EC2VpnConnection](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2VpnConnection.html) \(AWS Tools for Windows PowerShell\)

## Deleting a customer gateway<a name="delete-cgw"></a>

If you no longer need a customer gateway, you can delete it\. You can't delete a customer gateway that's being used in a Site\-to\-Site VPN connection\.

**To delete a customer gateway using the console**

1. In the navigation pane, choose **Customer Gateways**\.

1. Select the customer gateway to delete and choose **Actions**, **Delete Customer Gateway**\.

1. Choose **Yes, Delete**\.

**To delete a customer gateway using the command line or API**
+ [DeleteCustomerGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DeleteCustomerGateway.html) \(Amazon EC2 Query API\)
+ [delete\-customer\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-customer-gateway.html) \(AWS CLI\)
+ [Remove\-EC2CustomerGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2CustomerGateway.html) \(AWS Tools for Windows PowerShell\)

## Detaching and deleting a virtual private gateway<a name="delete-vgw"></a>

If you no longer require a virtual private gateway for your VPC, you can detach it from the VPC\.

**To detach a virtual private gateway using the console**

1. In the navigation pane, choose **Virtual Private Gateways**\.

1. Select the virtual private gateway and choose **Actions**, **Detach from VPC**\.

1. Choose **Yes, Detach**\.

If you no longer require a detached virtual private gateway, you can delete it\. You can't delete a virtual private gateway that's still attached to a VPC\.

**To delete a virtual private gateway using the console**

1. In the navigation pane, choose **Virtual Private Gateways**\.

1. Select the virtual private gateway to delete and choose **Actions**, **Delete Virtual Private Gateway**\.

1. Choose **Yes, Delete**\.

**To detach a virtual private gateway using the command line or API**
+ [DetachVpnGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DetachVpnGateway.html) \(Amazon EC2 Query API\)
+ [detach\-vpn\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/detach-vpn-gateway.html) \(AWS CLI\)
+ [Dismount\-EC2VpnGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/Dismount-EC2VpnGateway.html) \(AWS Tools for Windows PowerShell\)

**To delete a virtual private gateway using the command line or API**
+ [DeleteVpnGateway](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DeleteVpnGateway.html) \(Amazon EC2 Query API\)
+ [delete\-vpn\-gateway](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-vpn-gateway.html) \(AWS CLI\)
+ [Remove\-EC2VpnGateway](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2VpnGateway.html) \(AWS Tools for Windows PowerShell\)