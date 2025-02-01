# Containers on AWS

* Amazon Elastic Container Service (Amazon ECS).
  * Amazon's own container platform.
* Amazon Elastic Kubernetes Service (Amazon EKS).
  * Amazon's managed Kubernetes (open source).
* AWS Farget.
  * Amazon's own serverless container platform.
  * Works with ECS and with EKS.
* Amazon ECR.
  * Store container images (\~ dockerhub).

### Amazon ECS

[Task Role vs. Task Execution Role](https://www.youtube.com/shorts/VQfRQ9gGYD4)

Launch Types: Farget, EC2

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - EC2 Launch Type

* Launch Docker containers on AWS = Launch **ECS Tasks** on **ECS Clusters.**
* :warning: EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances).
* **Each EC2** Instance must **run the ECS Agent** to register in the ECS Cluster.
* AWS takes care of starting / stopping containers.

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Fargate Launch Type

* You do **not provision the infrastructure** (no EC2 instances to manage.
* It is all Serverless.
* Create only task definitions.
* AWS runs ECS Tasks based on CPU/RAM needed.
* To scale, just increase the number of tasks. Simple - no more EC2 instances.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - IAM Roles for ECS

* EC2 Instance Profile (:warning: EC2 Launch Type only):
  * Used by the ECS agent.
  * Makes API calls to ECS service.
  * Send container logs to CloudWatch Logs.
  * Pull Docker image from ECR.
  * Reference sensitive data in Secrets Manager or SSM Parameter Store.
* ECS Task Role:
  * Allows each **task to have a specific role.**
  * Use different roles for the different ECS Services you run.
  * Task Role is defined in the task definition.
* [Task Role vs. Task Execution Role](https://www.youtube.com/shorts/VQfRQ9gGYD4)

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Load Balancer Integrations

* Application Load Balancer supported and works for most use cases.
* Network Load Balancer recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link.
* :thumbsdown: Classic Load Balancer supported but **not recommended** (no advanced features – no Fargate)

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Data Volumes (EFS)

* Mount EFS file systems onto ECS tasks.
* Works for both EC2 and Fargate launch types.
* Tasks **running in any AZ** will share the same data in the EFS file system.
* **Fargate + EFS = Serverless.**
* Use cases: persistent multi-AZ shared storage for your containers.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### ECS Service Auto Scaling

* Amazon ECS Auto Scaling uses **AWS Application Auto Scaling.**
  * ECS Service Average CPU Utilization.
  * ECS Service Average Memory Utilization - Scale on RAM.
  * :thumbsup:ALB Request Count Per Target – metric coming from the ALB.

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>

* **Target Tracking** - scale based on target value for a specific CloudWatch metric.
* **Step Scaling** - scale based on a specified CloudWatch Alarm.
* **Scheduled Scaling** - scale based on a specified date/time (predictable changes).

#### EC2 Launch Type - Auto Scaling EC2 Instances

* Accommodate ECS Service Scaling by adding underlying EC2 Instances.
*   **Auto Scaling Group Scaling.**

    * Scale your ASG based on CPU Utilization.
    * Add EC2 instances over time.
    *

    ```
    <figure><img src="../../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>
    ```
* **ECS Cluster Capacity Provider:**
  * Used to automatically provision and scale the infrastructure for your ECS Tasks.
  * Capacity Provider paired with an Auto Scaling Group.
  * Add EC2 Instances when you’re missing capacity (CPU, RAM…)
  * [Amazon ECS Cluster Auto Scaling with a Capacity Provider](https://www.youtube.com/watch?v=0j8D-be2J1k)

<div data-full-width="false"><figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption><p>Font: DAVIS, 2023</p></figcaption></figure></div>

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### ECS Scaling - Service CPU Usage Example

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>

#### ECS Rolling Updates

* When updating from v1 to v2, we can control how many tasks can be started and stopped, and in which order.
* Service's Task Config Update Screen (console):
  * ![](<../../.gitbook/assets/image (71).png>)
  * min healthy percent: 500 and Max healthy percent: 100
    * ![](<../../.gitbook/assets/image (72).png>)
  * min healthy percent: 100 and Max healthy percent: 200
    * ![](<../../.gitbook/assets/image (73).png>)

#### ECS tasks invoked by Event Bridge

Use Case 1:

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

Use Case 2 - Schedule:

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

Use Case 3 - Intercept Stopped Tasks using EventBridge:

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Task Definitions

* Task definitions are metadata in JSON form to tell ECS how to run a Docker Container.
* It contains crucial information, such as:
  * Image name.
  * Port binding for container and host.
  * Memory and CPU required.
  * Environment variables.
  * Networking information.
  * IAM Role.
  * Logging configuration (ex: CloudWatch).
* Can define up to 10 containers in a Task Definition.

#### Amazon ECS - One IAM Role per Task Definition

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Load Balancing (EC2 Launch Type)

* If **only** the **container port** in the task definition, a port in the host will be dynamic mapped.
* Although there is this dynamic port allocation, the ALB can find the right port on your EC2 Instances.
* In order for what was described above to work, you must allow on the **EC2** instance’s **Security Group** **any port from the ALB’s Security Group**:
  *   :exclamation::exclamation::warning: My understanding:

      * EC2s holds docker containers.

      > Docker containers are inside EC2. Although the requests are for the Docker container because it "lives" inside an EC2, you need to set permissions for the ALB to access the EC2 running your container to establish communication. You set this permission in the security group of the EC2 instances acting as capacity providers (BUENO, 2023).

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Load Balancing (Fargate Launch Type)

* Each task has a unique private IP.
* **Only define the container port** (host port is not applicable).
* Example:
  * ECS ENI Security Group:
    * Allow port 80 from the ALB.
  * ALB Security Group:
    * Allow port 80/443 from web.

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Environment Variables

* Environment Variable:
  * Hardcoded - e.g., URLs
  * SSM Parameter Store - sensitive variables (e.g., API Keys, shared Configs).
  * Secrets Manager - sensitive variables 9e.g., DB passwords).
* Environment Files (bulk) - Amazon S3.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Data Volumes (Bind Mounts)

* Share data between multiple containers in the same task definition.
* works for both ec2 and farget tasks.
* **EC2 Tasks - using EC2 instance storage:**
  * Data are tied to the lifecycle of the EC2 instance.
* **Fargate tasks - using ephemeral storage:**
  * Data are tied to the containers using them
  * 20GB - 200GB (default 20GB).
* Use Cases:
  * Share ephemeral data between multiple containers.
  * “Sidecar” container pattern, where the “sidecar” container used to send metrics/logs to other destinations (separation of conerns).

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Tasks Placement

* :warning: Note: only for ECS tasks with EC2 Launch Type (**Farget not supported**).
* When an ECs tasks is started with EC2 Launch Type, ECS, must determine where to place it, with the constraints of CPU and memory (RAM).
* When a service in, ECS needs to determine which tasks to terminate.
* Can define:
  * **Task Placement Strategy.**
  * **Task Placement Constraints.**

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon ECS - Tasks Placement Process

* Task placement strategies are a best effort.
* When Amazon ECS places a task, it use the following process to select the appropriate EC2 Containers instance:
  * Identify which instances that satisfy the **CPU, memory, and port** requirements.
  * Identify which instances that satisfy the **Task Placement Constraints.**
  * Identify which instances that satisfy the **Task Placement Strategies.**
  * Select the instances based on the steps described above.

#### Amazon ECS - Tasks Placement Strategies

* **Binpack**
  * Tasks are placed on the least available amount of CPU and Memory.
  * Minimizes the number of EC2 instances in use (cost savings).

```
"placementStrategy": [
    {
        "type": "binpack",
        "field": "memory"
    }
]
```

* **Random**
  * Tasks are placed randomly.
* **Spread**
  * Tasks are placed evenly based on the specified value.
  * Example: instanceId, attributes:ecs.availability-zone, ...

{% code overflow="wrap" %}
```
"placementStrategy": [
    {
        "type": "spread",
        "field": "attributes:ecs:availability-zone"
    }
]
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* **Mixed Strategies**

```
"placementStrategy": [
    {
        "type": "spread",
        "field": "attributes:ecs:availability-zone"
    },
    {
        "type": "binpack",
        "field": "memory"
    }
]
```

#### Amazon ECS - Tasks Placement Constraints

* _**distinctInstance**_
  * tasks are placed on a different EC2 instance.

```
"placementConstraints": [
    {
        "type": "distinctInstance"
    }
]
```

* _**memberOf**_
  * tasks are placed on EC2 instances that satisfy a specified expression.
  * Uses the **Cluster Query Language**

```
"placementConstraints": [
    {
        "type": "memberOf",
        "expression": "attribute:ecs.availability-zone in [eu-west-2a, eu-west-2b]
        # // OR "expression": "attribute:ecs.instance-type =~t2.*"
    }
]
```

**Console Config Example**

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption><p>Font: BUENO, 2023</p></figcaption></figure>

### Amazon Elastic Container Registry (ECR)

* Store and Manage Docker images on AWS.
* Private and Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws).
* Fully integrated with ECS, backed by Amazon S3.
* Access is controlled through IAM (permission errors => policy).
* Supports image vulnerability scanning, versioning, image tags, image lifecycle, …

#### Amazon ECR - Using AWS CLI

```
# AWS CLI V2
aws ecr get-login-password --region <region> | docker login --username AWS
--password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com

# Docker Commands
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/demo:latest

docker pull <aws_account_id>.dkr.ecr.<region>.amazonaws.com/demo:latest
```

Note: In case an EC2 instance (or you) can’t pull a Docker image, check IAM permissions.

### AWS Copilot

* CLI tool to build, release, and operate production-ready containerized apps.
* Run your apps on AppRunner, ECS, and Fargate.
* Helps you focus on building apps rather than setting up infrastructure.
* Provisions all required infrastructure for containerized apps (ECS, VPC, ELB, ECR…).
* Automated deployments with one command using CodePipeline.
* Deploy to multiple environments.
* Troubleshooting, logs, health status…

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Amazon EKS Overview

* Amazon EKS = Amazon Elastic Kubernetes Service.
* It is a way to launch managed Kubernetes clusters on AWS.
* Kubernetes is an open-source system for automatic deployment, scaling and management of containerized (usually Docker) application.
* t’s an alternative to ECS, **similar goal** but different API.
* EKS supports **EC2** if you want to deploy worker nodes or **Fargate** to deploy serverless containers.
* :warning:Use Case:
  * if your company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes.
* For multiple regions, deploy one EKS cluster per region.
* Collect logs and metrics using CloudWatch Container Insights.

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Amazon EKS - Node Types

* Managed Node Groups:
  * Creates and manages Nodes (EC2 Instances) for you.
  * Nodes are part of an ASG Managed by EKS.
  * Supports On-Demand or Spot Instances.
* Self-managed Nodes:
  * Nodes created by you and registered to the EKS cluster and managed by an ASG.
  * You can use prebuilt AMI - Amazon EKS Optimized AMI.
  * Supports On-Demand or Spot Instances.
* AWS Farget:
  * No maintenance required; no nodes managed.

#### Amazon EKS - Data Volumes

* Need to specify StorageClass manifest on your EKS cluster.
* Leverages a Container Storage Interface (CSI) compliant driver.
* Support for...
  * Amazon EBS.
  * Amazon EFS (works with Fargate).
  * Amazon FSx for Lustre.
  * Amazon FSx for NetApp ONTAP.

### References

DAVIS, Neal. _Amazon ECS Cluster Auto Scaling with a Capacity Provider_. \[Videoaula]. YouTube, 2023. Disponível em: <[https://www.youtube.com/watch?v=0j8D-be2J1k](https://www.youtube.com/watch?v=0j8D-be2J1k)>. Acesso em: 26 set. 2023.
