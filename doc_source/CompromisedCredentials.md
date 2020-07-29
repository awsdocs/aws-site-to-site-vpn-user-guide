# Replacing compromised credentials<a name="CompromisedCredentials"></a>

If you believe that the tunnel credentials for your Site\-to\-Site VPN connection have been compromised, you can change the IKE pre\-shared key or change the ACM certificate\. The method you use depends on the authentication option you used for your VPN tunnels\. For more information, see [Site\-to\-Site VPN tunnel authentication options](vpn-tunnel-authentication-options.md)\.

**To change the IKE pre\-shared key**  
You can modify the tunnel options for the Site\-to\-Site VPN connection and specify a new IKE pre\-shared key for each tunnel\. For more information, see [Modifying Site\-to\-Site VPN tunnel options](modify-vpn-tunnel-options.md)\.

Alternatively, you can delete the Site\-to\-Site VPN connection\. For more information, see [Deleting a Site\-to\-Site VPN connection](delete-vpn.md)\. You don't need to delete the VPC or the virtual private gateway\. Then, create a new Site\-to\-Site VPN connection using the same virtual private gateway, and configure the new keys on your customer gateway device\. You can specify your own pre\-shared keys for the tunnels or let AWS generate new pre\-shared keys for you\. For more information, see [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)\. The tunnel's inside and outside addresses might change when you recreate the Site\-to\-Site VPN connection\.

**To change the certificate for the AWS side of the tunnel endpoint**  
Rotate the certificate\. For more information, see [Rotating Site\-to\-Site VPN tunnel endpoint certificates](roate-vpn-certificate.md)\.

**To change the certificate on the customer gateway device**

1. Create a new certificate\. For information about creating an ACM certificate, see [Getting started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.

1. Add the certificate to the customer gateway device\.