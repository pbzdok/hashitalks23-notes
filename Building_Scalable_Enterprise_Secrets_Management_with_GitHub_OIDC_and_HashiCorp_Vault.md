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
