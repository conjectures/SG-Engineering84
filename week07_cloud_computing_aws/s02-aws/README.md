
# AWS

## What is AWS
AWS is the larger cloud computing provider with servers in every continent. AWS are trusted by large companies like Netflix, BBC, Facebook and Twitch, which accentuates their reliability and range of services.
The benefits of AWS over its competitors is that their services are efficient, competitive and reliable. 
Security group works in the instance level


## Getting started with AWS
To get started with AWS we need to either create new user, or acquire an IAM access role to work in an organisation's account.

## EC2 - Elastic Compute Cloud
Elastic Compute Cloud is an AWS service that provides secure, resizable compute capacity in the cloud. They are virtual machines hosted on the cloud, which means that we can choose an operating system, CPU capabilies, memory capacity and other settings, according to our needs.
EC2 instances are charged according to the time they spend on *Running* state, and have a mutliplier applied according to their CPU and RAM capabiliy 'level'. 

## AWS Regions, Edge Locations and Availability Zones
- **Region**: 

A **Regions** are the physical locations where clusters of AWS data centers exist. 
By choosing a region for our application, we can choose to have our application be geographically close to our users, so that the connection latency can be reduced.

- **Availability Zone**

**Availability Zones** correspond to discrete data centers with redundant power, networking and connectivity in an AWS Region. We can choose to host our application in one or more *Availability Zones* in order to have our application work in case there is an outage in a specific datacenter.

- **Edge Location**

**Edge Locations** refer to locations from where the global *CloudFront* network can provide its service. The *CloudFront* service is a **Content Delivery Network** (**CDN**) that focuses on *'caching'* responses from our website so that repeated requests are not handled by our servers.
Along with other security features, like DDOS attack protection, the CDN provided by AWS is a useful feature to increase our application availability.

## VPC, Subnets and Security Groups
One of the services provided by AWS is the use of Virtual Private Networks in the cloud, named **Virtual Private Cloud** (**VPC**).
With the help of these networks, we can provide an added layer of security to our application and create multi-tiered applications with services that talk with eachothers in an internal network.

### VPC
A **Virtual Private Cloud** (**VPC**) is, in essence a *Virtual Private Network* on the cloud. That means, it has network interface to connect to the internet, and any other applications need to connect through it to reach the internet. 
This interface is called the **Internet Gateway** and we need to create it in AWS if we want to enabel internet access to our network.

### Subnet
Inside the VPC, we can create **Subnets** which are, again, Virtual Private Networks, although they are within our initial VPC.
As they are within our initial VPC, the access to these subnets is as restrictive as the VPC rules. However, we can add more restrictive rules if we want more protection for sensitive services, like Databases. 
In order to allow communication between subnets, we can create a **Routing Table** . The routing table can be used to let a subnet know where to find the other resouces in the VPC. They can also be used to tell the  subnet where to find the Internet Gateway.

We can restrict access to traffic in subnets with the use of **Network ACL** rules, or we can apply rules specifically for an instance with **Security Groups**.

### Security Groups
Security Groups is a collection of rules that control the inbound and outbound traffic for our instances. Each EC2 instance has at least one security group attached that controls the traffic that is allowed to pass. 

### NACL
**Network ACL** (**NACL**) control inbound and outbound traffic for subnets. They are an added layer of security, on top of *Security Groups*.
As they are applied for an entire subnet, they can be used to set inbound or outbound rules for every instance included in the subnet.
NACL rules are associated with a rule number. This number is used to specify the order with which the rules are evaluated. 
AWS recommends rule numbers to be added with increments of 10, so that if we need to add a specific rule in the future, we can have adequate slots to insert them.

### Stateful vs Stateless Filtering
Stateful filtering allows responses to go back out even if the outbound rules don't allow it. Stateless filtering will not allow for the responce to go through in the same scenario.
Security groups are an example of a stateful filtering method, while NACLs are stateless.

## Routing

### Route Tables
The **Route Tables** containe routes, that help direct traffic outside of the subnet IPs

