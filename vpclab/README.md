# VPC Hands On Lab
### Getting Started with Virtual Private Cloud

## Virtual Private Cloud (VPC) Overview

Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. You have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways. You can use both IPv4 and IPv6 in your VPC for secure and easy access to resources and applications.

In this lab, you will learn how to set up a VPC with public and private subnets. You will also learn about AWS networking concepts such as Elastic IPs, NAT Gateways, and Flow Logs.

## Navigate to the VPC Dashboard

To get started, let’s take a look at the VPC Dashboard.  In every region, a **default VPC** has already been created for you. So, even if you haven’t created anything in your account yet, you will see some VPC resources already there.

![VPC Dashboard](vpc1.png)

In this lab, we will be using the **VPC Wizard** to create a VPC with private and public subnets pre-configured. Once you’re more familiar with AWS networking, you can create VPCs and subnets without the Wizard to create custom networking configurations

![Warning!](../static/Warning.png)

During the VPC wizard set up, you will need to specify an **Elastic IP Address (EIP)**. An Elastic IP is a static public IPv4 address that you can attach to AWS resources, such as EC2 instances and NAT Gateways. Elastic IPs are required for NAT Gateways.

To create one, go to **Elastic IPs** in the sidebar, and press **Allocate new address, Allocate, Close**.

![EIP](EIP.png)

Now click on **VPC Dashboard** in the top left corner to go back to the main VPC page. 
Click on **Launch VPC Wizard** to start the VPC Wizard and select **‘VPC with Public and Private Subnets’**. 

![VPC Dashboard](vpc2.png)

This option will create a VPC with a /16 CIDR block and two subnets with /24 CIDR blocks which have 256 total IP addresses each. In each subnet, **AWS reserves 5 IP addresses**. In this case, that leaves you 251 IP addresses per subnet. Fill in the **VPC name** **_("e.g. test")_** and select your **Elastic IP Allocation ID** from the drop-down. For this lab, we will leave the rest of the default configuration as is.

![VPC Dashboard](vpc3.png)

This Elastic IP will be used to create a **Network Address Translation (NAT) gateway** for the private subnet. NAT gateway is a managed, highly scalable NAT service that gives your resources access to the internet but doesn’t allow anyone on the internet access to your resources. NAT is helpful for when a resource needs to pull down updates from the internet but should not be publicly accessible.

Click on **Create VPC**. This step will take a couple minutes. Once your VPC has been created, click **OK**. 

## What the VPC Wizard Created
Let’s walk through the VPC Console and explain each component that the wizard created. 

From the last step, you should now be on the **Your VPCs** dashboard looking at all of your VPCs in this region. Select the VPC that you just created, and look at the **Summary** tab. If you can’t see everything in the pane, you can pull the pane up by dragging on the pane’s top line. 

In the **Summary** tab in the left-hand column, you can see the **Main Route Table** for your VPC. Any subnets in the VPC that do not have a route table directly associated with it will use this route table by default. To explore this further, click on the **Route table link**.

![VPC Dashboard](vpc4.png)

You are now in the **Route Tables** dashboard, filtered on the main route table of the VPC you just created. That means that there should be only one route table shown. Select this route table and click on the **Subnet Associations** tab

![VPC Dashboard](vpc5.png)

You can see that there are no **explicit subnet associations** on this route table. However, since this is the main route table for the VPC, there is one subnet implicitly associated. It’s the **Private subnet** of the VPC. 

What makes a subnet public or private? We can find out by looking at the routes in this table. 
Click on the **Routes** tab. 

In the **Routes** tab, look at the **Target** column. You’ll see one **local route** which every route table has. This ensures that resources within the VPC can talk to each other.

You will also see a route to the **NAT gateway** that the wizard created. Remember that a NAT gateway gives internet access to resources which are not publicly accessible. Because the resources in this subnet are not publicly accessible, this is considered a **private subnet**. 

Now let’s find out what makes a subnet public. First, get rid of the filter on this view. To do this, **click the X in the search bar**. You are now looking at all of the route tables in this region. Your dashboard should look something like the screenshot below. 

Select the other route table in your VPC (check **VPC** column for name) which is not the main route table (check **Main** column for **No**). In the **Routes** tab, you’ll see a route to an **Internet Gateway (IGW)**. IGWs are another managed and scalable service like the NAT Gateway except that it allows access from the internet to your resources in the VPC, making your resources publicly accessible. 

![VPC Dashboard](vpc6.png)

There is a reason why this route table is not the main route table. 

**_It is best practice for your VPC’s main route table to not have a route to an IGW so that subnets are private by default and only public if specified._**

Go to the **Subnet Associations** tab and confirm that it is the **Public subnet** which is associated with this route table. Click on the **Public subnet link**.

Look at the **Description** tab. You’ll notice a **Network Access Control List (Network ACL)** link. NACLs are virtual stateless firewalls at the subnet layer. This wizard used the **default NACL** (created automatically with the VPC) for both subnets. Similar to the main route table, the default NACL is implicitly associated with all subnets in a VPC unless another NACL is directly associated with that subnet. 

Go to the **Network ACL** tab to look at the default NACL rules. Rules are evaluated in order from lowest to highest. If the traffic doesn’t match any rules, the * rule is applied, and the traffic is denied. Default NACLs allow all inbound and outbound traffic, as shown below, unless customized.

![VPC Dashboard](vpc7.png)

Allowing all traffic in and out of your subnets is not a good security posture. However, it is possible to achieve good security with this default NACL by leveraging **Security Groups** as well. 

Security Groups are virtual stateful firewalls at the resource (EC2 instance) level. It is best practice to implement necessary firewall rules with Security Groups first and only adding rules to NACLs as necessary. For instance, you can explicitly deny traffic from specific IPs with NACLs but not with Security Groups. We will explore Security Groups more in the next section.

We have now gone through the bread and butter of AWS networking. You should now understand how routing works in a VPC, what makes a subnet public or private, and how to secure your resources at the subnet and resource levels.