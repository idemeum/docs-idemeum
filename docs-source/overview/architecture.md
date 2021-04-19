# Microservices architecture

At idemeum we have chosen to build our passwordless identity platform leveraging **Microservice-based Architecture (MSA)**. Our goal is to achieve high velocity in software development while ensuring scalability, availability, and resilience of our cloud infrastructure.

Our blog talks in details about our journey with microservices. 

[Learn more](https://blog.idemeum.com/microservice-scalability/){ .md-button }

## Why microservices?

We want to be very agile in responding to customer requests and avoid re-deployment of the whole code base for every small change or fix. What is more, we do not want to put reliability of code at risk where any potential bug can bring the whole system down. And of course we want to scale our software development team fast where any developer joining the team can quickly understand the codebase and should not depend on any tribal or historical knowledge.

Here are the benefits that we are able to achieve with our current infrastructure design. 

1. **Smaller teams** - engineering teams can be smaller and more targeted, each focused on autonomously managing, updating, and scaling a microservice. When onboarding new engineers there is no need to spend weeks figuring out how monolith works whereby increasing team velocity and removing learning curve barrier.  
2. **Development flexibility** - each team has a flexibility to choose technology stacks that best suit the need of the service and business capability they are developing. Exploration of new technologies and refactoring can be done incrementally as opposed to requiring "big-bang" changes.
3. **Deployment flexibility** - teams can practice independent continuous integration and deployment of the business capabilities. With MSA a microservice can be updated without having to redeploy the whole application, making it easier to deliver small updates and changes.
4. **Flexible resource allocation** - with MSA different services can have different availability requirements and have resources assigned based on criticality and business needs.
5. **Resilience** - with MSA there is not centralized point of failure, and when a certain microservice goes down, the whole application does not stop functioning. Errors are isolated and are easier to remedy. We also believe that MSA is the architecture that provides a path to four 9s SLA with high internal SLO and SLI.

## How we designed our architecture

Here are the principles that we relied upon when designing idemeum architecture. 

1. **Single Responsibility Principle (SRP)** - microservices need to be cohesive and implement a small set of strongly related functions.
2. **Common Closure Principle** - microservices must conform to a principle where things that change together should be packaged together in order to ensure that each change affects only one service.
3. **Independent data store** - microservice doesn’t share database tables with other microservices.
4. **Thoughtfully stateful or stateless** - whether it requires access to a persistence store or is it going to be a stateless service providing compute only processing.
5. **Data availability needs** - data availability needs are accounted for - data distribution, data replication and understanding the impact to other microservices that rely on this new microservice.

## Deployment topology

![idemeum deployment topology](/assets/architecture/architecture.png)

idemeum deployment topology has no single point of failure and provides high availability and fault tolerance using the following AWS services:

* Virtual Private Cloud (VPC)
* Public and Private subnets
* Multiple Availability Zones
* Cross-AZ Load Balancer
* Internet Gateway
* NAT Gateway
* CloudFront / CDN