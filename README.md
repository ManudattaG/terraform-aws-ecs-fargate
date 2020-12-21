# Terraform ECS Fargate

The goal of this effort is to provision the infrastructure and deployment of a containerized orchestration service to AWS making use of Terraform and ECS (Elastic Container Service) Fargate --> [AWS ECS Fargate][fargate].


## Implementation

We will be making use of Terraform to initially provision the infrastructure for the service, and eventually use it to apply updates to the application & infrastructure as required.

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
| [ecs_task.tf][ecs] | ECS Cluster, Service, Task Definition, ecsTaskExecutionRole |  |
| [alb.tf][alb] | ALB, Target Group, Listener  |  |
| [nsg.tf][nsg] | Security groups for ALB and ECS |  |
| [iam_role.tf][iam] | Roles and permissions for ECS tasks | Yes |
| [cw_logs.tf][cwl] | Container to send and collect log information | Yes |
| [ecr.tf][ecr] | ECR for storing docker images |  |
| [ecs-event-stream.tf][ees] | Add an ECS event log dashboard | Yes |


## Usage

```
# Move into the base directory
$ cd base

# Sets up Terraform to run
$ terraform init

# Sets up resource plan before applying
$ terraform plan

# Executes the Terraform run
$ terraform apply
```


We can proceed to run ```terraform plan``` which will give us an overview of how the infrastructure is going to be provisioned before actually being provisioned.

On successful execution of ```terraform plan```, we can finally execute the provision plan by running ```terraform apply```.

Once ```terraform apply``` is complete, all the resources would have been created, and should be accessible via the URL provided by the Application Load Balancer.


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