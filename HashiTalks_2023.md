# HashiTalks 2023

## How Terraform Helps Us Catch Bad Guys at Sea

`[Terraform]`

Presented by Jonathann Zenou

Data and AI based predictions of operational risks for high sea transports.

![](./reference/Bad_Guys_at_Sea/1.jpg)

Before Terraform they used *SaltStack* with a Jenkins Job as a *Salt Master*.

![](./reference/Bad_Guys_at_Sea/3.png)

How does it work with Terraform?

![](./reference/Bad_Guys_at_Sea/4.png)

Model Creation with several GPU environments:
* Jenkins Pipeline, pod template, IAM Role
* precooked AMI
* EFS Storage
* Route53
* Monitor
* Data Source
* Clout Init

For project creation (as the next step) they used the Terraform official Github provider (can create repos, branches, protections, teams, webhooks). Pitfalls: Creation of many many resources and checking remote state, etc. takes very long for this type of application -> remedy: split up the terraform.

Micro Services & Provision:
* Kubernetes: eks, helm
* Monitoring: Kubernetes
* Autoscaling: Karpenting, Keda.sh 

Pain Points:
* Terraform changes need to be delegated to DevOps Team
* Lots of PRs to review

Conclusion:
* Way more stable with Terraform
* Infrastructure changes can be done faster
* Less copying and code clone (Terraform modules help here)
* Multi region is painless

---

## An Introduction to "Packering"

`[Packer]`

Presented by Tom Howarth

Packer is an open source products by HashiCorp, basically Terraform for Images. Configurable with JSON or HCL2.

No state files, unlike Terraform. Just runs scripts and no leftover.

HCP Packer allows you to create a Image Library, which can be in turn used by Terraform for builds which will use those images.

---

## The Ups and Downs of Maintaining a Terraform (Docker) Provider

`[Terraform]`

Presented by Martin Wentzel

Why do we need a Docker provider?
* Many companies still using on-premise and want to use docker containers IaC

Developing a Provider:
* Docker daemon is the backend
* The Docker provider acts as a client (via Docker client package)
  * This can be used to inspect containers etc.
* ! Versioning issues can appear between backend - frontend
* Why not using the Docker CLI to talk to the daemon: Mostly because of the added dependency (is the client pkg not??)

![](./reference/Maintaining_a_Terraform_Provider/1.png)

Testing & CI/CD:
* No need to mock anything because docker daemon are pre installed on github action runners (?)
* Testing with HTTPS, insecure HTTPS and HTTP registries
* Long running tests (14 minutes): Terraform matrix for all the ressources that run about 20 jobs in parallel (brought test time down to 9 minutes) 

Feature Development:
* Support for Podman (not easy, probably only unofficial)
* Support for Docker Compose (in works but very complex)

---

## Zero Trust Security with Boundary and Vault

`[Boundary] | [Vault]`

Presented by Japneet Sahni

Traditional Workflow for Priviledged Access Management:
* Operator SSH Key -> Connect to VPN / Bastion Host -> Firewall / IP based access -> Connects to mostly everything
  * Onboarding is difficult
  * Key rotations are difficult
  * After Bastion Host user has network access, increasing attack surface
  * Managing IPs is brittle in a dynamic environments
  * Application credentials are exposed to the user (such as long lived DB credentials)
* Number of challanges for Zero Trust security approach

![](./reference/Boundary_and_Vault/1.png)

How does Boundry-Vault integration comes to the rescue?
* Onboarding is easy as it is connected to an trusted identity provider
* Role based access controls with logic service with tags
* User has no network access but rather a selected host or service (not breaching into the network / zero trust network)
* Vault as secrets management tool can generate dynamic on-demand credentials

![](./reference/Boundary_and_Vault/2.png)


There are some paid features (Credential Injection) for Boundary-Vault integration.

![](./reference/Boundary_and_Vault/3.png)
![](./reference/Boundary_and_Vault/4.png)
![](./reference/Boundary_and_Vault/5.png)

Boundary Desktop as a end user solution to connect to Boundary services.

Potential relevant usecase: Accessing Linux servers with local accounts using ssh otp secret engine

![](./reference/Boundary_and_Vault/6.png)

Boundary and Vault can be configured in Terraform with HCL.

---

## Writing Your First Waypoint Deploy Plugin

`[Waypoint]`

