# **EC2 Hands On Lab (contd.)**
## **Changing the Instance Type**



Did you know that you can change the instance type that an AMI is running on? This is very useful when you need a larger (or smaller) or perhaps different type of instance to run a workload. This only works with EBS-backed instances (what we’re running here).  There is no particular reason to change the instance type in this lab, but the following steps outline how easy it is to do in AWS

In the AWS Console, select your lab instance, then right-click on it and hover over **Instance State** and select **Stop** (NOT Terminate).  Then select **Yes, Stop** to confirm.

![EC2-22](ec2-22.PNG)

After it has stopped, right-click on it again, hover over **Instance Settings** and select **Change Instance Type**

![EC2-23](ec2-23.PNG)

After going through the options and selecting **t3.large** as the instance type, right-click your lab instance and select **Start**

![EC2-24](ec2-24.PNG)

**_What happens to the public IP address after you start the instance again?_**


