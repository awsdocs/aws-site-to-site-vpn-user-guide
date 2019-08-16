# Site\-to\-Site VPN Tunnel Authentication Options<a name="vpn-tunnel-authentication-options"></a>

You can use pre\-shared keys, or certificates to authenticate your Site\-to\-Site VPN tunnel endpoints\.

## Pre\-Shared Keys<a name="pre-shared-keys"></a>

A pre\-shared key is the default authentication option\. 

A pre\-shared key is a Site\-to\-Site VPN tunnel option that you can specify when you create a Site\-to\-Site VPN tunnel\.

A pre\-shared key is a string that you enter when you configure your customer gateway device\. If you do not specify a string, we auto\-generate one for you\.

## Private Certificate from AWS Certificate Manager Private Certificate Authority<a name="certificate"></a>

If you do not want to use pre\-shared keys, you can use a private certificate from AWS Certificate Manager Private Certificate Authority to authenticate your VPN\. 

You must create a private certificate using AWS Certificate Manager Private Certificate Authority\. For information about creating a private certificate, see [Creating and Managing a Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCreatingManagingCA.html) in the *AWS Certificate Manager Private Certificate Authority User Guide*\.

You must create a service\-link role to generate and use the certificate for the AWS side of the Site\-to\-Site VPN tunnel endpoint\. For more information, see [Permissions Granted by the Service\-Linked Role](vpn-service-linked-roles.md#service-linked-role-permissions)\.

After you generate the private certificate, you specify the certificate when you create the customer gateway, and then apply it to your customer gateway device\.

If you do not specify the IP address of your customer gateway device, we do not check the IP address\. This operation allows you to move the customer gateway device to a different IP address without having to re\-configure the VPN connection\. 