# Authentication and Access Control for Your AWS Site\-to\-Site VPN<a name="vpn-authentication-access-control"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way, without sharing your security credentials\.

By default, IAM users do not have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources, such as Site\-to\-Site VPN connections, virtual private gateways, and customer gateways, and to perform tasks, you must:
+ Create an IAM policy that grants the IAM user permission to use the specific resources and API actions they will need
+ Attach the policy to the IAM user or the group to which the IAM user belongs

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

To work with a Site\-to\-Site VPN connections, virtual private gateways, and customer gateways, one of the following AWS managed policies might meet your needs:
+ **PowerUserAccess**
+ **ReadOnlyAccess**
+ **AmazonEC2FullAccess**
+ **AmazonEC2ReadOnlyAccess**

For more information about policies, see [IAM Policies for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) in the *Amazon EC2 User Guide*\.