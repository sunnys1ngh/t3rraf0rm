# t3rraf0rm
Terraform Code - Sunny Singh

 I aim to guide you through the components needed in order to successfully deploy Azure Infrastructure using Terraform via an Azure DevOps Pipeline.

 The idea here is to help understand how you may be able to automate the deployment and updating of your cloud infrastructure hosted in Azure.

 The basic idea behind Terraform , is that it enables the use of Infrastructure as Code (IaC) tooling in one language to deploy to multiple Cloud Platforms with ease, known as ‘Providers’ in Terraform. Terraform has hundreds of providers, with Azure being just one. 

 The ability to deploy, destroy, redeploy is made very simple with the use of a ‘tfstate’ file 
 
 This would enable Terraform to know the ‘state’ since the last deployment and only implement the changes implied by a code update. There is actually a feature in Terraform known as ‘PLAN’ that actually just reports the changes it will make before you ‘APPLY’ them.

Azure DevOps is a set of ‘technologies’ that enable you to improve your business productivity, reliability, scale and robustness if used correctly. 
DevOps is consists of 3 fundamental principals: People, Process and Technology.
Azure DevOps very much sits in the ‘Technology’ part of the triangle.

Azure DevOps gives us multiple ‘tooling’: Azure Boards - Azure Repos - Azure Pipelines - Azure test Plans - Azure Artifacts


