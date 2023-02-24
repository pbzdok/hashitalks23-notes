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
