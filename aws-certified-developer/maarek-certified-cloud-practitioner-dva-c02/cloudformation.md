# CloudFormation

* CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).

### Benefits of AWS CloudFormation

* Infrastructure as code:
  * No resources are manually created, which is excellent for control.
  * The code can be version controlled for example using git.
  * Changes to the infrastructure are reviewed through code.
* Cost:
  * Each resource within the stack is tagged with an identifier so you can easily see how much a stack cost.
  * You can estimate the cost of your resource using the CloudFormation Template.
  * Savings strategy: In DEV, you could automate the deletion of templates at 5 PM and recreate them at 8 AM, safely.
* Productivity:
  * Ability to destroy and re-create an infrastructure on the cloud on the fly.
  * Declarative programming (no need to figure out ordering and orchestration)
* Separation of Concern:
  * Create many stacks for many apps, and many layers.
  * Example: VPC stacks, Network stacks, App Stacks.

### How CloudFormation Works

* Templates have to be uploaded in S3 and then referenced in CloudFormation.
* To update a template, we can’t edit previous ones. We have to reupload a new version of the template to AWS.
* Stacks are identified by a name.
* • Deleting a stack deletes every single artifact that was created by CloudFormation.

### Deploying CloudFormation templates

* Manual way:
  * Editing templates in the CloudFormation Designer.
  * Using the console to input parameters, etc.
* Automated way:
  * Editing templates in a YAML file.
  * Using the AWS CLI to deply templates.
  * Recommended way when you fully want to automate your flow.

### CloudFormation - Building Blocks

Templates components:

* Resources: your AWS resources declared in the template (MANDATORY).
* Parameters: the dynamic inputs for your template.
* 3\. Mappings: the static variables for your template.
* 4\. Outputs: References to what has been created.
* 5\. Conditionals: List of conditions to perform resource creation.
* 6\. Metadata.

### CloudFormation - Resources

* Resources are the core of your CloudFormation template (MANDATORY).
* They represent the different AWS Components that will be created and configured.
* Resources are declared and can reference each other.
* Resource types identifiers are of the form:
  * `AWS::aws-product-name::data-type-name`

All the resources can be found here:

* http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aw s-template-resource-type-ref.html
* Example here (for an EC2 instance):
  * http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
  * http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/awsproperties-ec2-instance.html
  * http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/awsproperties-ec2-security-group.html
  * http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/awsproperties-ec2-eip.html

### CloudFormation - Parameters

* Parameters are a way to provide inputs to your AWS CloudFormation template.
* When should I youse a parameter?
  * Ask yourself this:
    * Is this CloudFormation resource configuration likely to change in the future?
    * If so, make it a parameter.
* NOTE: You won’t have to re-upload a template to change its conten.

```yaml
Parameters:
  ParameterLogicalID:
    Type: DataType
    ParameterProperty: value
```

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

```yaml
Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - m1.small
      - m1.large
    Description: Enter t2.micro, m1.small, or m1.large. Default is t2.micro.
```

#### Referencing a parameter within a template

```yaml
Ec2Instance:
  Type: AWS::EC2::Instance
  Properties:
    InstanceType:
      Ref: InstanceTypeParameter
    ImageId: ami-0ff8a91507f77f867
    
# OR

Resources: 
  Instance: 
    Type: AWS::EC2::Instance
    Properties: 
      InstanceType: !Ref InstanceTypeParameter

```

#### CloudFormation - Pseudo Parameters

Pseudo parameters are parameters that are predefined by AWS CloudFormation. You don't declare them in your template. Use them the same way as you would a parameter, as the argument for the `Ref` function (AWS, 2023a).

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Font: AWS, 2023a</p></figcaption></figure>

### CloudFormation - Mappings

* Mappings are fixed variables within your CloudFormation Template.
* They’re very handy to differentiate between different environments (dev vs prod), regions (AWS regions), AMI types, etc.
* All the values are hardcoded within the template.

```yaml
Mappings: 
  Mapping01: 
    Key01: 
      Name: Value01
    Key02: 
      Name: Value02
    Key03: 
      Name: Value03
```

```yaml
Mappings: 
  RegionMap: 
    us-east-1: 
      "HVM64": "ami-0ff8a91507f77f867"
    us-west-1: 
      "HVM64": "ami-0bdb828fd58c52235"
    eu-west-1: 
      "HVM64": "ami-047bb4163c506cd98"
    ap-southeast-1: 
      "HVM64": "ami-08569b978cc4dfa10"
    ap-northeast-1: 
      "HVM64": "ami-06cd52961ce9f0d85"
```

#### Accessing Mapping Values with _Fn::FindInMap_

* _!FindInMap \[ MapName, TopLevelKey, SecondLevelKey ]_

```yaml
Resources: 
  myEC2Instance: 
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
      InstanceType: m1.small
```

The `FindInMap` function specifies key as the region where the stack is created (using the [AWS::Region pseudo parameter](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)) and `HVM64` as the name of the value to map to.

```yaml
  Parameters: 
    EnvironmentType: 
      Description: The environment type
      Type: String
      Default: test
      AllowedValues: 
        - prod
        - test
      ConstraintDescription: must be a prod or test
  Mappings: 
    RegionAndInstanceTypeToAMIID: 
      us-east-1: 
        test: "ami-8ff710e2"
        prod: "ami-f5f41398"
      us-west-2: 
        test: "ami-eff1028f"
        prod: "ami-d0f506b0"
        
      ...other regions and AMI IDs...
  
  Resources:
  
   ...other resources...

  Outputs: 
    TestOutput: 
      Description: Return the name of the AMI ID that matches the region and environment type keys
      Value: !FindInMap [RegionAndInstanceTypeToAMIID, !Ref "AWS::Region", !Ref EnvironmentType]

```

