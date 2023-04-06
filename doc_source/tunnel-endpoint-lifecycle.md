# Tunnel endpoint lifecycle control<a name="tunnel-endpoint-lifecycle"></a>

Tunnel endpoint lifecycle control provides control over the schedule of endpoint replacements, and can help minimize connectivity disruptions during AWS managed tunnel endpoint replacements\. With this feature, you can choose to accept AWS managed updates to tunnel endpoints at a time that works best for your business\. Use this feature if you have short\-term business needs or can only support a single tunnel per VPN connection\.

**Note**  
In rare circumstances, AWS might apply critical updates to tunnel endpoints immediately, even if the tunnel endpoint lifecycle control feature is enabled\.

**Topics**
+ [How tunnel endpoint lifecycle control works](#how-elc-works)
+ [Enable tunnel endpoint lifecycle control](#enable-elc)
+ [Verify if tunnel endpoint lifecycle control is enabled](#view-elc-status)
+ [Check for available updates](#view-elc-updates)
+ [Accept a maintenance update](#accept-update)
+ [Turn tunnel endpoint lifecycle control off](#turn-elc-off)

## How tunnel endpoint lifecycle control works<a name="how-elc-works"></a>

Turn on the tunnel endpoint lifecycle control feature for individual tunnels within a VPN connection\. It can be enabled at the time of VPN creation or by modifying tunnel options for an existing VPN connection\.

After tunnel endpoint lifecycle control is enabled, you will gain additional visibility into upcoming tunnel maintenance events in two ways:
+ You will receive AWS Health notifications for upcoming tunnel endpoint replacements\.
+ The status of pending maintenance, along with the **Maintenance auto applied after** and **Last maintenance applied** timestamps, can be seen in the AWS Management Console or by using the [get\-vpn\-tunnel\-replacement\-status](https://docs.aws.amazon.com/cli/latest/reference/ec2/get-vpn-tunnel-replacement-status.html) AWS CLI command\.

When a tunnel endpoint maintenance is available, you will have the opportunity to accept the update at a time that is convenient for you, before the given **Maintenance auto applied after** timestamp\.

If you do not apply updates before the **Maintenance auto applied after** date, AWS will automatically perform the tunnel endpoint replacement soon after, as part of the regular maintenance update cycle\.

## Enable tunnel endpoint lifecycle control<a name="enable-elc"></a>

You can enable this feature using the AWS Management Console or AWS CLI\.

**Note**  
By default when you turn on the feature for an existing VPN connection, a tunnel endpoint replacement will be initiated at the same time\. If you want to turn the feature on, but not initiate an tunnel endpoint replacement immediately, you can use the **skip tunnel replacement** option\.

------
#### [ Existing VPN connection ]

The following steps demonstrate how to enable tunnel endpoint lifecycle control on an existing VPN connection\.

**To enable tunnel endpoint lifecycle control using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left\-side navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the appropriate connection under **VPN connections**\.

1. Choose **Actions**, then **Modify VPN tunnel options**\.

1. Select the specific tunnel that you want to modify by choosing the appropriate **VPN tunnel outside IP address**\.

1. Under **Tunnel Endpoint Lifecycle Control**, select the **Enable** check box\.

1. \(Optional\) Select **Skip tunnel replacement**\.

1. Choose **Save changes**\.

**To enable tunnel endpoint lifecycle control using the AWS CLI**  
Use the [modify\-vpn\-tunnel\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel-options.html) command to turn on tunnel endpoint lifecycle control\.

------
#### [ New VPN connection ]

The following steps demonstrate how to enable tunnel endpoint lifecycle control during creation of a new VPN connection\.

**To enable tunnel endpoint lifecycle control during creation of a new VPN connection using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Choose **Create VPN connection**\.

1. In the sections for **Tunnel 1 options** and **Tunnel 2 options**, under **Tunnel Endpoint Lifecycle Control**, select **Enable**\.

1. Choose **Create VPN Connection**\.

**To enable tunnel endpoint lifecycle control during creation of a new VPN connection using the AWS CLI**  
Use the [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) command to turn on tunnel endpoint lifecycle control\.

------

## Verify if tunnel endpoint lifecycle control is enabled<a name="view-elc-status"></a>

You can verify whether tunnel endpoint lifecycle control is enabled on an existing VPN tunnel by using the AWS Management Console or CLI\.

**To verify if tunnel endpoint lifecycle control is enabled using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left\-side navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the appropriate connection under **VPN connections**\.

1. Select the **Tunnel details** tab\.

1. In the tunnel details, look for **Tunnel Endpoint Lifecycle Control**, which will report whether the feature is **Enabled** or **Disabled**\. 

**To verify if tunnel endpoint lifecycle control is enabled using the AWS CLI**  
Use the [describe\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-connections.html) command to verify if tunnel endpoint lifecycle control is enabled\.

## Check for available updates<a name="view-elc-updates"></a>

After you enable the tunnel endpoint lifecycle control feature, you can view whether a maintenance update is available for your VPN connection by using the AWS Management Console or CLI\.

**To check for available updates using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left\-side navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the appropriate connection under **VPN connections**\.

1. Select the **Tunnel details** tab\.

1. Check the **Pending maintenance** column\. The status will be either **Available** or **None**\.

**To check for available updates using the AWS CLI**  
Use the [get\-vpn\-tunnel\-replacement\-status](https://docs.aws.amazon.com/cli/latest/reference/ec2/get-vpn-tunnel-replacement-status.html) command to check for available updates\.

## Accept a maintenance update<a name="accept-update"></a>

When a maintenance update is available, you can accept it using the AWS Management Console or CLI\.

**To accept an available maintenance update using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left\-side navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the appropriate connection under **VPN connections**\.

1. Choose **Actions**, then **Replace VPN Tunnel**\.

1. Select the specific tunnel that you want to replace by choosing the appropriate **VPN tunnel outside IP address**\.

1. Choose **Replace**\.

**To accept an available maintenance update using the AWS CLI**  
Use the [replace\-vpn\-tunnel](https://docs.aws.amazon.com/cli/latest/reference/ec2/replace-vpn-tunnel.html) command to accept an available maintenance update\.

## Turn tunnel endpoint lifecycle control off<a name="turn-elc-off"></a>

If you no longer want to use the tunnel endpoint lifecycle control feature, you can turn it off using the AWS Management Console or the AWS CLI\. When you turn off this feature, AWS will automatically deploy maintenance updates periodically, and these updates might happen during your business hours\. To avoid any business impact, we highly recommend that you configure both tunnels in your VPN connection for high availability\.

**Note**  
While there is an available pending maintenance, you cannot specify the **skip tunnel replacement** option while turning the feature off\. You can always turn the feature off without using the **skip tunnel replacement** option, but AWS will automatically deploy the available pending maintenance updates by initiating a tunnel endpoint replacement immediately\.

**To turn off tunnel endpoint lifecycle control using the AWS Management Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the left\-side navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the appropriate connection under **VPN connections**\.

1. Choose **Actions**, then **Modify VPN tunnel options**\.

1. Select the specific tunnel that you want to modify by choosing the appropriate **VPN tunnel outside IP address**\.

1. To turn off tunnel endpoint lifecycle control, under **Tunnel Endpoint Lifecycle Control**, clear the **Enable** check box\.

1. \(Optional\) Select **Skip tunnel replacement**\.

1. Choose **Save changes**\.

**To turn off tunnel endpoint lifecycle control using the AWS CLI**  
Use the [modify\-vpn\-tunnel\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel-options.html) command to turn off tunnel endpoint lifecycle control\.