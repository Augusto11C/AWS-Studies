# CI/CD - CodeCoomit, CodePipeline, CodeBuild, CodeDeploy and CodeStar

* AWS CodeCommit: \~ GitHub, GitLab, Bitbucket.
* [AWS CodeBuild](https://aws.amazon.com/codebuild/): A fully managed **continuous integration** service that compiles source code, runs tests, and produces software packages that are ready to deploy, on a dynamically created build server.
* [AWS CodeDeploy](https://aws.amazon.com/codedeploy/): A fully managed deployment (**continuous deployment**) service that automates software deployments to a variety of compute services such as Amazon EC2, [AWS Fargate](https://aws.amazon.com/fargate/), [AWS Lambda](http://aws.amazon.com/lambda), and your on-premises servers.
* [AWS CodePipeline](https://aws.amazon.com/codepipeline/): A fully managed [**continuous delivery**](https://aws.amazon.com/devops/continuous-delivery/) service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Font: <a href="https://www.youtube.com/@TinyTechnicalTutorials">Tiny Technical Tutorials</a>, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CodeCommit

* **AWS CodeCommit:**
  * Private Git repositories.
  * No size limit on repositories (scale seamlessly).
  * Fully managed, highly available.
  * Code only in AWS Cloud account => increased security and compliance.
  * Security (encrypted, access control, …).
  * Integrated with Jenkins, AWS CodeBuild, and other CI tools.
* **Authentication:**
  * SSH Keys: AWS Users can configure SSH keys in their IAM Console.
  * HTTPS: with AWS CLI Credential helper or Git Credentials for IAM user.
* **Authorization:** IAM policies to manage users/roles permissions to repositories.
* **Encryption:**
  * :warning: Repositories are automatically encrypted at rest using AWS KMS.
  * Encrypted in transit (can only use HTTPS or SSH – both secure).

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CodePipeline

* **Source:** CodeCommit, ECR, S3, Bitbucket, GitHub.
* **Build:** CodeBuild, Jenkins, CloudBees, TeamCity.
* **Test:** CodeBuild, AWS Device Farm, 3rd party tools, …
* **Deploy:** CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3,...
* **Invoke:** Lambda, Step Functions.
* **Consists of stages:**
  * Each stage can have sequential actions and/or parallel actions.
  * Manual approval can be defined at any stage.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

* :point\_up:A stage can have multiple Action Groups.
* In the image abova the stage DeployToProd has to action groups:
  * (1) The first Action group requires a manual approve to proceed to the next action group.
  * (2) The second Action group is executed when there is an manual approve and it will deploy in elasticbeanstalk an application in the prod environment.

#### Events vs. Webhooks vs. Polling

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Manual Approval Stage (Exam Question)

* IAM User must have permission to access the pipeline and approve the stage/step.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CodeBuild

* A fully managed continuous integration (CI) service.
* Continuous scaling (no servers to manage or provision – no build queue).
* Compile source code, run tests, produce software packages, …
* Alternative to other build tools (e.g., Jenkins).
* **Charged per minute** for _compute resources_ (time it takes to complete the builds).
* Leverages Docker under the hood for reproducible builds.
* Use prepackaged Docker images or create your own custom Docker image.
* Security:
  * Integration with KMS for encryption of build artifacts.
  * IAM for CodeBuild permissions, and VPC for network security.
  * AWS CloudTrail for API calls logging.

***

* **Source:** **CodeCommit**, S3, Bitbucket, GitHub.
* **Build instructions:** Code file `buildspec.yml` or insert manually in Console.
* Output logs can be stored in Amazon S3 & CloudWatch Logs.
* Use CloudWatch Metrics to monitor build statistics.
* Use **EventBridge** to **detect failed builds** and trigger notifications.
* Use CloudWatch Alarms to notify if you need "thresholds" for failures.
* Build projects can be defined with CodePipeline or Code Build

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* CodeBuild will run every thing inside a docker container. It will use the image specified in the `run-as: Linux-user-name` (2) in the file `buildspec.yml` or a default image choose by AWS (1).

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

#### The file buildspec.yml

* `buildspec.yml` file must be at the root of your code.
* env - define environment variables:
  * variables - plaintext variables.
  * parameter-store - variables stored in SSM Parameter Store.
  * secrets-manager - variables stored in AWS Secrets Manager.
* phases – specify commands to run:
  * install - install dependencies you may need for your build.
  * pre\_build - final commands to execute before build.
  * Build - actual build commands.
  * post\_build - finishing touches (e.g., zip output).
* artifacts - what to upload to S3 (encrypted with KMS).
* cache - files to cache (usually dependencies) to S3 for future build speedup.

```yaml
version: 0.2

run-as: Linux-user-name

env:
  shell: shell-tag
  variables:
    key: "value"
    key: "value"
  parameter-store:
    key: "value"
    key: "value"
  exported-variables:
    - variable
    - variable
  secrets-manager:
    key: secret-id:json-key:version-stage:version-id
  git-credential-helper: no | yes

proxy:
  upload-artifacts: no | yes
  logs: no | yes

batch:
  fast-fail: false | true
  # build-list:
  # build-matrix:
  # build-graph:
        
phases:
  install:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    runtime-versions:
      runtime: version
      runtime: version
    commands:
      - command
      - command
    finally:
      - command
      - command
    # steps:
  pre_build:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    commands:
      - command
      - command
    finally:
      - command
      - command
    # steps:
  build:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    commands:
      - command
      - command
    finally:
      - command
      - command
    # steps:
  post_build:
    run-as: Linux-user-name
    on-failure: ABORT | CONTINUE
    commands:
      - command
      - command
    finally:
      - command
      - command
    # steps:
reports:
  report-group-name-or-arn:
    files:
      - location
      - location
    base-directory: location
    discard-paths: no | yes
    file-format: report-format
artifacts:
  files:
    - location
    - location
  name: artifact-name
  discard-paths: no | yes
  base-directory: location
  exclude-paths: excluded paths
  enable-symlinks: no | yes
  s3-prefix: prefix
  secondary-artifacts:
    artifactIdentifier:
      files:
        - location
        - location
      name: secondary-artifact-name
      discard-paths: no | yes
      base-directory: location
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: no | yes
      base-directory: location
cache:
  paths:
    - path
    - path

```

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: openjdk11
  pre_build:
    commands:
      - echo Executing pre-build steps...
      # Add any pre-build steps here
  build:
    commands:
      - echo Building the Java application...
      - ./gradlew build
      # Add any additional build steps or custom commands here
  post_build:
    commands:
      - echo Executing post-build steps...
      # Add any post-build steps here
artifacts:
  files:
    - build/libs/*.jar
```

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

#### CodeBuild - Local Build

* In case of need of deep troubleshooting beyond logs…
* You can run CodeBuild locally on your desktop (after installing Docker).
* [https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)

#### CodeBuild - Inside VPC

* By default, your CodeBuild containers are launched outside your VPC.
* You can specify a VPC configuration:
  * • VPC ID.
  * Subnet IDs.
  * Security Group IDs.
* Then your build can access resources in your VPC (e.g., RDS, ElastiCache, EC2, ALB, …)
* **Use cases:** integration tests, data query, internal load balancers, …

#### CodePipeline - CloudFormation Integration

* CloudFormation is used to deploy complex infrastructure using an API.
  * CREATE\_UPDATE – create or update an existing stack
  * DELETE\_ONLY – delete a stack if it exists

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* (1) CloudFormation deploy/creates an infra & app in a test environment.
* (2) CloudBuild test this newly created environment
* (3) CloudFormation, after the tests, delete the test environment.
* (4) CloudFormation, after the deletion os the test environment, deploy/creates the prod environment.

### CodeDeploy

* Deployment service that automates application deployment.
* Deploy new applications versions to EC2 Instances, On -premises servers, Lambda functions, ECS Services.
* Automated Rollback capability in case of failed deployments, or trigger CloudWatch Alarm.
* Gradual deployment control.
* :warning:A file named `appspec.yml` defines how the deployment happens.

```yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
permissions:
  - object: /var/www/html
    owner: ec2-user
    group: apache
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/application_start.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root
```

#### CodeDeploy - EC2/On-premises Platform

* Can deploy to EC2 Instances & **on-premises** servers.
* Perform **in-place deployments** OR **blue/green** deployments.
* Must run the **CodeDeploy Agent** on the **target instances**.
* Define deployment speed
  * **AllAtOnce:** most downtime.
  * **HalfAtATime:** reduced capacity by 50%.
  * **OneAtATime:** slowest, lowest availability impact.
  * **Custom:** define your %.

**CodeDeploy Agent**

* :warning: The CodeDeploy Agent must be running on the EC2 instances as a prerequisites.
* It can be installed and updated automatically if you’re using Systems Manager.
* :warning: The EC2 Instances must have sufficient permissions to access Amazon S3 to get deployment bundles.

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CodeDeploy - Lambda Platform

* CodeDeploy can help you automate **traffic shift for Lambda aliases.**
* Feature is integrated within the SAM framework.
  * > The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications. It provides a shorthand syntax to express serverless applications and is an extension of AWS CloudFormation. AWS SAM consists of two primary parts: the AWS SAM template specification and the AWS SAM command line interface (CLI). The AWS SAM template specification is an open-source framework that you can use to define your serverless application infrastructure on AWS. The AWS SAM CLI is a command-line tool that you can use with AWS SAM templates and supported third-party integrations to build and run your serverless applications (AWS, 2023a).
* Linear: grow traffic every N minutes until 100%:
  * LambdaLinear10PercentEvery3Minutes.
  * LambdaLinear10PercentEvery10Minutes.
* Canary: try X percent then 100%:
  * LambdaCanary10Percent5Minutes.
  * LambdaCanary10Percent30Minutes.
* AllAtOnce: Immediate.

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CodeDeploy - ECS Platform

* Only Blue/Green Deployments.
* Linear: grow traffic every N minutes until 100%:
  * LambdaLinear10PercentEvery3Minutes.
  * LambdaLinear10PercentEvery10Minutes.
* Canary: try X percent then 100%:
  * LambdaCanary10Percent5Minutes.
  * LambdaCanary10Percent30Minutes.
* AllAtOnce: Immediate.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CodeDeploy - Deployment to EC2

* Define how to deploy the application using appspec.yml + Deployment Strategy.
* Will do In -place update to your fleet of EC2 instances.
* Can use hooks to verify the deployment after each deployment phase.

#### CodeDeploy - Deploy to an ASG

* In-place Deployment:
  * Updates existing EC2 instances.
  * Newly created EC2 instances by an ASG will also get automated deployments.
* Blue/Green Deployment:
  * A new Auto-Scaling Group is created (settings are copied).
  * Choose how long to keep the old EC2 instances (old ASG).
  * :warning: Must be using an ELB.

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CodeDeploy - Redeploy & Rollbacks

* Rollback = redeploy a previously deployed revision of your application.
* Deployments can be rolled back:
  * Automatically: rollback when a deployment fails or rollback when a CloudWatch Alarm thresholds are met.
  * Manually.
* Disable Rollbacks: do not perform rollbacks for this deployment.
* :warning: If a roll back happens, CodeDeploy redeploys the last known good revision as a new deployment (not a restored version).

#### CodeDeploy - Console Config

* Creating a CodeDeploy Application called DemoApplication:

<figure><img src="../../.gitbook/assets/image (251).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

* Code Deploy Deployment Group for DemoApplication:

<figure><img src="../../.gitbook/assets/image (253).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (254).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (255).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (256).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (257).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (258).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

### References

Tine Technical Tutorials. _Basics of CodeStar, Code Pipeline, CodeBuild, Cloud9, CodeCommit, CodeDeploy - AWS for Beginners_. \[Videoaula]. YouTube, 2023. Disponível em: <[https://www.youtube.com/watch?v=iGCJ-N7bPX0](https://www.youtube.com/watch?v=iGCJ-N7bPX0)>. Acesso em: 01 oct. 2023.

AWS. _What is the AWS Serverless Application Model (AWS SAM)?_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)>. Acesso em: 01 oct. 2023a.
