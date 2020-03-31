# AWS Site\-to\-Site VPN service\-linked role<a name="vpn-service-linked-roles"></a>

AWS Site\-to\-Site VPN uses service\-linked roles for the permissions that it requires to call other AWS services on your behalf\. For more information, see [Using service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

## Permissions granted by the service\-linked role<a name="service-linked-role-permissions"></a>

When you work with a Site\-to\-Site VPN connection that uses certificate\-based authentication, Site\-to\-Site VPN uses the service\-linked role named **AWSServiceRoleForVPCS2SVPN** to call the following AWS Certificate Manager \(ACM\) actions on your behalf:
+ `acm:ExportCertificate`
+ `acm:DescribeCertificatee`
+ `acm:ListCertificates`
+ `acm-pca:DescribeCertificateAuthority`

## Create the service\-linked role<a name="create-service-linked-role"></a>

You don't need to manually create the **AWSServiceRoleForVPCS2SVPN** role\. Site\-to\-Site VPN creates this role for you when you create a customer gateway with an associated ACM private certificate\.

For a Site\-to\-Site VPN user to create a service\-linked role on your behalf, you must have the required permissions\. For information about service\-linked roles, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Edit the service\-linked role<a name="edit-service-linked-role"></a>

You can edit the description of **AWSServiceRoleForVPCS2SVPN** using IAM\. For information about editing service\-linked roles, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Delete the service\-linked role<a name="delete-service-linked-role"></a>

If you no longer need to use the Site\-to\-Site VPN connections with certificate\-based authentication, we recommend that you delete **AWSServiceRoleForVPCS2SVPN**\.

You can delete this service\-linked role only after you delete all customer gateways that have an associated ACM private certificate\. This ensures that you cannot inadvertently remove permission to access your ACM certificates in use by Site\-to\-Site VPN connections\.

You can use the IAM console, the IAM CLI, or the IAM API to delete service\-linked roles\. For information about deleting service\-linked roles, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

After you delete **AWSServiceRoleForVPCS2SVPN**, Amazon VPC creates the role again for a customer gateway with an associated ACM private certificate\.