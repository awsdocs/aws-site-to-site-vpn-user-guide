# Rotating Site\-to\-Site VPN Tunnel Endpoint Certificates<a name="roate-vpn-certificate"></a>

You can rotate the certificates on the tunnel endpoints on the AWS side by using by using the Amazon VPC console\. When a tunnel endpoint’s certificate is close to expiration, AWS automatically rotates the certificate using the service\-linked role\. For more information, see [Permissions Granted by the Service\-Linked Role](vpn-service-linked-roles.md#service-linked-role-permissions)\.

**To identify the Site\-to\-Site VPN category using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection, and then choose **Actions, Rotate Tunnel Certificates**\.

1. Select the tunnel endpoint whose certificate you want to rotate\.

1. Choose **Save**\.