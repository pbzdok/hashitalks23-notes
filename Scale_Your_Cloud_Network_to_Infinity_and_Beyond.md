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
