# Testing the Site\-to\-Site VPN connection<a name="HowToTestEndToEnd_Linux"></a>

After you create the AWS Site\-to\-Site VPN connection and configure the customer gateway, you can launch an instance and test the connection by pinging the instance\. 

Before you begin, make sure of the following:
+ Use an AMI that responds to ping requests\. We recommend that you use one of the Amazon Linux AMIs\.
+ Configure any security group or network ACL in your VPC that filters traffic to the instance to allow inbound and outbound ICMP traffic\.
+ If you are using instances running Windows Server, connect to the instance and enable inbound ICMPv4 on the Windows firewall in order to ping the instance\.
+ \(Static routing\) Ensure that the customer gateway device has a static route to your VPC, and that your VPN connection has a static route so that traffic can get back to your customer gateway device\.
+ \(Dynamic routing\) Ensure that the BGP status on your customer gateway device is established\. It takes approximately 30 seconds for a BGP peering session to be established\. Ensure that routes are advertised with BGP correctly and showing in the subnet route table, so that traffic can get back to your customer gateway\. Make sure that both tunnels are configured with BGP routing\.
+ Ensure that you have configured routing in your subnet route tables for the VPN connection\.

**To test end\-to\-end connectivity**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the dashboard, choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, choose an AMI, and then choose **Select**\.

1. Choose an instance type, and then choose **Next: Configure Instance Details**\. 

1. On the **Configure Instance Details** page, for **Network**, select your VPC\. For **Subnet**, select your subnet\. Choose **Next** until you reach the **Configure Security Group** page\.

1. Select the **Select an existing security group** option, and then select the group that you configured earlier\. Choose **Review and Launch**\.

1. Review the settings that you've chosen\. Make any changes that you need, and then choose **Launch** to select a key pair and launch the instance\.

1. After the instance is running, get its private IP address \(for example, `10.0.0.4`\)\. The Amazon EC2 console displays the address as part of the instance's details\.

1. From a computer in your network that is behind the customer gateway device, use the `ping` command with the instance's private IP address\. A successful response is similar to the following:

   ```
   ping 10.0.0.4
   ```

   ```
   Pinging 10.0.0.4 with 32 bytes of data:
   
   Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
   
   Ping statistics for 10.0.0.4:
   Packets: Sent = 3, Received = 3, Lost = 0 (0% loss),
   
   Approximate round trip times in milliseconds:
   Minimum = 0ms, Maximum = 0ms, Average = 0ms
   ```

To test tunnel failover, you can temporarily disable one of the tunnels on your customer gateway device, and repeat the above step\. You cannot disable a tunnel on the AWS side of the VPN connection\.

You can use SSH or RDP to connect to your instances in the VPC\. For more information about how to connect to a Linux instance, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#EC2_ConnectToInstance_Linux) in the *Amazon EC2 User Guide for Linux Instances*\. For more information about how to connect to a Windows instance, see [Connect to your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2Win_GetStarted.html#EC2Win_ConnectToInstanceWindows) in the *Amazon EC2 User Guide for Windows Instances*\. 