Presented by Bram Vogelaar & Fokke Dekker

Seaplane is a multi region, multi cloud, edge running serivce provider using waypoint

Waypoint has basic 3 steps:
* Build step (Docker): Build image and push to container registry
* Deploy (Seaplane): Deploys the application and updates the application status
* Release

![](./reference/First_Waypoint_Deploy_Plugin/1.png)

Waypoint integration framework:
* Has barebone code set up for the 3 basic steps
* Create Waypoint.hcl and validate input
* Build and push image to our registry
* Deploy app to Seaplane 
* Verify the status
* Implement a destroy funtion to take down deployment

![](./reference/First_Waypoint_Deploy_Plugin/2.png)

Basically an ad on Seaplane and showing how to write some Go module that uses the Seaplane CLI

![](./reference/First_Waypoint_Deploy_Plugin/4.png)

---

## Scale Your Cloud Network to Infinity and Beyond

`[Terraform]`

Presented by Du'An Lightfoot 

![](./reference/Scale_to_Infinity_and_Beyond/1.png)

Multi VPC Network Infrastructure:
* VPC is a virtual network very similar to local networks but on AWS infrastructure
* Larger VPCs vs smaller VPCs

![](./reference/Scale_to_Infinity_and_Beyond/2.png)

VPC Peering:
* Point to point connection between VPCs, works intra network, intra region, same or different account
* Does not support transit routing

![](./reference/Scale_to_Infinity_and_Beyond/3.png)

![](./reference/Scale_to_Infinity_and_Beyond/4.png)

AWS Transit Gateway:
* Without you often need a full mesh for many VPCs, making it costly and cumbersome
* Transit gateway can take the role of a switch / router that manages all point to point VPC connections

![](./reference/Scale_to_Infinity_and_Beyond/5.png)

Terraform Overview:
* AWS provider comes battery included
* VPC ressoures can be created with HCL
* Modules are opinionated grouping of ressources, reusable and flexible

![](./reference/Scale_to_Infinity_and_Beyond/6.png)

![](./reference/Scale_to_Infinity_and_Beyond/7.png)

Demo ensues... complete overview of VPC and AWS Transit Gateway with Terraform in AWS Cloud9

---

## Terraform: Don't Reinvent the Modules

`[Terraform]`

Presented by Lays Rodrigues

What is Terraform? Quick introduction followed...

Time To Market:
* Plan
* Code
* Build
* Test
* Release
* Deploy

![](./reference/Dont_reinvent_Modules/1.png)

![](./reference/Dont_reinvent_Modules/2.png)

What Companies do:
* Many companies create a private repo with all of the Terraform modules

What Companies should do:
* Use Terraform AWS Modules
* Don't reinvent the wheel

![](./reference/Dont_reinvent_Modules/3.png)

![](./reference/Dont_reinvent_Modules/4.png)

Video of demo ensues showing the usage of AWS VPC module

The main point was that the usage of readily available modules is better since it follows the best practices and saves you from the most common pitfalls.

When should you create your own module?
* Corner cases

Talk was finished short

---

## Advanced Terraform Techniques

`[Terraform]`

Presented by John McDonough

Code Reuse: Git submodules
* Is a regular repository
* Added to the repository as a reference
* Subdirectory one or more level below root

![](./reference/Advanced_Terraform_Techniques/1.png)

![](./reference/Advanced_Terraform_Techniques/2.png)

![](./reference/Advanced_Terraform_Techniques/3.png)

Code Reuse: Terraform modules 
* Generic module + variable file (and maybe some defaults)
* Locals to define actual data that the module (vars) are called with

![](./reference/Advanced_Terraform_Techniques/4.png)

![](./reference/Advanced_Terraform_Techniques/5.png)

Data Driven Code:
* Using locals with abstract and generic resources takes away the complexity as it is the only real point of interest (especially for reusing)
* Maps and lists can atomate reoccuring resource creation

![](./reference/Advanced_Terraform_Techniques/6.png)

![](./reference/Advanced_Terraform_Techniques/7.png)

Don't use `count`! Use `for_each`!

![](./reference/Advanced_Terraform_Techniques/8.png)

![](./reference/Advanced_Terraform_Techniques/9.png)

![](./reference/Advanced_Terraform_Techniques/10.png)

Further presentation of dynamic block ressources was very hard to follow

