# Migrating from AWS Classic VPN to AWS VPN<a name="aws-vpn-migrate"></a>

If your existing Site\-to\-Site VPN connection is an AWS Classic VPN connection, you can migrate to an AWS VPN connection\. You can migrate directly to a new virtual private gateway \(option 1\), or you can migrate using a transit gateway \(option 2\)\. During the procedure for option 1, your Site\-to\-Site VPN connection is temporarily interrupted when you detach the old virtual private gateway from your VPC\. During the procedure for option 2, your Site\-to\-Site VPN connection is not interrupted, however you will incur additional [transit gateway costs](https://aws.amazon.com/transit-gateway/pricing/)\. 

If you use an AWS Classic VPN connection as a backup for your AWS Direct Connect connection, you can delete and recreate the Site\-to\-Site VPN connection \(option 3\)\. During the procedure for option 3, there is zero downtime on the AWS Direct Connect private virtual interface\.

If your existing virtual private gateway is associated with multiple Site\-to\-Site VPN connections, you must recreate each Site\-to\-Site VPN connection for the new virtual private gateway\. If there are multiple AWS Direct Connect private virtual interfaces attached to your virtual private gateway, you must recreate each private virtual interface for the new virtual private gateway\. For more information, see [Creating a virtual interface](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-vif.html) in the *AWS Direct Connect User Guide*\.

If your existing Site\-to\-Site VPN connection is an AWS VPN connection, you cannot migrate to an AWS Classic VPN connection\.

**Topics**
+ [Option 1: Migrate directly to a new virtual private gateway](#aws-vpn-migrate-vgw)
+ [Option 2: Migrate using a transit gateway](#aws-vpn-migrate-tgw)
+ [Option 3: \(Backup VPN connections for AWS Direct Connect\) Delete and recreate the VPN connection](#aws-vpn-migrate-dx)

## Option 1: Migrate directly to a new virtual private gateway<a name="aws-vpn-migrate-vgw"></a>

In this option, you create a new virtual private gateway and Site\-to\-Site VPN connection, detach the old virtual private gateway from your VPC, and attach the new virtual private gateway to your VPC\.

**Note**  
During this procedure, connectivity over the current Site\-to\-Site VPN connection is interrupted when you disable route propagation and detach the old virtual private gateway from your VPC\. Connectivity is restored when the new virtual private gateway is attached to your VPC and the new Site\-to\-Site VPN connection is active\. Ensure that you plan for the expected downtime\.

**To migrate to an AWS VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Virtual Private Gateways**, **Create Virtual Private Gateway** and create a virtual private gateway\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN Connection**\. Specify the following information, and choose **Yes, Create**\.
   + **Virtual Private Gateway**: Select the virtual private gateway that you created in the previous step\.
   + **Customer Gateway**: Choose **Existing**, and select the existing customer gateway for your current AWS Classic VPN connection\.
   + Specify the routing options as required\.

1. Select the new Site\-to\-Site VPN connection and choose **Download Configuration**\. Download the appropriate configuration file for your customer gateway device\.

1. Use the configuration file to configure VPN tunnels on your customer gateway device\. For more information, see [Your customer gateway device](your-cgw.md)\. Do not enable the tunnels yet\. Contact your vendor if you need guidance on keeping the newly configured tunnels disabled\. 

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

## Option 2: Migrate using a transit gateway<a name="aws-vpn-migrate-tgw"></a>

In this option, you create a transit gateway, attach it to the VPC in which your Site\-to\-Site VPN connection resides, and create a temporary Site\-to\-Site VPN connection on the transit gateway using your existing customer gateway\. You then route the traffic through the transit gateway VPN connection while you configure a new Site\-to\-Site VPN connection on a new virtual private gateway\.

Alternatively, you can use this option to migrate your Site\-to\-Site VPN connection directly to a transit gateway\. In this case, you create your new VPN connection on the transit gateway instead of creating it on a new virtual private gateway\.

### Step 1: Create a transit gateway and VPN connection<a name="aws-vpn-migrate-tgw-step-1"></a>

**To create a transit gateway and VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateways**, **Create Transit Gateway** and create a transit gateway using the default options\.

1. In the navigation pane, choose **Transit Gateway Attachments**, **Create Transit Gateway Attachment**\. Specify the following information, and choose **Create attachment**\.
   + For **Transit Gateway ID**, choose the transit gateway you created\. 
   + For **VPC ID**, choose the VPC to attach to the transit gateway\.

1. Choose **Create Transit Gateway Attachment** again, specify the following information, and choose **Create attachment**\.
   + For **Transit Gateway ID**, choose the transit gateway you created\.
   + For **Attachment type**, choose **VPN**\. 
   + For **Customer Gateway ID**, choose the customer gateway for your existing Site\-to\-Site VPN connection, and choose the required routing option\.

### Step 2: Create a new virtual private gateway<a name="aws-vpn-migrate-tgw-step-2"></a>

Create a new virtual private gateway and new Site\-to\-Site VPN connection\. This step is only required if you want to migrate to a new virtual private gateway\. If you want to migrate your VPN connection to a transit gateway, you can skip these steps and go directly to [Step 3](#aws-vpn-migrate-tgw-step-3)\.

**To create a new Site\-to\-Site VPN connection**

1. In the navigation pane, choose **Virtual Private Gateways**, **Create Virtual Private Gateway** and create a new virtual private gateway\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, **Create VPN Connection**\.

1. For **Virtual Private Gateway**, choose the virtual private gateway you created\.

1. For **Customer Gateway ID**, choose the existing customer gateway for your existing Site\-to\-Site VPN connection, and specify the type of routing\. Choose **Create VPN Connection**\.

1. Select your new Site\-to\-Site VPN connection and choose **Download Configuration** to download the example configuration file\. Configure the VPN connection on your customer gateway device, but do not route any traffic yet \(do not create any static routes or filter out BGP announcements\)\.

### Step 3: Switch to the new VPN connection<a name="aws-vpn-migrate-tgw-step-3"></a>

During this procedure, you'll temporarily enable asymmetric routing for your VPN traffic when you switch traffic to the transit gateway and then to the new Site\-to\-Site VPN connection\.

**To switch to the new Site\-to\-Site VPN connection**

1. Configure your customer gateway device to use the VPN connection on the transit gateway \(specify a static route or allow BGP announcements, as needed\)\. This starts asymmetric traffic routing\.

1. In the navigation pane, choose **Route Tables**, select the route table for your VPC, and choose **Actions**, **Edit routes**\.

1. Add routes that point to your on\-premises network and choose the transit gateway as the target\. For the destination routes, enter more specific routes, for example, if your on\-premises network is `10.0.0.0/16`, create a route that points to `10.0.0.0/17` and another route that points to `10.0.128.0/17`\. Asymmetric traffic routing stops and all traffic is routed through the transit gateway\.
**Note**  
If you're migrating your VPN connection to a transit gateway instead of a new virtual private gateway, you can stop here\.

1. In the navigation pane, choose **Virtual Private Gateways**\.

1. Select the old virtual private gateway that's attached to your VPC, and choose **Actions**, **Detach from VPC**\. Choose **Yes, Detach**\.

1. Select the new virtual private gateway that you created earlier, and choose **Actions**, **Attach to VPC**\. Choose your VPC, and choose **Yes, Attach**\.

1. In the navigation pane, choose **Route Tables**\. Select the route table for your VPC, and choose **Route Propagation**, **Edit route propagation**\. Choose the check box for the new virtual private gateway and choose **Save**\. Verify that the route is propagated to your VPC route table\.

1. Configure your customer gateway device to use the new virtual private gateway and route traffic from your on\-premise network to your VPC, using static routes or BGP\. This starts asymmetric routing\. 

1. In the navigation pane, choose **Route Tables**\. Select the route table for your VPC, and choose **Actions**, **Edit routes**\. Delete the more specific routes to your transit gateway\. This stops the asymmetric traffic flow and all traffic is routed through your new Site\-to\-Site VPN connection\.

### Step 4: Clean up<a name="aws-vpn-migrate-tgw-step-4"></a>

If you no longer need your AWS Classic VPN connection, you can delete it\. If you migrated to a new virtual private gateway, you can also delete the transit gateway VPN connection and the transit gateway that you created for the migration\.

**To clean up your resources**

1. On your customer gateway device, remove the configuration for the temporary VPN connection on the transit gateway, and the configuration for the old VPN connection\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, select your old Site\-to\-Site VPN connection, and choose **Actions**, **Delete**\.

1. In the navigation pane, choose **Virtual Private Gateways**, select your old virtual private gateway, and choose **Actions**, **Delete Virtual Private Gateway**\. If you migrated your VPN connection to a transit gateway, you can stop here\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections** and select the transit gateway VPN connection\. Choose **Actions**, **Delete**\.

1. In the navigation pane, choose **Transit Gateway Attachments**, and select the VPC attachment\. Choose **Actions**, **Delete**\.

1. In the navigation pane, choose **Transit Gateways** and select your transit gateway\. Choose **Actions**, **Delete**\.

## Option 3: \(Backup VPN connections for AWS Direct Connect\) Delete and recreate the VPN connection<a name="aws-vpn-migrate-dx"></a>

Use this option if you have an AWS Direct Connect connection and an AWS Classic VPN connection on the same virtual private gateway, and you use the VPN connection as a backup for the AWS Direct Connect connection\. In this option, you delete the existing AWS Classic VPN connections on your virtual private gateway\. When the AWS Classic VPN connections are in the `deleted` state, you can then migrate to an AWS VPN connection by creating a new VPN connection on the same virtual private gateway\. You do not have to make any changes to your existing AWS Direct Connect private virtual interface\.

**Important**  
During this procedure, connectivity over your AWS Direct Connect private virtual interface is not interrupted, but you will not have any connectivity over the Site\-to\-Site VPN connection \(zero downtime with loss of redundancy\)\. Connectivity through the VPN connection is restored when VPN connections are recreated on the virtual private gateway\. Ensure that you plan for this loss of redundancy\.  
After you've deleted the AWS Classic VPN connection, you cannot revert or migrate your new AWS VPN connection back to an AWS Classic VPN connection\.

**To migrate to an AWS VPN connection**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**, and choose the AWS Classic VPN connection\. Choose **Actions**, **Delete**\.

1. Remove the previous tunnel configurations from your customer gateway device\.

1. Repeat the previous two steps until you've deleted all the existing AWS Classic VPN connections for the virtual private gateway\. Wait for the VPN connections to enter the `deleted` state\.

1. Choose **Create VPN Connection**\. Specify the following information, and choose **Create VPN Connection**\.
   + **Virtual Private Gateway**: Choose the virtual private gateway that you used for the AWS Classic VPN connection\.
   + **Customer Gateway**: Choose **Existing**, and select the existing customer gateway for your current AWS Classic VPN connection\.
   + Specify the routing options as required\.

1. Select the new Site\-to\-Site VPN connection and choose **Download Configuration**\. Download the appropriate configuration file for your customer gateway device\.

1. Use the configuration file to configure VPN tunnels on your customer gateway device\. For more information, see [Your customer gateway device](your-cgw.md)\.

1. Enable the new tunnels on your customer gateway device\. To bring the tunnels up, you must initiate the connection from your local network\.

If applicable, check your route tables to ensure that the routes are being propagated\. The routes propagate to the route table when the status of the VPN tunnel is `UP`\.