Load Balancers are servers that route traffic to any one from the  multiple servers that are attached.

Load balancers expose single point of access to applications.

They seperate public traffic from private , provides SSL (HTTPS) for websites.

Ensures high availability , handles failures.

## OSI Model

When ever request from the client transfers to servers the request has to pass through 7 layers.
Different Load balancers act on different layers

` Layer 7 : Application Layer` When request was send from client it is 1st layer it has to pass through where client sends what protocol is used to talk to server 
   like http, https, ftp,smtp etc.
   
  ` Layer 6: Presentation Layer`  Used to encrypt the request

  `Layer 5: Seesion Layer` Used to store the sessions of the request means time at which request made , client details etc.

  
  `Layer 4: Transport Layer` Used to split the request into parts to reduce network over head.

  `Layer 3: Network Layer` Small parts travel from user to server through multiple routers and reach server.

  `Layer 2: Data Link Layer` 

  `Layer 1: Physical Layer`
  

## ALB 
If we want to use Load balancer at Layer 7 then we use ALB.

Application Load Balancer is AWS managed load balancer. AWS takes care of upgrades , maintenance , avalability.

It has integration with many AWS services. Using health Checks ALB will route traffic to only the healthy nodes.

ALB is used if want to route based on 

` 1. paths (amazon.com/search  & amazon.com/cart) `

 `2. hosts  (cloud.amazon.com & shop.amazon.com)`
 
 ` 3. Query (amazon.com?cart=20)`

    4. Ratio Based Routing
    
 ALB is a great fit to container based application , has a port mapping feature to route to specific port, we need  <b>multiple classic Load balancers</b>  per application.

Even if we send plain  http request ALB can send secure request.

It is costly as it has many additional capabilities like it can use one of many load balancing algos and analyzing  the request etc.

It is also slow as it is intercepting the request from user and analying to which target group / instances it has to forward .   

 ## Target Groups

  Target groups can be EC2 instances , Lambda functions , ECS tasks