Implicit Dependencies:
* Use implicit dependencies where ever possible by referencing the module directly

![](./reference/Advanced_Terraform_Techniques/11.png)

---

## Building Scalable Enterprise Secrets Management with GitHub OIDC and HashiCorp Vault

`[Vault]`

Presented by Ari Kalfus

Secret management is hard:
* The secret zero problem: Where do you store your secret storage system's login credentials

![](./reference/Enterprise_Secrets_Management/1.png)

Goal: Don't wind up on the news
* How do we mitigate catrastrophic impact from compromised environments
* Don't use GitHub secrets for managing secrets
* Use OIDC
  * Ephemeral to each workflow run
  * Lot more options than per-repo
  * Great auditability
  * Enables very short TTLs

What is OIDC:

![](./reference/Enterprise_Secrets_Management/2.png)

![](./reference/Enterprise_Secrets_Management/3.png)
  
Logging in with GitHub actions: Use `hashicorp/vault-action`

OIDC unlocks flexibility:
* Ephemeral least privilage RBAC, ABAC, PBAC
* Vault roles per use case (this creates a lot of vault roles)

![](./reference/Enterprise_Secrets_Management/4.png)

![](./reference/Enterprise_Secrets_Management/5.png)

![](./reference/Enterprise_Secrets_Management/6.png)

Enterprise Consumability:
* Reduce developer friction (make the secure path the easy path)
* Encapsulate complexity (does every dev need to understand jwt, claims and roles?)
  * At Digital Ocean: Command line wizard to request a new vault role for their projects.
* Oversight and auditing (audit via `aud` claim and `user` claim)

![](./reference/Enterprise_Secrets_Management/7.png)

![](./reference/Enterprise_Secrets_Management/8.png)

![](./reference/Enterprise_Secrets_Management/9.png)

Digital Ocean has open sourced a best-practices Terraform module for GitHub OIDC - Vault integration, hands-on course and soon a in-depth security article on Digital Ocean blog.


![](./reference/Enterprise_Secrets_Management/11.png)

---

## Vault and Boundary - Managing Secrets at Home

`[Boundary] | [Terraform] | [Vault]`

Presented by Michael Greenlaw

HashiPass:
* Password Manager
* Business use
* At home use
* Cheap
* Simple
* Scalable
* Secure
* Zero Trust

Walktrough of a simple secrets manager implementation. This although is not feasible as it needs a eks setup and quite some implementation

![](./reference/Managing_Secrets_at_Home/5.png)

![](./reference/Managing_Secrets_at_Home/6.png)

![](./reference/Managing_Secrets_at_Home/7.png)

We can subsitute Bastion with Boundary and handle everything in Terraform to make it easier to use and set up.

![](./reference/Managing_Secrets_at_Home/9.png)

Further iteration of a "well-worn path" of a high availablility dev environment

![](./reference/Managing_Secrets_at_Home/10.png)

![](./reference/Managing_Secrets_at_Home/11.png)

---

## Enhancing Platform Teams Workflow with Infrastructure as Code

`[Terraform]`

Presented by Sivamuthu Kumar

> Plattform engineering is the practice of building and maintaining the infrastructure, tools, and processes that support an organization's technology platform.

Key benefits of Terraform include:
* Declarative configuration language for defining infrastructure as code.
* Cross-platform support for managing infrastructure resources across different environments
* Modularity and reusability for managing and testing infrastructure changes Infrastructure graph for visualizing resource dependencies and preventing issues
* Plan and apply commands for testing and validating infrastructure changes before they are applied.
  
By leveraging Terraform, platform engineering teams can streamline their workflow, reduce errors, and increase the speed of infrastructure delivery.

They are using GitHub issue creation (with predefined form) to trigger a GitHub action that populates a Terraform module and triggers a plan -> Platform engineering team comes in an approves the issue. That will apply and provision it, which is again reported in the original issue.

![](./reference/Enhancing_Platform_Teams_Workflow/5.png)

---

## Secure Developer Workflows with Vault & Github Actions

`[Vault]`

Presented by Kartik Lunkad

![](./reference/Secure_Developer_Workflows/1.png)

Quick Takeaways:
* Security starts at the personal development environment
* Developer experience is the hack to adoption of secure practices
* Start with (just) good enough security is better than no security

Many bad data breaches are suggesting that credentials are the core source of security incidents, therefore secret management is a big focus point here.

