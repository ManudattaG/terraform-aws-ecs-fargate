# Terraform ECS Fargate

The goal of this effort is to provision the infrastructure and deployment of a containerized service to AWS making use of Terraform and ECS (Elastic Container Service) Fargate --> [AWS ECS Fargate][fargate].


## Implementation

We will be making use of Terraform to initially provision the infrastructure for our service, and eventually use it to apply updates to our application & infrastructure as required.

1. Provisioning Infrastructure on AWS
    * Create Security Groups
    * Create Application Load Balancer
        * Configure Load Balancer Listener
        * Configure Load Balancer Target Groups
    * Create ECR
    * Create ECR Lifecycle Policy
    * Configure IAM Role for ECS Execution
    * Create Task Definitions
    * Create ECS Service
    * Create Cloudwatch group for ECS logs
2. Build and Tag Docker Image
3. Push Tagged Docker Image to ECR
4. Update Task Definition to point to newly built Docker Image

## Components

These components are for a specific environment. There should be a corresponding directory for each environment
that is needed.

| Name | Description | Optional |
|------|-------------|:----:|
| [main.tf][main] | Terraform remote state, AWS provider, output |  |
| [variables.tf][var] | Set actual values of the variables | Yes |
| [ecs_task.tf][ecs] | ECS Cluster, Service, Task Definition, ecsTaskExecutionRole, CloudWatch Log Group |  |
| [alb.tf][alb] | ALB, Target Group, S3 bucket for access logs  |  |
| [nsg.tf][nsg] | NSG for ALB and Task |  |
| [iam_role.tf][iam] | Application Role for container | Yes |
| [cw_logs.tf][cwl] | IAM user that can be used by CI/CD systems | Yes |
| [ecr.tf][ecr] | Add an ECS event log dashboard |  |
| [ecs-event-stream.tf][ees] | Add an ECS event log dashboard | Yes |


## Usage

```
# Move into the base directory
$ cd base

# Sets up Terraform to run
$ terraform init

# Executes the Terraform run
$ terraform apply
```


We can proceed to run ```terraform plan``` which will give us an overview of how our infrastructure is going to be provisioned before actually being provisioned.

On successful execution of ```terraform plan```, we can finally execute our provision plan by running ```terraform apply```.

Once ```terraform apply``` is complete, all the resources would have been created, and should be accessible via the URL provided by our Application Load Balancer.


[fargate]: https://aws.amazon.com/fargate/
[main]: ./base/main.tf
[var]: ./base/variables.tf
[ecs]: ./base/ecs_task.tf
[alb]: ./base/alb.tf
[nsg]: ./base/nsg.tf
[iam]: ./base/iam_role.tf
[cwl]: ./base/cw_logs.tf
[ecr]: ./base/ecr.tf
[ees]: ./base/ecs-event-stream.tf