### Internet Gateway
The **Internet Gateway** is a highliy available VPC component that allows communication between your VPC and the internet.

### Ephemeral Port
An **ephemeral prot** or **dynamic port** is a short-lived port number used by an Internet Protocol (IP) transport protocol. They are allocated automatically from a predefined range of IP stack soaftware.
They can be used as the post assignement on the server end of a communication. Several file transfer file protocols continue communication on ephemeral ports with a client that initially connected to one of the server's well-known service listenintg ports.
Different range of ports were suggested to be dynamic / ephemeral by different organisations or operating systems:
| Organisation / OS | Dynamic Ports Range|
|---|---|
| Internet Assigned Numbers Authority (IANA) | `49152` - `65535`|
| Linux Kernel | `32768`-`60999`|
| BSD | `1024`-`5000`|
| Windows XP | `1025`-`5000`
| Windows 7, Server 2018 |  `49152` - `65535`|

## S3
The **Simple Storage Service** (**S3**) is a storage solution provided by AWS.
It can be used to backup and restore data, disaster recovery, archiving, or even host cloud-native applicatios.
S3 offers a wide range of storage classes for different retrieval and storage frequencies. The classes are priced differently, so it is beneficial to be familiar with them in order to optimise costs:

Additionally, S3 has no Region or Availability Zone requiremetns, as it is globally available.


|S3 Class|Use case|
|---|---|
|Standard| General Purpose|
|Intelligent-Tiering|Unknown or changing access|
|Standard-IA| Infrequent access|
|One Zone-IA| Infrequent, quick access on single availability zone|
|Glacier|Archiving|
|Glacier Deep Archive||
|Outposts||

## Load Balancers
There are three types of load balancers provided by AWS:

### Classic Load Balancer
Makes routing decisions at either the transport layer (TCP) or the application layer (HTTP/HTTPS). 
Classic load balancers require a fixed relationship between the balancer port and the container instance port.

### Application Load Balancer
Application load balancers only apply to the application layer, that is, the HTTP layer.
It is used to load balance to multiple HTTP applications across machines. These machines are grouped into **Target Groups**.
Application Load Balancers can also be used to load balance between multiple applications on the same machine, for example containers.
It has support for HTTP/2 and WebSocket and can support HTTP to HTTPS redirects.
It also has routing tables to different target groups (routing based on path in URL, on hostname or on query strings or headers)
In short, it does the job of multiple Classic Load Balancers per application

ALB are a great fit for micro services and container based applications
It has port mapping freature to redirect to a dynamic port in ECS

The target groups of an ALB can be:
- EC2 instances
- EC2 tasks
- Lambda functions
- IP addresses

ALBs can route to multiple target groups

Additionally, when an Application Load Balancer talks to our instances, it will use its own internal IP, and will have the client's IP in the `X-Forwarded-For` and `X-Forwarded-Port` headers in the HTTP request.
### Network Load Balancer


## Auto Scaling Groups
The load on websites ansd applications will change in real life. For that reason we use **Auto Scaling Groups (ASG)** to scale out (add EC2 instances)  or in (remove EC2 instances) to match an increased or decreased load accordingly.
We can also use an ASG to ensure that we have a minimum, or maximum number of machines running
These new machines are automatically registered to the load balancer.

![ASG with Load Balancer](asg-with-load-balancer.png)

### Auto Scaling Alarms
It is possible to scale ASG based on Cloud Watch alarms:

![Scaling with CloudWatch Alarms](cooldowns-example-scaling-policy-diagram.png)

An alarm monitors a metric, such as average CPU load. And the metrics are computed for all the ASG instances.  
Based on those metrics, we can create scla-out and scale-in policies.

We can also scale based on a **custom metric**, for example, the number of connected users. Therefore, the scaling group alarms are not limited by the metrics provided by AWS CloudWatch.
To do this, we need to send the custom metric from the application on EC2 to CLoudWatch, then use it to create alarms and scaling policy for ASG.

Additionally, ASG can terminate instances marked as unhealthy by a Load Balancer, and then replace them.