### CloudFormation - Outputs

The optional `Outputs` section declares output values that you can [import into other stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html) (to [create cross-stack references](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/walkthrough-crossstackref.html)), return in response (to describe stack calls), or [view on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html). For example, you can output the S3 bucket name for a stack to make the bucket easier to find (AWS, 2023b)

```yaml
Outputs:
  Logical ID:
    Description: Information about the value
    Value: Value to return
    Export:
      Name: Name of resource to export
```

* Value (required): The value of the property returned by the `aws cloudformation describe-stacks` command. The value of an output can include literals, parameter references, pseudo-parameters, a mapping value, or intrinsic functions (AWS, 2023b) .
* Export (optional): The name of the resource output to be exported for a [cross-stack reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/walkthrough-crossstackref.html) (AWS, 2023b).

```yaml
Outputs:
  StackVPC:
    Description: The ID of the VPC
    Value: !Ref MyVPC
    Export:
      Name: !Sub "${AWS::StackName}-VPCID"
```

### CloudFormation - Cross Stack Reference

* Import outputs from other templates.
* For this, we use the _Fn::**ImportValue**_ function.
* You can’t delete the underlying stack until all the references are deleted too.

```yaml
Outputs:
  StackSSHSecurityGroup:
    Description: The SSH Security Group for our company
    Value: !Ref MyCompanyWideSSHSecurityGroup
    Export:
      Name: SSHSecurityGroup
```

```yaml
Resources: 
  MySecureInstance: 
    Type: "AWS::EC2::Instance"
    Properties:
      AvailabilityZone: us-east-1a 
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
      InstanceType: t2.micro
      SecurityGroups:
        - !ImportValue SSHSecurityGroup
```

### CloudFormation - Conditions

* Conditions are used to control the creation of resources or outputs based on a condition.
* Conditions can be whatever you want them to be, but common ones are:
  * Environment (dev / test / prod).
  * AWS Region.
  * Any parameter value.

```yaml
Conditions:
  CreateProdResources: !Equals [!Ref EnvType, prod]
```

* The logical ID is for you to choose. It’s how you name condition.
* The intrinsic function (logical) can be any of the following.
  * `Fn::And`
  * `Fn::Equals`
  * `Fn::If`
  * `Fn::Not`
  * `Fn::Or`

#### Using a Conditions

```
Resources:
  MountPoint:
    Type: "AWS::EC2::VolumeAttachment"
    Condition: CreateProdResources
```

### CloudFormation - Must Know Functions

* `Ref`
* `Fn::GetAtt`
* `Fn::FindInMap`
* `Fn::ImportValue`
* `Fn::Join`
* `Fn::Sub`
* Condition Functions (Fn::If, Fn::Not, Fn::Equals, etc…)

#### Fn::Ref

* The Fn::Ref function can be leveraged to reference:
  * Parameters -> returns the value of the parameter.
  * Resources -> returns the physical ID of the underlying resource (ex: EC2 ID).

```yaml
DbSubnet1:
  Type: AWS::EC2::Subnet
  Properties:
   VpcId: !Ref MyVPC
```

#### Fn::GetAtt

* **Attributes** are attached to any resources you create.
* To know the attributes of your resources, the best place to look at is the documentation.
* For example: the AZ of an EC2 machine!

```yaml
Resource:
  EC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: ami-1234567
      InstanceType: t2.micro
  NewVolume:
    Type: "AWS::EC2::Volume"
    Condition: CreateProdResources
    Properties:
      Size: 100
      Availability:
        !GetAtt EC2Instance.AvailabilityZone # Here!!!
```

#### Fn::Join

* Join values with a delimiter
  * !Join \*\*\[\*\*delimiter, \[_comma-delimited list of values_] **]**
* **Example: "a**:b:**c"**
  * !Join \[":", \[a,b,c]]

#### Fn::Sub

* Fn::Sub, or !Sub as a shorthand, is used to substitute variables from a text. It’s a very handy function that will allow you to fully customize your templates.

```yaml
!Sub
  - String
  - Var1Name: Var1Value
    Var2Name: Var2Value
    
# OR

Fn::Sub:
  - String
  - Var1Name: Var1Value
    Var2Name: Var2Value
```

**Example `Fn::Sub` with a mapping**

* The following example uses a mapping to substitute the `${Domain}` variable with the resulting value from the `Ref` function.

```yaml
Name: !Sub 
  - 'www.${Domain}'
  - Domain: !Ref RootDomainName
```

**Example `Fn::`**`Sub` **without a mapping**

* The following example uses Fn::Sub with the `AWS::Region` and `AWS::AccountId` pseudo parameters and the `vpc` resource logical ID to create an Amazon Resource Name (ARN) for a VPC.

_!Sub 'arn:aws:ec2:${AWS::Region}:${AWS::AccountId}:vpc/${vpc}'_

**Example inline `!Sub` usage**

```yaml
Outputs:
  StackVPC:
    Description: The ID of the VPC
    Value: !Ref MyVPC
    Export:
      Name: !Sub "${AWS::StackName}-VPCID"
```

### CloudFormation - Rollbacks

* Stack Creation Fails:
  * :warning: :warning::warning: **Default:** everything rolls back (gets deleted). We can look at the log.
  * Option to disable rollback and troubleshoot what happened.
* Stack Update Fails
  * The stack automatically rolls back to the previous known working state.
  * Ability to see in the log what happened and error messages.

### CloudFormation - Stack Notifications

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### References

[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html) 2023a

[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html) 2023b