![](./reference/Secure_Developer_Workflows/2.png)

Level 1 Secure App Workflow - Static Secrets into .NET App:
* OpenAPI based Vault .NET client library
* Authentication + secret retrieval in source code
* Demo ensues...

![](./reference/Secure_Developer_Workflows/4.png)

Level 2 Secure App Workflow - Dynamic DB Secret in your Go App
* OpenAPI based Vault Go client library
* Dynamic credentials retrieved to access database
* Demo ensues...

![](./reference/Secure_Developer_Workflows/5.png)

Level 3 Secure App Workflow - Secrets in GitHub Action Workflows
* Multiple authentication methods
* Vault GitHub Action to retrieve secrets from Vault
* Demo ensues...

![](./reference/Secure_Developer_Workflows/6.png)

---

## Building an Agnostic Security Solution with HashiCorp Vault

`[Terraform] | [Vault]`

Presented by Hillol Habib Un Nabi

Key Takeaways:
* Benefits of being agnostic in terms of security
* Managing secretes securely and seamlessly with HashiCorp Vault
* Dynamic Security Key management with HashiCorp Vault

Security agnostic Solution:
* Hence Security agnostic means a solution that is independent and compatible with many vendors in terms of security and authentication.

Why is it needed?
* Service disruption of cloud providers. Ready made solution cannot be ported easily to another provider.
* Pricing of cloud providers is ofter high
* Simplify the solution with one generic solution, rather having multiple slightly different solutions.
* Future Proofing

Background:
* Secrets everywhere in the DevOps CI/CD Pipleline

![](./reference/Agnostic_Security_Solution/4.png)

Solution: HashiCorp Vault
* Stores and manages sensitive environment variables, database credentials, API keys, etc
* Access is tightly controlled
* Its has UI,CLI or HTTP API interface

![](./reference/Agnostic_Security_Solution/5.png)

![](./reference/Agnostic_Security_Solution/6.png)

Vault Integration:

![](./reference/Agnostic_Security_Solution/7.png)

Workflow Management:
* Workflows in main repositories calls common action repo with vault secrets
* GitHub Action builds Python Docker image that then runs a script and provisions all things needed for Vault
* showing some pyhton code...

![](./reference/Agnostic_Security_Solution/8.png)

![](./reference/Agnostic_Security_Solution/10.png)

![](./reference/Agnostic_Security_Solution/12.png)

![](./reference/Agnostic_Security_Solution/13.png)

---

## When You're in Command of the Incident

`[Culture] | [Other]`

Presented by Martin Smith
  
Classic Incident Management: Expandable and adaptable

![](./reference/Incident_Management/1.png)

OODA: observe-orient-decide-act (or size-up, stabilize, update, verify)

![](./reference/Incident_Management/2.png)

Incident Commander is in full control of the entire company response and holds folks responsible

![](./reference/Incident_Management/3.png)

![](./reference/Incident_Management/4.png)

How can we improve the incident command?
* First responders need to consider themselfs as the first incident commander, until you hand it off
* Sometimes you are better at solving the problem, sometimes you might be better as the commander because of you relationships in the organisation
* Shadowing: Follow a more experienced commander / Are you shadow-able when being a experienced commander
* Communication is key, share you thoughts and involve people in the process: Keep talking - nobody explodes. This enables better documentation and knowledge sharing and takes away some stress from the responder as other can chime in and verify
* Retros are a great place to share your doubts as an experienced incident commander

![](./reference/Incident_Management/5.png)

![](./reference/Incident_Management/6.png)
  
Common challanges in incident command:
* No one responds to paging
  * Additional rotations for managers, well groomed escalation paths
  * Assistant Commanders
  * Refocus sprint work
  * Stop declaring incidents (if its happens always, like thats the state of the app just broken)
