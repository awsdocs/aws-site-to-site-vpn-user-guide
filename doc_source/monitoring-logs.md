# AWS Site\-to\-Site VPN logs<a name="monitoring-logs"></a>

AWS Site\-to\-Site VPN logs provide you with deeper visibility into your Site\-to\-Site VPN deployments\. With this feature, you have access to Site\-to\-Site VPN connection logs that provide details on IP Security \(IPsec\) tunnel establishment, Internet Key Exchange \(IKE\) negotiations, and dead peer detection \(DPD\) protocol messages\.

Site\-to\-Site VPN logs can be published to Amazon CloudWatch Logs\. This feature provides customers with a single consistent way to access and analyze detailed logs for all of their Site\-to\-Site VPN connections\.

**Topics**
+ [Benefits of Site\-to\-Site VPN logs](#log-benefits)
+ [Amazon CloudWatch Logs resource policy size restrictions](#cwl-policy-size)
+ [Contents of Site\-to\-Site VPN logs](log-contents.md)
+ [IAM requirements to publish to CloudWatch Logs](#publish-cw-logs)
+ [View Site\-to\-Site VPN logs configuration](#status-logs)
+ [Enable Site\-to\-Site VPN logs](#enable-logs)
+ [Disable Site\-to\-Site VPN logs](#disable-logs)

## Benefits of Site\-to\-Site VPN logs<a name="log-benefits"></a>
+ **Simplified VPN troubleshooting:** Site\-to\-Site VPN logs help you to pinpoint configuration mismatches between AWS and your customer gateway device, and address initial VPN connectivity issues\. VPN connections can intermittently flap over time due to misconfigured settings \(such as poorly tuned timeouts\), there can be issues in the underlying transport networks \(like internet weather\), or routing changes or path failures can cause disruption of connectivity over VPN\. This feature allows you to accurately diagnose the cause of intermittent connection failures and fine\-tune low\-level tunnel configuration for reliable operation\.
+ **Centralized AWS Site\-to\-Site VPN visibility:** Site\-to\-Site VPN logs can provide tunnel activity logs for all of the different ways that Site\-to\-Site VPN is connected: Virtual Gateway, Transit Gateway, and CloudHub, using both internet and AWS Direct Connect as transport\. This feature provides customers with a single consistent way to access and analyze detailed logs for all of their Site\-to\-Site VPN connections\.
+ **Security and compliance:** Site\-to\-Site VPN logs can be sent to Amazon CloudWatch Logs for retrospective analysis of VPN connection status and activity over time\. This can help you meet compliance and regulatory requirements\.

## Amazon CloudWatch Logs resource policy size restrictions<a name="cwl-policy-size"></a>

CloudWatch Logs resource policies are limited to 5120 characters\. When CloudWatch Logs detects that a policy approaches this size limit, it automatically enables log groups that start with `/aws/vendedlogs/`\. When you enable logging, Site\-to\-Site VPN must update your CloudWatch Logs resource policy with the log group you specify\. To avoid reaching the CloudWatch Logs resource policy size limit, prefix your log group names with `/aws/vendedlogs/`\.

## IAM requirements to publish to CloudWatch Logs<a name="publish-cw-logs"></a>



VPN tunnel logs can be published directly to CloudWatch Logs\. For this to work properly, the IAM policy that's attached to your IAM role must include at least the permissions shown in the following\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "logs:CreateLogDelivery",
        "logs:GetLogDelivery",
        "logs:UpdateLogDelivery",
        "logs:DeleteLogDelivery",
        "logs:ListLogDeliveries"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow",
      "Sid": "S2SVPNLogging"
    },
    {
      "Sid": "S2SVPNLoggingCWL",
      "Action": [
        "logs:PutResourcePolicy",
        "logs:DescribeResourcePolicies",
        "logs:DescribeLogGroups"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

## View Site\-to\-Site VPN logs configuration<a name="status-logs"></a>

**To view current tunnel logging settings**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the VPN connection that you want to view from the **VPN connections** list\.

1. Choose the **Tunnel details** tab\.

1. Expand the **Tunnel 1 options** and **Tunnel 2 options** sections to view all tunnel configuration details\.

1. You can view the current status of the logging feature under **Tunnel VPN log**, and the currently configured CloudWatch log group \(if any\) under **CloudWatch log group**\.

**To view current tunnel logging settings on a Site\-to\-Site VPN connection using the AWS command line or API**
+ [DescribeVpnConnections](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-DescribeVpnConnections.html) \(Amazon EC2 Query API\)
+ [describe\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpn-connections.html) \(AWS CLI\)

## Enable Site\-to\-Site VPN logs<a name="enable-logs"></a>

**Note**  
When you enable Site\-to\-Site VPN logs for an existing VPN connection tunnel, your connectivity over that tunnel can be interrupted for several minutes\. However, each VPN connection offers two tunnels for high availability, so you can enable logging on one tunnel at a time while maintaining connectivity over the tunnel not being modified\. For more information, see [Site\-to\-Site VPN tunnel endpoint replacements](endpoint-replacements.md)\.

**To enable VPN logging during creation of a new Site\-to\-Site VPN connection**  
Follow the procedure [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)\. During Step 9 **Tunnel Options**, you can specify all the options you want to use for both tunnels, including **VPN logging** options\. For more information about these options, see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\.

**To enable tunnel logging on a new Site\-to\-Site VPN connection using the AWS command line or API**
+ [CreateVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-CreateVpnConnection.html) \(Amazon EC2 Query API\)
+ [create\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpn-connection.html) \(AWS CLI\)

**To enable tunnel logging on an existing Site\-to\-Site VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the VPN connection that you want to modify from the **VPN connections** list\.

1. Select **Actions**, **Modify VPN tunnel options**\.

1. Select the tunnel that you want to modify by choosing the appropriate IP address from the **VPN tunnel outside IP address** list\.

1. Under **Tunnel activity log**, select **Enable**\.

1. Under **Amazon CloudWatch log group**, select the Amazon CloudWatch log group where you want the logs to be sent\.

1. \(Optional\) Under **Output format**, choose the desired format for the log output, either **json** or **text**\.

1. Select **Save changes**\.

1. \(Optional\) Repeat steps 4 through 9 for the other tunnel if desired\.

**To enable tunnel logging on an existing Site\-to\-Site VPN connection using the AWS command line or API**
+ [ModifyVpnTunnelOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-ModifyVpnTunnelOptions.html) \(Amazon EC2 Query API\)
+ [modify\-vpn\-tunnel\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel-options.html) \(AWS CLI\)

## Disable Site\-to\-Site VPN logs<a name="disable-logs"></a>

**To disable tunnel logging on a Site\-to\-Site VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the VPN connection that you want to modify from the **VPN connections** list\.

1. Select **Actions**, **Modify VPN tunnel options**\.

1. Select the tunnel that you want to modify by choosing the appropriate IP address from the **VPN tunnel outside IP address** list\.

1. Under **Tunnel activity log**, clear **Enable**\.

1. Select **Save changes**\.

1. \(Optional\) Repeat steps 4 through 7 for the other tunnel if desired\.

**To disable tunnel logging on a Site\-to\-Site VPN connection using the AWS command line or API**
+ [ModifyVpnTunnelOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ApiReference-query-ModifyVpnTunnelOptions.html) \(Amazon EC2 Query API\)
+ [modify\-vpn\-tunnel\-options](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-tunnel-options.html) \(AWS CLI\)