# Identifying a Site\-to\-Site VPN connection<a name="identify-vpn"></a>

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