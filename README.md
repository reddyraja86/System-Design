# System-Design

Scalability : This is "being able to handle more requests" ( Horizontal scaling)

Resillient : In case of failure, Recorver from the  failure node and start serving the user using another node.( Horizontal scaling), where as in vertical scalling you have single point of failure.

Consitent : Data wont be lost.(vertical scalling ,as there is a single node,req and response will come to the same node)

Avalaiblilty : System is always avialble for the end user.( Horizontal scaling)

Fault toularent ( Distributes system ): When we distribute our system it will be fault toularent and might give quick response.( ex face book maintain diff systems and diff geo locations to make it fault toularent and provide quick response)

Load balancer : This will routes the req based on speecific rule.

Logging and Metrics : To identify the status on diff services and diff systems.


We have two types of scalability

Horizontal and vertical  :
========================





