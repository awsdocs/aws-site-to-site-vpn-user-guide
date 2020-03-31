# Replacing compromised credentials<a name="CompromisedCredentials"></a>

If you believe that the tunnel credentials for your Site\-to\-Site VPN connection have been compromised, you can change the IKE pre\-shared key\. To do so, delete the Site\-to\-Site VPN connection, create a new one using the same virtual private gateway, and configure the new keys on your customer gateway\. You can specify your own pre\-shared keys when you create the Site\-to\-Site VPN connection\. You also need to confirm that the tunnel's inside and outside addresses match, because these might change when you recreate the Site\-to\-Site VPN connection\. While you perform the procedure, communication with your instances in the VPC stops, but the instances continue to run uninterrupted\. After the network administrator implements the new configuration information, your Site\-to\-Site VPN connection uses the new credentials, and the network connection to your instances in the VPC resumes\.

**Important**  
This procedure requires assistance from your network administrator group\.

**To change the IKE pre\-shared key**

1. Delete the Site\-to\-Site VPN connection\. For more information, see [Deleting a Site\-to\-Site VPN connection](delete-vpn.md)\. You don't need to delete the VPC or the virtual private gateway\.

1. Create a new Site\-to\-Site VPN connection and specify your own pre\-shared keys for the tunnels or let AWS generate new pre\-shared keys for you\. For more information, see [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)\.

1. Download the new configuration file\.

**To change the certificate for the AWS side of the tunnel endpoint**
+ Rotate the certificate\. For more information, see [Rotating Site\-to\-Site VPN tunnel endpoint certificates](roate-vpn-certificate.md)\.

**To change the certificate on the customer gateway device**

1. Create a new certificate, For information about creating an ACM certificate, see [Getting started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.

1. Add the certificate to the customer gateway device\.