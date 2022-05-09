## Blogs
### [AWS SaaS isolation Strategies](https://d1.awsstatic.com/whitepapers/saas-tenant-isolation-strategies.pdf)
* Pool: Compute shares but storage are siloed
* Silo: Cell based architecture. Example - AWS accoint level, VPC level, Subnet, 
* Hybrid: Monilithic web tier, rest are siloed. 

The general rule of thumb here is to look at any resource and assess the noisy neighbor and security profile for that resource. You’ll have to weigh these isolation options against the added complexities, cost, operational burden, and service limits that may come with adopting a silo strategy for some part of your system. Finding the right balance is often key to the success of your system.

## Design Patterns and questions 
* https://github.com/donnemartin/system-design-primer

## Technology 
### JWT
### TLS 
