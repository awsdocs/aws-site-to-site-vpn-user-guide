# Logging and monitoring<a name="logging-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of your AWS Site\-to\-Site VPN connection\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides several tools for monitoring your resources and responding to potential incidents\.

**Amazon CloudWatch**  
*Amazon CloudWatch* monitors your AWS resources and the applications that you run on AWS in real time\. You can collect and track metrics for your Site\-to\-Site VPN tunnels, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For more information, see [Monitoring your Site\-to\-Site VPN connection](monitoring-overview-vpn.md)\.

**AWS CloudTrail**  
*AWS CloudTrail* captures Amazon EC2 API calls and related events made by or on behalf of your AWS account\. It then delivers the log files to an Amazon S3 bucket that you specify\. For more information, see [Logging Amazon EC2, Amazon EBS, and Amazon VPC API calls with AWS CloudTrail](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html) in the *Amazon EC2 API Reference*\.

**AWS Trusted Advisor**  
AWS Trusted Advisor draws upon best practices learned from serving hundreds of thousands of AWS customers\. Trusted Advisor inspects your AWS environment, and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps\.

Trusted Advisor has a check for VPN tunnel redundancy, which checks the number of tunnels that are active for each of your VPN connections\.

For more information, see [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html#trusted-advisor) in the *AWS Support User Guide*\.