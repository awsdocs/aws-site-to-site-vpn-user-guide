# Using service\-linked roles for Site\-to\-Site VPN<a name="using-service-linked-roles"></a>

AWS Site\-to\-Site VPN uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Site\-to\-Site VPN\. Service\-linked roles are predefined by Site\-to\-Site VPN and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Site\-to\-Site VPN easier because you don’t have to manually add the necessary permissions\. Site\-to\-Site VPN defines the permissions of its service\-linked roles, and unless defined otherwise, only Site\-to\-Site VPN can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Site\-to\-Site VPN resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Site\-to\-Site VPN<a name="slr-permissions"></a>

Site\-to\-Site VPN uses the service\-linked role named **AWSServiceRoleForVPCS2SVPN** – Allow Site\-to\-Site VPN to create and manage resources related to your VPN connections\.

The AWSServiceRoleForVPCS2SVPN service\-linked role trusts the following services to assume the role:
+ `AWS Certificate Manager`
+ `AWS Private Certificate Authority`

The role permissions policy named AWSVPCS2SVpnServiceRolePolicy allows Site\-to\-Site VPN to complete the following actions on the specified resources:
+ Action: `acm:ExportCertificate` on `Resource: "*"`
+ Action: `acm:DescribeCertificate` on `Resource: "*"`
+ Action: `acm:ListCertificates` on `Resource: "*"`
+ Action: `acm-pca:DescribeCertificateAuthority` on `Resource: "*"`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Site\-to\-Site VPN<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you create a customer gateway with an associated ACM private certificate in the AWS Management Console, the AWS CLI, or the AWS API, Site\-to\-Site VPN creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create a customer gateway with an associated ACM private certificate, Site\-to\-Site VPN creates the service\-linked role for you again\. 

## Editing a service\-linked role for Site\-to\-Site VPN<a name="edit-slr"></a>

Site\-to\-Site VPN does not allow you to edit the AWSServiceRoleForVPCS2SVPN service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Site\-to\-Site VPN<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

**Note**  
If the Site\-to\-Site VPN service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete Site\-to\-Site VPN resources used by the AWSServiceRoleForVPCS2SVPN**  
You can delete this service\-linked role only after you delete all customer gateways that have an associated ACM private certificate\. This ensures that you cannot inadvertently remove permission to access your ACM certificates in use by Site\-to\-Site VPN connections\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForVPCS2SVPN service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.