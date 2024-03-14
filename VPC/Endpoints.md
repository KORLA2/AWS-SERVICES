Endpoints are used  to privately communicate to other aws services without internet, instead of using nat gateways end points are secure.

When we want to connect to other AWS services from EC2(or any other service) as both are AWS services instead of allowing traffic to flow from public internet, traffic can also flow privately with in AWS LAN connections. 

Using the endpoints the traffic always remains within AWS doesn't flow through internet.

There are 3 types of endpoints:

1. GateWay endpoints

2. Interface endpoints

   <img width="490" alt="image" src="https://github.com/KORLA2/AWS-SERVICES/assets/96729391/836dd027-539c-464e-8a3d-36961f76c97d">


Basically endpoints connect to  one AWS service , now we have to connect  the service we want to communicate  to , to end points , and traffic stays within AWS.

Gate Way end points are only for S3 and DynamoDB.

1. Create gateway endpoint for S3 and mention the VPC and subnet

