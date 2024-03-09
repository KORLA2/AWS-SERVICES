Load Balancers are servers that route traffic to any one from the  multiple servers that are attached.

Load balancers expose single point of access to applications.

They seperate public traffic from private , provides SSL (HTTPS) for websites.

Ensures high availability , handles failures.

## ALB 
Application Load Balancer is AWS managed load balancer. AWS takes care of upgrades , maintenance , avalability.

It has integration with many AWS services. Using health Checks ALB will route traffic to only the healthy nodes.

ALB can route traffic to different target groups based on 
` 1. paths (amazon.com/search  & amazon.com/cart) `
 `2. hosts  (cloud.amazon.com & shop.amazon.com)`
 ` 3. Query (amazon.com?cart=20)`

 ALB is a great fit to container based application , has a port mapping feature to route to specific port, we need  <b>multiple classic Load balancers</b>  per application.
