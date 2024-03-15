VPC Endpoints are used  to privately communicate to other aws services in the same region  without internet, instead of using nat gateways end points are secure.

When we want to connect to other AWS services from EC2(or any other service) as both are AWS services instead of allowing traffic to flow from public internet, traffic can also flow privately with in AWS LAN connections. 

Using the endpoints the traffic always remains within AWS doesn't flow through internet.

There are 3 types of endpoints:

1. GateWay endpoints

2. Interface endpoints

   <img width="490" alt="image" src="https://github.com/KORLA2/AWS-SERVICES/assets/96729391/836dd027-539c-464e-8a3d-36961f76c97d">


Basically endpoints connect to  one AWS service , now we have to connect  the service we want to communicate  to , to end points , and traffic stays within AWS.

GateWay end points are only for S3 and DynamoDB.

Creating Gate way end points are easy and fast.

1. Create gateway endpoint for S3 and mention the VPC and subnet where private server is present.

2. AWS automatically adds a route to S3 to private subnet router so that when ever S3 request was made in private server using routes present router knows where to forward 
   the request.  

Simple .

Interface Endpoints can be used to connect to other AWS services or to other VPC in the same region privately.



<img width="301" alt="image" src="https://github.com/KORLA2/AWS-SERVICES/assets/96729391/944baf48-77f7-4940-9547-5685c7aaf7c3">

VPC interface endpoint when created will create an ENI per subnet in the VPC for you. It will also provide you a DNS name per each AZ and a global name that you can use within your applications.

In addition it supports the ability to have the AWS service domain name for the VPC interface endpoint be resolvable to the private IPs of the endpoint. As long as your VPC has DNS enabled it will first check the VPC private DNS resolver and then resolve it to the private IP rather than the public one.



From the AWS side this is just an ENI created in your AWS VPC that is connected to one of AWS internal VPCs. It's actually possible to implement this for your own services too to share with another organisations VPCs, this is implemented using AWS PrivateLink.



  

