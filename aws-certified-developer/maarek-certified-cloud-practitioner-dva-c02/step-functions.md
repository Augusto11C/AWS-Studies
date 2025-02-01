# Step Functions

AWS Step Functions is a service that lets you coordinate multiple AWS services into serverless workflows so you can build and update apps quickly.

* **Workflow Orchestration**: AWS Step Functions allows you to design and run workflows that stitch together services, such as AWS Lambda, AWS Fargate, and Amazon DynamoDB, into feature-rich applications. Each step in the workflow is a specific task, which could be a Lambda function, an ECS task, or inserting data into DynamoDB.
* **Starting a Workflow**: Workflows can be initiated in several ways. You can use an SDK API call, API Gateway, CloudWatch Events or Amazon EventBridge. You can also manually launch a step function from the AWS Management Console.
* **Tasks**: In AWS Step Functions, your state machine is composed of one or more states. A task state represents a single unit of work performed by a state machine. Tasks could involve invoking a Lambda function, running an ECS task, inserting an item into DynamoDB, publishing a message into SNS or SQS, or even launching another step function workflow.
* **Activities**: A task could also be running one activity. An activity is a piece of code that runs some work outside of AWS Step Functions. This could be hosted on an EC2 machine, an Amazon ECS task, or even on-premises servers. These activities pull for work from Step Functions, perform the work, and then send back the results.
* **Role of Step Functions**: The primary role of AWS Step Functions is to orchestrate and manage the execution of workflows (state machines). It helps define what your workflow will look like and coordinates the tasks involved in the workflow.



