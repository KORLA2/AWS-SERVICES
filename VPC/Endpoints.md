When our private subnet want to communicate with other AWS services we attach NAT gate way to give internet connection to the private subnet. Instead of providing a internet connection to private subnet as both are AWS services , AWS provides a better way to connect privately rather than providing internet to private subnet.

VPC Endpoints are used to privately communicate to other AWS services in the same region without internet, end points are secure as instead of allowing traffic to flow from public internet, traffic flows privately with in AWS LAN connections.

Using the endpoints the traffic always remains within AWS doesn't flow through internet.

There are 2 types of endpoints:

GateWay endpoints

Interface endpoints

![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/4e8bf0ec-5092-4888-a4a6-fde8af8b33e1)


Through endpoints we can connect our VPC to other AWS service and traffic stays with in AWS and is private.

Gate Way end points are only for S3 and DynamoDB.

Creating Gate way end points are easy and fast.

Create gateway endpoint for S3 and mention the VPC and subnet where private server is present.

AWS automatically adds a route to gateway endpoint which is connected to S3 in private subnet router , so when ever request was made to S3 in private server using routes present in route table router knows where to forward the request.

Simple .

Interface Endpoints can be used to connect to other AWS services or to other VPC in the same region privately .


![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/411dbc6e-2416-49e0-b2ee-dee58ff0129d)


## Creating Interface Endpoints


Here I created S3 interface endpoint and placed in private subnet and added a security group to endpoint that allows traffic from the private subnet.

Gave the ec2 in the private subnet a S3 read access IAM role.

Enabled the DNS host names and resolution in the VPC because DNS of the target is resolved to the private IP of the interface endpoint.( will be cleared in a bit.)


![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/9d69f958-6c10-4f30-b097-709088ac50db)



VPC interface endpoint when created will create an ENI per subnet in the VPC . It will also provide you a DNS name per each AZ and a global name that you can use within your applications. I created interface endpoint in one subnet.


![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/f0e98b77-4939-4c02-a0a8-c8e9da15060b)



In addition it supports the ability to have the AWS service domain name for the VPC interface endpoint be resolvable to the private IPs of the ENI and ENI send to endpoint. As long as your VPC has DNS enabled it will first check the VPC private DNS resolver and then resolve it to the private IP rather than the public one.

![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/be589fc0-3cb0-4d42-869a-a7fca60a5f1a)

So we have learned how the interface endpoint works on high level. Lets test wheather the private server can talk to S3 .

Instead of creating a server in public subnet and ssh to public and then to private, I have created an endpoint to ec2 instance connect endpoint so that without creating public server we can use AWS connect yo ssh to it.

![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/625b75c5-5a67-4736-aca6-1255f39b37e1)

![image](https://github.com/KORLA2/AWS-SERVICES/assets/96729391/39e1eaf9-fa71-4bac-afc5-4c33e52cb67b)

So while testing we have to mention the endpoint url which is DNS name of the end point in the subnet ( which is after https://bucket. ) if not mentioned ec2 treates the traffic has to flow according to routes in route table.

