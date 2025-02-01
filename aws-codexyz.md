# AWS CodeXYZ

* [AWS CodeBuild](https://aws.amazon.com/codebuild/) – A fully managed **continuous integration** service that compiles source code, runs tests, and produces software packages that are ready to deploy, on a dynamically created build server.
* [AWS CodeDeploy](https://aws.amazon.com/codedeploy/) – A fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, [AWS Fargate](https://aws.amazon.com/fargate/), [AWS Lambda](http://aws.amazon.com/lambda), and your on-premises servers.
* [AWS CodePipeline](https://aws.amazon.com/codepipeline/) – A fully managed [**continuous delivery**](https://aws.amazon.com/devops/continuous-delivery/) service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.
  * CodePipeline can be used to create an end-to-end pipeline that **fetches the application code from CodeCommit**, _builds and tests using CodeBuild_, **and finally deploys using CodeDeploy.**
* AWS CodeStar

<figure><img src=".gitbook/assets/image (137).png" alt=""><figcaption><p>Font: <a href="https://www.youtube.com/@TinyTechnicalTutorials">Tiny Technical Tutorials</a>, 2023</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (138).png" alt=""><figcaption><p>Font: <a href="https://www.youtube.com/@TinyTechnicalTutorials">Tiny Technical Tutorials</a>, 2023</p></figcaption></figure>

### Example of Architecture Using CodeXYZ Services

<figure><img src=".gitbook/assets/image (154).png" alt=""><figcaption><p>Font: <a href="https://aws.amazon.com/blogs/devops/complete-ci-cd-with-aws-codecommit-aws-codebuild-aws-codedeploy-and-aws-codepipeline/">AWS Doc</a>, 2023</p></figcaption></figure>

In summary, the solution has the following workflow:

* A change or commit to the code in the CodeCommit application repository triggers CodePipeline with the help of a CloudWatch event.
* The pipeline downloads the code from the CodeCommit repository, initiates the Build and Test action using CodeBuild, and securely saves the built artifact on the S3 bucket.
* If the preceding step is successful, the pipeline triggers the Deploy in Dev action using CodeDeploy and deploys the app in dev account.
* If successful, the pipeline triggers the Deploy in Prod action using CodeDeploy and deploys the app in the prod account.
