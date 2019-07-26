# Migrating from AWS Classic VPN to AWS VPN<a name="aws-vpn-migrate"></a>

If your existing Site\-to\-Site VPN connection is an AWS Classic VPN connection, you can migrate to an AWS VPN connection by creating a new virtual private gateway and Site\-to\-Site VPN connection, detaching the old virtual private gateway from your VPC, and attaching the new virtual private gateway to your VPC\.

If your existing virtual private gateway is associated with multiple Site\-to\-Site VPN connections, you must recreate each Site\-to\-Site VPN connection for the new virtual private gateway\. If there are multiple AWS Direct Connect private virtual interfaces attached to your virtual private gateway, you must recreate each private virtual interface for the new virtual private gateway\. For more information, see [Creating a Virtual Interface](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-vif.html) in the *AWS Direct Connect User Guide*\.

If your existing Site\-to\-Site VPN connection is an AWS VPN connection, you cannot migrate to an AWS Classic VPN connection\.

**Note**  
During this procedure, connectivity over the current VPC connection is interrupted when you disable route propagation and detach the old virtual private gateway from your VPC\. Connectivity is restored when the new virtual private gateway is attached to your VPC and the new Site\-to\-Site VPN connection is active\. Ensure that you plan for the expected downtime\.

**To migrate to an AWS VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Virtual Private Gateways**, **Create Virtual Private Gateway** and create a virtual private gateway\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN Connection**\. Specify the following information, and choose **Yes, Create**\.
   + **Virtual Private Gateway**: Select the virtual private gateway that you created in the previous step\.
   + **Customer Gateway**: Choose **Existing**, and select the existing customer gateway for your current AWS Classic VPN connection\.
   + Specify the routing options as required\.

1. Select the new Site\-to\-Site VPN connection and choose **Download Configuration**\. Download the appropriate configuration file for your customer gateway device\.

1. Use the configuration file to configure VPN tunnels on your customer gateway device\. For examples, see the *[Amazon VPC Network Administrator Guide](https://docs.aws.amazon.com/vpc/latest/adminguide/)*\. Do not enable the tunnels yet\. Contact your vendor if you need guidance on keeping the newly configured tunnels disabled\. 

1. \(Optional\) Create test VPC and attach the virtual private gateway to the test VPC\. Change the encryption domain/source destination addresses as required, and test connectivity from a host in your local network to a test instance in the test VPC\.

1. If you are using route propagation for your route table, choose **Route Tables** in the navigation pane\. Select the route table for your VPC, and choose **Route Propagation**, **Edit**\. Clear the check box for the old virtual private gateway and choose **Save**\.
**Note**  
From this step onwards, connectivity is interrupted until the new virtual private gateway is attached and the new Site\-to\-Site VPN connection is active\.

1. In the navigation pane, choose **Virtual Private Gateways**\. Select the old virtual private gateway and choose **Actions**, **Detach from VPC**, **Yes, Detach**\. Select the new virtual private gateway, and choose **Actions**, **Attach to VPC**\. Specify the VPC for your Site\-to\-Site VPN connection, and choose **Yes, Attach**\. 

1. In the navigation pane, choose **Route Tables**\. Select the route table for your VPC and do one of the following: 
   + If you are using route propagation, choose **Route Propagation**, **Edit**\. Select the new virtual private gateway that's attached to the VPC and choose **Save**\.
   + If you are using static routes, choose **Routes**, **Edit**\. Modify the route to point to the new virtual private gateway, and choose **Save**\.

1. Enable the new tunnels on your customer gateway device and disable the old tunnels\. To bring the tunnel up, you must initiate the connection from your local network\.

   If applicable, check your route table to ensure that the routes are being propagated\. The routes propagate to the route table when the status of the VPN tunnel is `UP`\. 
**Note**  
If you need to revert to your previous configuration, detach the new virtual private gateway and follow steps 8 and 9 to re\-attach the old virtual private gateway and update your routes\.

1. If you no longer need your AWS Classic VPN connection and do not want to continue incurring charges for it, remove the previous tunnel configurations from your customer gateway device, and delete the Site\-to\-Site VPN connection\. To do this, go to **Site\-to\-Site VPN Connections**, select the Site\-to\-Site VPN connection, and choose **Delete**\.
**Important**  
After you've deleted the AWS Classic VPN connection, you cannot revert or migrate your new AWS VPN connection back to an AWS Classic VPN connection\.