* Suggestions from powerfull players (like CEO's etc.)
  * Indicent Commander is the most powerful person in the company during the incident
  * This is hard but the commander needs to set the stage and make people aware
  * Incorporate them into the incident
  * Ask them to leave
* Too many threads
  * Swimlanes with objectives and deadlines
  * Seperate chat rooms or even ask folks to leave
  * Redirect async chatter to another medium
  * Designate coordinators or add more assistant commanders
* Incident fatigue
* Too many processes
  * Automate as much as possible

![](./reference/Incident_Management/8.png)

![](./reference/Incident_Management/9.png)

![](./reference/Incident_Management/10.png)

![](./reference/Incident_Management/11.png)

---

## Terraform for Platform Teams

`[Terraform]`

Presented by Issac Goldstand

Is DevOps dead? DevOps vs Plattform engineering:
* There has been so much effort in including DevOps to Developers, FullStack + Ops = You build it, you own it
  * Much money has also been spent paying those Devs to also include that responsibility
* In a real world that might not work out becuase of security constraints. Some resources / connections / networks can only be created by a designated plattform team in bigger companies
  * Division of resources that can be created / managed by DevOps or Plattform Engineers

Create a REST API to submit request for priviledged ressource creation between Platform Engineers and Devs. How does Terraform fit here?
* A Provider is the glue between the API and the Terraform code
* Creating an own Provider enables you to abstract away the authentication, priviledges access mechanisms and also hide that REST API

Why use a Terraform Provider and not just a module:
* A provider has a several aspects of separation of responsibilities that a module cannot do
* A module is automatically open source, also applies to module nesting. If you want to hide that you need providers, which is distributed in a binary format.
* The state file can be different for providers (The provider can orchestrate much more stuff that is also not bound to Terraform itself)

![](./reference/Terraform_for_Platform_Teams/1.png)

Do we have a Plattform team?

---

## Exploring and Provisioning Infrastructure with Packer

`[Packer]`

Presented by Andrei Buzoianu & Elif Samedin

What is Packer?
* One of the tools should not be missing from a DevOps Engineer
* Can build images for various platforms as Docker, VM, etc. from one configuration
* Focus on immutable infrastructure
* What does it try to solve?
  * Immutable infrastructure
  * Configuration drift
  * Deployment and Time to Market
  * Control over costs

![](./reference/Provisioning_Infrastructure_with_Packer/1.png)

Golden Images:
* Master Image / Base Image
* Contains software and configuration
* Increaeses image usability
* Avoid manually installing and configuring software

Automation does increase deployment speed and time to market, which in turn reduces costs and increases profitability

![](./reference/Provisioning_Infrastructure_with_Packer/5.png)

Packer Workflow:
* Takes a source
* Creates a machine on the builder
* Executes tasks defined by the Provisioner

![](./reference/Provisioning_Infrastructure_with_Packer/6.png)

Demo ensues...

---

## Containerized Infrastructure-as-Code with Immutable Clusters

`[Consul] | [Nomad] | [Terraform] | [Waypoint]`

Presented by Felipe Angelo Sgarbi

Traditional servers come with many pitfalls and much manual labor.

This led to a new perspective on servers and how to handle them.

![](./reference/Containerized_Infrastructure-as-Code/1.png)

Also the server OSs where re-imagined (example Fedora CoreOS).

![](./reference/Containerized_Infrastructure-as-Code/2.png)

CoreOS enables simple yaml file configuration, which greatly simplifies and improves the process.

Terraform can also be used with CoreOS to bootstrap a complete HashiStack ready for production:
* Terraform bootstraps the infrastructure with the proper configuration files for CoreS to know how to set-up everything, while CoreOS makes sure to atomically install and configure all the softwares

![](./reference/Containerized_Infrastructure-as-Code/3.png)

Few Gotachs with CoreOS:
* Single configuration only
* Persistent storage must be explicitly set up
* Changing the configuration isn't as straightforward after first boot (OSTree atomic updates as remedy)
* Server must reboot to apply updates

Why not the alternatives, such as ansible with common server?
* Different tools for different jobs
* Tools like Ansible, Chef and Puppet are Configuration Management Tools, which leverages configurations of multiple servers with already existing resources.
* Orchestration Tools such as Terraform ensure the environment is on a desired state continuously. If one server is not on the state it expects, it replaces it with one that is as expected.

![](./reference/Containerized_Infrastructure-as-Code/9.png)

Simplicity over Complex Power:
* Kubernetes is a costly power: Being a notoriously complex ecosystem,
Kubernetes tries to do everything out of the box at the cost of a way steeper learning curve.
* Hashistack simplifies this: Aiming for requiring way less configuration and being more simplistic, the HashiStack tends to be more specialized.

![](./reference/Containerized_Infrastructure-as-Code/10.png)

Immutability and consistency really shines with HashiStack.
