# Terraform
# What is Terraform?
Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. it is developed by Hashicorp & written in GoLang

Terraform is also known as infrastructure as code solution.

# 🌱 Terraform Lifecycle: Overview
Terraform's lifecycle revolves around the management of infrastructure resources, from creation to deletion, in a declarative way.

The lifecycle includes the following phases:
Write
Init
Plan
Apply
Change/Update
Destroy

Each of these steps fits into a typical workflow.

🔁 1. Write (Configuration)
You define infrastructure as code in .tf files using HCL (HashiCorp Configuration Language).

Example: A simple AWS EC2 instance

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
➡️ This defines the desired state of the infrastructure.

⚙️ 2. Init (terraform init)
This initializes the Terraform working directory:

Downloads the necessary provider plugins (e.g., AWS, Azure, etc.)

Prepares the backend (e.g., S3 + DynamoDB for remote state)

Sets up required modules

terraform init
✅ Run once when starting a new project or after adding providers/modules.

🧠 3. Plan (terraform plan)
Generates an execution plan by comparing the current state (in .tfstate) with the desired state (in .tf files).

terraform plan
Terraform will show:

What will be created

What will be changed

What will be destroyed

No actual changes are made yet—this is a safe dry run.

✅ Use it before every apply to verify changes.

🚀 4. Apply (terraform apply)
Applies the planned changes to the infrastructure—this is where the real provisioning happens.

terraform apply
It executes the actions in the plan

You confirm it (unless you use -auto-approve)

It updates the terraform.tfstate file to reflect the actual infrastructure

✅ Terraform ensures idempotency, so it won’t re-create unchanged resources.

♻️ 5. Change/Update
When you modify .tf files:

Update resource configurations (e.g., change instance type or add tags)

Add new resources

Remove obsolete ones

Then repeat:

terraform plan
terraform apply
Terraform will:

Detect drift or changes

Modify infrastructure incrementally

💣 6. Destroy (terraform destroy)
This command removes all resources managed by Terraform.

terraform destroy
Use it with caution. It:

Deletes everything described in .tfstate

Useful for cleaning up dev environments

You can also destroy a specific resource:

terraform destroy -target=aws_instance.web
🔐 Bonus: Lifecycle Meta-Arguments
Terraform lets you control the lifecycle behavior of resources using meta-arguments like:

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  lifecycle {
    create_before_destroy = true
    prevent_destroy       = true
    ignore_changes        = [tags]
  }
}
create_before_destroy: Useful for replacing instances without downtime

prevent_destroy: Blocks accidental deletions

ignore_changes: Ignores changes to specified attributes (like external tag updates)

🧩 Terraform State
Terraform keeps track of all managed resources in a state file:

terraform.tfstate (local)

Or remote (S3 + DynamoDB for team collaboration)

State is critical for determining deltas between desired and actual infrastructure.

# 📘 Summary of Terraform Workflow
Step	Command	Purpose
Write	—	Define infrastructure in HCL
Init	terraform init	Set up plugins, modules, backends
Plan	terraform plan	Show what changes will be made
Apply	terraform apply	Provision or update real infrastructure
Update	Modify .tf + Plan	Make changes safely
Destroy	terraform destroy	Clean up infrastructure

# Infrastructure as code?
in simple words provisioning / creating infrastructure with code

Infrastructure as code, or IaC, has gained a lot of momentum over the years because it helps solve several problems that trobeled infrastructure management in the past.

# IaC enabels
Reproducible Environments:

By using code to generate infrastructure, the same environment can be created over and over.
Over a perdiod of time an environment can drift away from its desired state and difficult to diagnose issues can creep into your release pipeline.
With IaC no environment gets special treatment and fresh new environments are easily created and destroyed.
Idempotence & Convergence:

Extending the last point, idempotence is the trait that no matter you apply the configuration described by your IaC, there are no side effects on the environment.
Convergence is the trait that actions are only taken if they need to be.
In IaC, only the actions needed to bring the environment to the desired state are executed. If the environment is already in the desired state, no actions are taken.
Easing collaboration:

Having the code in a version control system like Git allows teams to collaborate on infrastructure.
Team members can get specific versions of the code and create their own environments for testing or other scenarios.
Self-service infrastructure:

A pain point that often existed for developers before moving to cloud infrastructure was the delays required to have operations teams create the infrastructure they needed to build new features and tools.
With the elasticity of the cloud allowing resources to be created on-demand, developers can provision the infrastructure they need when they need it.
IaC further improves the situation by allowing developers to use infrastructure modules to create identical environments at any point in the application development lifecycle.
The infrastructure modules could be created by operations and shared with developers freeing developers from having to learn another skill.
How Terraform, providers and modules work
Terraform provisions, updates, and destroys infrastructure resources such as physical machines, VMs, network switches, containers, and more.

# Configurations
code written for Terraform, using the human-readable HashiCorp Configuration Language (HCL) to describe the desired state of infrastructure resources.

# Providers
The plugins that Terraform uses to manage those resources. Every supported service or infrastructure platform has a provider that defines which resources are available and performs API calls to manage those resources.

Modules
Reusable Terraform configurations that can be called and configured by other configurations. Most modules manage a few closely related resources from a single provider.

The Terraform Registry
makes it easy to use any provider or module. To use a provider or module from this registry, just add it to your configuration; when you run terraform init, Terraform will automatically download everything it needs.
