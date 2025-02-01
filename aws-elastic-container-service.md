# Amazon Elastic Container Service (ECS)

Amazon Elastic Container Service (ECS) is, according to Amazon,
> …a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster.

It is comparable to Kubernetes, Docker Swarm, and Azure Container Service.

ECS runs containers on a cluster of Amazon EC2 (Elastic Compute Cloud) virtual machine instances pre-installed with Docker.

## ECS Terms (Task Definition, Task, and Service)

**Task Definition**
Task definition is the blueprint describing which Docker containers to run and represents the application.

**Task**
An instance of a Task Definition, running the containers detailed within it. Multiple Tasks can be created by one Task Definition, as demand requires.

**Service**


![image](https://user-images.githubusercontent.com/17462762/185269682-b755d7d4-0e07-4998-b29e-73e9d3e0bed5.png)
![image](https://cdn-media-1.freecodecamp.org/images/eL718lUcFCktxO96DKpdAIu1uBguoNqOKHRF)
![image](https://cdn-media-1.freecodecamp.org/images/Ff1BK5c7YRL7KAv71Hu7ZSwByYpeJPTaolzT)

[FONTE](https://www.freecodecamp.org/news/amazon-ecs-terms-and-architecture-807d8c4960fd/
