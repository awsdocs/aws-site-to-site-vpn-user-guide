# Identity and access management for AWS Site\-to\-Site VPN<a name="vpn-authentication-access-control"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way, without sharing your security credentials\.

By default, IAM users do not have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources, such as Site\-to\-Site VPN connections, virtual private gateways, and customer gateways, and to perform tasks, you must:
+ Create an IAM policy that grants the IAM user permission to use the specific resources and API actions that they need
+ Attach the policy to the IAM user or the group to which the IAM user belongs

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

Site\-to\-Site VPN is part of Amazon VPC, which shares its API namespace with Amazon EC2\. To work with Site\-to\-Site VPN connections, virtual private gateways, and customer gateways, one of the following AWS managed policies might meet your needs:
+ **PowerUserAccess**
+ **ReadOnlyAccess**
+ **AmazonEC2FullAccess**
+ **AmazonEC2ReadOnlyAccess**

Take care when granting users permission to use the `ec2:DescribeVpnConnections` action\. This action enables users to view customer gateway configuration information for Site\-to\-Site VPN connections in your account\.

For more examples, see [Identity and Access Management for Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/security-iam.html) in the *Amazon VPC User Guide* and [IAM Policies for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) in the *Amazon EC2 User Guide*\.

## IAM policies for your Site\-to\-Site VPN connection<a name="iam-policies-vpn-connection"></a>

You can use resource\-level permissions to restrict what resources users can use when they invoke APIs\. You can specify the Amazon Resource Name \(ARN\) for the VPN connection in the `Resource` element of IAM permission policy statements \(for example, `arn:aws:ec2:us-west-2:123456789012:vpn-connection/vpn-0d4e855ab7d3536fb`\)\.

The following actions support resource\-level permissions for the VPN connection resource:
+ `ec2:CreateVpnConnection`
+ `ec2:ModifyVpnConnection`
+ `ec2:ModifyVpnTunnelOptions`

The following are the supported condition keys:


| Condition key | Description | Valid values | Type | 
| --- | --- | --- | --- | 
| ec2:AuthenticationType | The authentication type for the VPN tunnel endpoints\. | Pre Shared Keys, Certificates | String | 
| ec2:DPDTimeoutSeconds | The duration after which DPD timeout occurs\. | An integer between 0 and 30 | Numeric | 
| ec2:GatewayType | The gateway type for the VPN endpoint on the AWS side of the VPN connection\. | VGW, TGW | String | 
| ec2:IKEVersions | The internet key exchange \(IKE\) versions that are permitted for the VPN tunnel\. | ikev1, ikev2 | String | 
| ec2:InsideTunnelCidr | The range of inside IP addresses for the VPN tunnel\. | See [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)  | String | 
| ec2:Phase1DHGroupNumbers | The Diffie\-Hellman groups that are permitted for the VPN tunnel for the phase 1 IKE negotiations\. | 2, 14, 15, 16, 17, 18, 22, 23, 24 | Numeric | 
| ec2:Phase2DHGroupNumbers | The Diffie\-Hellman groups that are permitted for the VPN tunnel for the phase 2 IKE negotiations\. | 2, 5, 14, 15, 16, 17, 18, 22, 23, 24 | Numeric | 
| ec2:Phase1EncryptionAlgorithms | The encryption algorithms that are permitted for the VPN tunnel for the phase 1 IKE negotiations\. | AES128, AES256 | String | 
| ec2:Phase2EncryptionAlgorithms | The encryption algorithms that are permitted for the VPN tunnel for the phase 2 IKE negotiations\. | AES128, AES256 | String | 
| ec2:Phase1IntegrityAlgorithms | The integrity algorithms that are permitted for the VPN tunnel for the phase 1 IKE negotiations\. | SHA1, SHA2\-256 | String | 
| ec2:Phase2IntegrityAlgorithms | The integrity algorithms that are permitted for the VPN tunnel for the phase 2 IKE negotiations\. | SHA1, SHA2\-256 | String | 
| ec2:Phase1LifetimeSeconds | The lifetime in seconds for phase 1 of the IKE negotiation\. | An integer between 900 and 28,800 | Numeric | 
| ec2:Phase2LifetimeSeconds | The lifetime in seconds for phase 2 of the IKE negotiation\. | An integer between 900 and 3,600 | Numeric | 
| ec2:PresharedKeys | The pre\-shared key \(PSK\) to establish the initial IKE security association between the virtual private gateway and customer gateway\. | See [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)  | String | 
| ec2:RekeyFuzzPercentage | The percentage of the rekey window \(determined by the rekey margin time\) within which the rekey time is randomly selected\.  | An integer between 0 and100 | Numeric | 
| ec2:RekeyMarginTimeSeconds | The margin time before the phase 2 lifetime expires, during which AWS performs an IKE rekey\. | An integer from 60 and above | Numeric | 
| ec2:RoutingType | The routing type for the VPN connection\. | Static, BGP | String | 

You can allow or deny specific values for each supported condition key using IAM condition operators\. For more information, see [IAM JSON policy elements: condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

The following example policy enables users to create VPN connections, but only VPN connections with static routing types\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "statement1",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpnConnection"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:RoutingType": [
                        "static"
                    ]
                }
            }
        }
    ]
}
```