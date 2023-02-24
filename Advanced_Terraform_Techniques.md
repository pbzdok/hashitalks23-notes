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
