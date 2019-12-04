# **Storage on AWS Hands on Labs**

In this lab, we will look at working with Elastic Block Store (EBS) Volumes and Object Stores (S3).

## **Working with Elastic Block Store (EBS) Volumes**

The purpose of this AWS Immersion Day hands-on lab is to familiarize you with the Amazon Elastic Block Store (EBS) service. EBS is a block storage service that enables you to create volumes, and attach / detach them to Elastic Compute Cloud (EC2) instances. You can create EBS volumes with different volume types, such as: Provisioned IOPS (io1), General Purpose Solid State (gp2), Throughput Optimized HDD (st1), or Cold HDD (sc1), depending on the performance characteristics of your application. Once an EBS volume has been created, you can switch between different volume types, using the ModifyVolume API. Keep in mind that EBS volume modifications are limited to once every six (6) hours.

During this lab, you'll create an EBS volume, attach it to an EC2 instance, format and mount the volume, generate some ongoing disk activity, and then modify the volume attributes to increase its performance.

### **Create and attach an EBS Volume**

The first thing you’ll do is create a new Elastic Block Store (EBS) volume. You’ll simply specify the initial size for the volume, and assign it the default General Purpose SSD volume type. Next, you’ll attach the new EBS volume to an EC2 instance.

Navigate to the **[AWS EC2 Console](https://ap-southeast-2.console.aws.amazon.com/ec2/home?region=ap-southeast-2)** and launch an Ubuntu EC2 instance

![ST1](st1.png)

Note the **InstanceID** and **Availability Zone** of your EC2 instance; you'll need this in a moment
