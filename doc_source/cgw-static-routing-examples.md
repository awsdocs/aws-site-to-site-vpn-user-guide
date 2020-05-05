# Example customer gateway device configurations for static routing<a name="cgw-static-routing-examples"></a>

**Topics**
+ [Example configuration files](#cgw-static-routing-example-files)
+ [User interface procedures for static routing](#cgw-static-routing-example-interface)
+ [Additional information for Cisco devices](#cgw-static-routing-examples-cisco)
+ [Testing](#cgw-static-routing-example-testing)

## Example configuration files<a name="cgw-static-routing-example-files"></a>

You can download [static\-routing\-examples\.zip](samples/static-routing-examples.zip) to view example configuration files for the following customer gateway devices:
+ Cisco ASA running Cisco ASA 8\.2\+
+ Cisco ASA running Cisco ASA 9\.7\.1\+
+ Cisco IOS running Cisco IOS
+ Cisco Meraki MX Series running 9\.0\+
+ Citrix Netscaler CloudBridge running NS 11\+
+ Cyberoam CR15iNG running V 10\.6\.5 MR\-1
+ F5 Networks BIG\-IP running v12\.0\.0\+
+ Fortinet Fortigate 40\+ Series running FortiOS 4\.0\+
+ Generic configuration for static routing
+ H3C MSR800 running version 5\.20
+ IIJ SEIL/B1 running SEIL/B1 3\.70\+
+ Mikrotik RouterOS running 6\.36
+ Openswan running 2\.6\.38\+
+ pfSense running OS 2\.2\.5\+
+ SonicWALLrunning SonicOS 5\.9 or 6\.2
+ Strongswan Ubuntu 16\.04 running Strongswan 5\.5\.1\+
+ WatchGuard XTM, Firebox running Fireware OS 11\.11\.4
+ Zyxel Zywall running Zywall 4\.20\+

The files use placeholder values for some components\. For example, they use:
+ Example values for the VPN connection ID and virtual private gateway ID
+ Placeholders for the remote \(outside\) IP address AWS endpoints \(*AWS\_ENDPOINT\_1* and *AWS\_ENDPOINT\_2*\)
+ A placeholder for the IP address for the internet\-routable external interface on the customer gateway device \(*your\-cgw\-ip\-address*\)
+ Example values for the tunnel inside IP addresses\.

In addition to providing placeholder values, the files specify the minimum requirements of IKE version 1, AES128, SHA1, and DH Group 2 in most AWS Regions\. They also specify pre\-shared keys for [authentication](vpn-tunnel-authentication-options.md)\. You must modify the example configuration files to take advantage of IKE version 2, AES256, SHA256, other DH groups such as 2, 14\-18, 22, 23, and 24, and private certificates\. 

The following diagram provides an overview of the different components that are configured on the customer gateway device\. It includes example values for the tunnel interface IP addresses\.

![\[Customer gateway device with static routing\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/cgw-static-routing.png)

To download a configuration file with values that are specific to your VPN connection configuration, use the Amazon VPC console\. For more information, see [Download the configuration file](SetUpVPNConnections.md#vpn-download-config)\.

## User interface procedures for static routing<a name="cgw-static-routing-example-interface"></a>

The following are some example procedures for configuring a customer gateway device using its user interface \(if available\)\.

------
#### [ Check Point ]

The following are steps for configuring your customer gateway device if your device is a Check Point Security Gateway device running R77\.10 or above, using the Gaia operating system and Check Point SmartDashboard\. You can also refer to the [Check Point Security Gateway IPsec VPN to Amazon Web Services VPC](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk100726) article on the Check Point Support Center\.

**To configure the tunnel interface**

The first step is to create the VPN tunnels and provide the private \(inside\) IP addresses of the customer gateway and virtual private gateway for each tunnel\. To create the first tunnel, use the information provided under the `IPSec Tunnel #1` section of the configuration file\. To create the second tunnel, use the values provided in the `IPSec Tunnel #2` section of the configuration file\. 

1. Open the Gaia portal of your Check Point Security Gateway device\.

1. Choose **Network Interfaces**, **Add**, **VPN tunnel**\.

1. In the dialog box, configure the settings as follows, and choose **OK** when you are done:
   + For **VPN Tunnel ID**, enter any unique value, such as 1\.
   + For **Peer**, enter a unique name for your tunnel, such as `AWS_VPC_Tunnel_1` or `AWS_VPC_Tunnel_2`\.
   + Ensure that **Numbered** is selected, and for **Local Address**, enter the IP address specified for `CGW Tunnel IP` in the configuration file, for example, `169.254.44.234`\. 
   + For **Remote Address**, enter the IP address specified for `VGW Tunnel IP` in the configuration file, for example, `169.254.44.233`\.  
![\[Check Point Add VPN Tunnel dialog box\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/check-point-create-tunnel.png)

1. Connect to your security gateway over SSH\. If you're using the non\-default shell, change to clish by running the following command: `clish`

1. For tunnel 1, run the following command\.

   ```
   set interface vpnt1 mtu 1436
   ```

   For tunnel 2, run the following command\.

   ```
   set interface vpnt2 mtu 1436
   ```

1. Repeat these steps to create a second tunnel, using the information under the `IPSec Tunnel #2` section of the configuration file\.

**To configure the static routes**

In this step, specify the static route to the subnet in the VPC for each tunnel to enable you to send traffic over the tunnel interfaces\. The second tunnel enables failover in case there is an issue with the first tunnel\. If an issue is detected, the policy\-based static route is removed from the routing table, and the second route is activated\. You must also enable the Check Point gateway to ping the other end of the tunnel to check if the tunnel is up\.

1. In the Gaia portal, choose **IPv4 Static Routes**, **Add**\.

1. Specify the CIDR of your subnet, for example, `10.28.13.0/24`\.

1. Choose **Add Gateway**, **IP Address**\.

1. Enter the IP address specified for `VGW Tunnel IP` in the configuration file \(for example, `169.254.44.233`\), and specify a priority of 1\.

1. Select **Ping**\.

1. Repeat steps 3 and 4 for the second tunnel, using the `VGW Tunnel IP` value under the `IPSec Tunnel #2` section of the configuration file\. Specify a priority of 2\.  
![\[Check Point Edit Destination Route dialog box\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/check-point-static-routes.png)

1. Choose **Save**\.

If you're using a cluster, repeat the preceding steps for the other members of the cluster\. 

**To define a new network object**

In this step, you create a network object for each VPN tunnel, specifying the public \(outside\) IP addresses for the virtual private gateway\. You later add these network objects as satellite gateways for your VPN community\. You also need to create an empty group to act as a placeholder for the VPN domain\. 

1. Open the Check Point SmartDashboard\.

1. For **Groups**, open the context menu and choose **Groups**, **Simple Group**\. You can use the same group for each network object\.

1. For **Network Objects**, open the context \(right\-click\) menu and choose **New**, **Interoperable Device**\.

1. For **Name**, enter the name that you provided for your tunnel, for example, `AWS_VPC_Tunnel_1` or `AWS_VPC_Tunnel_2`\.

1. For **IPv4 Address**, enter the outside IP address of the virtual private gateway provided in the configuration file, for example, `54.84.169.196`\. Save your settings and close the dialog box\.  
![\[Check Point Interoperable Device dialog box\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/check-point-network-device.png)

1. In the SmartDashboard, open your gateway properties and in the category pane, choose **Topology**\. 

1. To retrieve the interface configuration, choose **Get Topology**\.

1. In the **VPN Domain** section, choose **Manually defined**, and then browse to and select the empty simple group that you created in step 2\. Choose **OK**\.
**Note**  
You can keep any existing VPN domain that you've configured\. However, ensure that the hosts and networks that are used or served by the new VPN connection are not declared in that VPN domain, especially if the VPN domain is automatically derived\.

1. Repeat these steps to create a second network object, using the information under the `IPSec Tunnel #2` section of the configuration file\.

**Note**  
If you're using clusters, edit the topology and define the interfaces as cluster interfaces\. Use the IP addresses that are specified in the configuration file\. 

**To create and configure the VPN community, IKE, and IPsec settings**

In this step, you create a VPN community on your Check Point gateway, to which you add the network objects \(interoperable devices\) for each tunnel\. You also configure the Internet Key Exchange \(IKE\) and IPsec settings\.

1. From your gateway properties, choose **IPSec VPN** in the category pane\.

1. Choose **Communities**, **New**, **Star Community**\.

1. Provide a name for your community \(for example, `AWS_VPN_Star`\), and then choose **Center Gateways** in the category pane\.

1. Choose **Add**, and add your gateway or cluster to the list of participant gateways\.

1. In the category pane, choose **Satellite Gateways**, **Add**, and then add the interoperable devices that you created earlier \(`AWS_VPC_Tunnel_1` and `AWS_VPC_Tunnel_2`\) to the list of participant gateways\.

1. In the category pane, choose **Encryption**\. In the **Encryption Method** section, choose **IKEv1 only**\. In the **Encryption Suite** section, choose **Custom**, **Custom Encryption**\.

1. In the dialog box, configure the encryption properties as follows, and choose **OK** when you're done:
   + IKE Security Association \(Phase 1\) Properties:
     + **Perform key exchange encryption with**: AES\-128
     + **Perform data integrity with**: SHA\-1
   + IPsec Security Association \(Phase 2\) Properties:
     + **Perform IPsec data encryption with**: AES\-128
     + **Perform data integrity with**: SHA\-1

1. In the category pane, choose **Tunnel Management**\. Choose **Set Permanent Tunnels**, **On all tunnels in the community**\. In the **VPN Tunnel Sharing** section, choose **One VPN tunnel per Gateway pair**\.

1. In the category pane, expand **Advanced Settings**, and choose **Shared Secret**\.

1. Select the peer name for the first tunnel, choose **Edit**, and then enter the pre\-shared key as specified in the configuration file in the `IPSec Tunnel #1` section\.

1. Select the peer name for the second tunnel, choose **Edit**, and then enter the pre\-shared key as specified in the configuration file in the `IPSec Tunnel #2` section\.  
![\[Check Point Interoperable Shared Secret dialog box\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/check-point-shared-secret.png)

1. Still in the **Advanced Settings** category, choose **Advanced VPN Properties**, configure the properties as follows, and then choose **OK** when you're done:
   + IKE \(Phase 1\):
     + **Use Diffie\-Hellman group**: `Group 2`
     + **Renegotiate IKE security associations every** `480` **minutes**
   + IPsec \(Phase 2\):
     + Choose **Use Perfect Forward Secrecy**
     + **Use Diffie\-Hellman group**: `Group 2`
     + **Renegotiate IPsec security associations every** `3600` **seconds**

**To create firewall rules**

In this step, you configure a policy with firewall rules and directional match rules that allow communication between the VPC and the local network\. You then install the policy on your gateway\.

1. In the SmartDashboard, choose **Global Properties** for your gateway\. In the category pane, expand **VPN**, and choose **Advanced**\.

1. Choose **Enable VPN Directional Match in VPN Column**, and save your changes\.

1. In the SmartDashboard, choose **Firewall**, and create a policy with the following rules: 
   + Allow the VPC subnet to communicate with the local network over the required protocols\. 
   + Allow the local network to communicate with the VPC subnet over the required protocols\.

1. Open the context menu for the cell in the VPN column, and choose **Edit Cell**\. 

1. In the **VPN Match Conditions** dialog box, choose **Match traffic in this direction only**\. Create the following directional match rules by choosing **Add** for each, and choose **OK** when you're done:
   + `internal_clear` > VPN community \(The VPN star community that you created earlier, for example, `AWS_VPN_Star`\)
   + VPN community > VPN community
   + VPN community > `internal_clear`

1. In the SmartDashboard, choose **Policy**, **Install**\. 

1. In the dialog box, choose your gateway and choose **OK** to install the policy\.

**To modify the tunnel\_keepalive\_method property**

Your Check Point gateway can use Dead Peer Detection \(DPD\) to identify when an IKE association is down\. To configure DPD for a permanent tunnel, the permanent tunnel must be configured in the AWS VPN community \(refer to Step 8\)\.

By default, the `tunnel_keepalive_method` property for a VPN gateway is set to `tunnel_test`\. You must change the value to `dpd`\. Each VPN gateway in the VPN community that requires DPD monitoring must be configured with the `tunnel_keepalive_method` property, including any 3rd party VPN gateway\. You cannot configure different monitoring mechanisms for the same gateway\.

You can update the `tunnel_keepalive_method` property using the GuiDBedit tool\.

1. Open the Check Point SmartDashboard, and choose **Security Management Server**, **Domain Management Server**\.

1. Choose **File**, **Database Revision Control\.\.\.** and create a revision snapshot\.

1. Close all SmartConsole windows, such as the SmartDashboard, SmartView Tracker, and SmartView Monitor\.

1. Start the GuiBDedit tool\. For more information, see the [Check Point Database Tool](http://supportcontent.checkpoint.com/solutions?id=sk13009) article on the Check Point Support Center\. 

1. Choose **Security Management Server**, **Domain Management Server**\.

1. In the upper left pane, choose **Table**, **Network Objects**, **network\_objects**\. 

1. In the upper right pane, select the relevant **Security Gateway**, **Cluster** object\. 

1. Press CTRL\+F, or use the **Search** menu to search for the following: `tunnel_keepalive_method`\.

1. In the lower pane, open the context menu for `tunnel_keepalive_method`, and choose **Edit\.\.\.**\. Choose **dpd** and then choose **OK**\.

1. Repeat steps 7 through 9 for each gateway that's part of the AWS VPN Community\.

1. Choose **File**, **Save All**\.

1. Close the GuiDBedit tool\.

1. Open the Check Point SmartDashboard, and choose **Security Management Server**, **Domain Management Server**\.

1. Install the policy on the relevant **Security Gateway**, **Cluster** object\.

For more information, see the [New VPN features in R77\.10](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk97746) article on the Check Point Support Center\.

**To enable TCP MSS clamping**

TCP MSS clamping reduces the maximum segment size of TCP packets to prevent packet fragmentation\.

1. Navigate to the following directory: `C:\Program Files (x86)\CheckPoint\SmartConsole\R77.10\PROGRAM\`\.

1. Open the Check Point Database Tool by running the `GuiDBEdit.exe` file\.

1. Choose **Table**, **Global Properties**, **properties**\.

1. For `fw_clamp_tcp_mss`, choose **Edit**\. Change the value to `true` and choose **OK**\.

**To verify the tunnel status**  
You can verify the tunnel status by running the following command from the command line tool in expert mode\.

```
vpn tunnelutil
```

In the options that display, choose **1** to verify the IKE associations and **2** to verify the IPsec associations\.

You can also use the Check Point Smart Tracker Log to verify that packets over the connection are being encrypted\. For example, the following log indicates that a packet to the VPC was sent over tunnel 1 and was encrypted\.

![\[Check Point log file\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/check-point-log.png)

------
#### [ SonicWALL ]

The following procedure demonstrates how to configure the VPN tunnels on the SonicWALL device using the SonicOS management interface\.

**To configure the tunnels**

1. Open the SonicWALL SonicOS management interface\. 

1. In the left pane, choose **VPN**, **Settings**\. Under **VPN Policies**, choose **Add\.\.\.**\.

1. In the VPN policy window on the **General ** tab, complete the following information:
   + **Policy Type**: Choose **Tunnel Interface**\.
   + **Authentication Method**: Choose **IKE using Preshared Secret**\.
   + **Name**: Enter a name for the VPN policy\. We recommend that you use the name of the VPN ID, as provided in the configuration file\.
   + **IPsec Primary Gateway Name or Address**: Enter the IP address of the virtual private gateway \(AWS endpoint\) as provided in the configuration file \(for example, `72.21.209.193`\)\.
   + **IPsec Secondary Gateway Name or Address**: Leave the default value\.
   + **Shared Secret**: Enter the pre\-shared key as provided in the configuration file, and enter it again in **Confirm Shared Secret**\.
   + **Local IKE ID**: Enter the IPv4 address of the customer gateway \(the SonicWALL device\)\. 
   + **Peer IKE ID**: Enter the IPv4 address of the virtual private gateway \(AWS endpoint\)\.

1. On the **Network** tab, complete the following information:
   + Under **Local Networks**, choose **Any address**\. We recommend this option to prevent connectivity issues from your local network\. 
   + Under **Remote Networks**, choose **Choose a destination network from list**\. Create an address object with the CIDR of your VPC in AWS\.

1. On the **Proposals** tab, complete the following information: 
   + Under **IKE \(Phase 1\) Proposal**, do the following:
     + **Exchange**: Choose **Main Mode**\.
     + **DH Group**: Enter a value for the Diffie\-Hellman group \(for example, `2`\)\. 
     + **Encryption**: Choose **AES\-128** or **AES\-256**\.
     + **Authentication**: Choose **SHA1** or **SHA256**\.
     + **Life Time**: Enter `28800`\.
   + Under **IKE \(Phase 2\) Proposal**, do the following:
     + **Protocol**: Choose **ESP**\.
     + **Encryption**: Choose **AES\-128** or **AES\-256**\.
     + **Authentication**: Choose **SHA1** or **SHA256**\.
     + Select the **Enable Perfect Forward Secrecy** check box, and choose the Diffie\-Hellman group\.
     + **Life Time**: Enter `3600`\.
**Important**  
If you created your virtual private gateway before October 2015, you must specify Diffie\-Hellman group 2, AES\-128, and SHA1 for both phases\.

1. On the **Advanced** tab, complete the following information:
   + Select **Enable Keep Alive**\.
   + Select **Enable Phase2 Dead Peer Detection** and enter the following:
     + For **Dead Peer Detection Interval**, enter `60` \(this is the minimum that the SonicWALL device accepts\)\.
     + For **Failure Trigger Level**, enter `3`\.
   + For **VPN Policy bound to**, select **Interface X1**\. This is the interface that's typically designated for public IP addresses\.

1. Choose **OK**\. On the **Settings** page, the **Enable** check box for the tunnel should be selected by default\. A green dot indicates that the tunnel is up\.

------

## Additional information for Cisco devices<a name="cgw-static-routing-examples-cisco"></a>

Some Cisco ASAs only support Active/Standby mode\. When you use these Cisco ASAs, you can have only one active tunnel at a time\. The other standby tunnel becomes active if the first tunnel becomes unavailable\. With this redundancy, you should always have connectivity to your VPC through one of the tunnels\. 

Cisco ASAs from version 9\.7\.1 and later support Active/Active mode\. When you use these Cisco ASAs, you can have both tunnels active at the same time\. With this redundancy, you should always have connectivity to your VPC through one of the tunnels\.

For Cisco devices, you must do the following:
+ Configure the outside interface\.
+ Ensure that the Crypto ISAKMP Policy Sequence number is unique\.
+ Ensure that the Crypto List Policy Sequence number is unique\.
+ Ensure that the Crypto IPsec Transform Set and the Crypto ISAKMP Policy Sequence are harmonious with any other IPsec tunnels that are configured on the device\.
+ Ensure that the SLA monitoring number is unique\.
+ Configure all internal routing that moves traffic between the customer gateway device and your local network\.

## Testing<a name="cgw-static-routing-example-testing"></a>

For more information about testing your Site\-to\-Site VPN connection, see [Testing the Site\-to\-Site VPN connection](HowToTestEndToEnd_Linux.md)\.