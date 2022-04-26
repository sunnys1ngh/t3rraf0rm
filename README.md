============================
# t3rraf0rm
Terraform Code - Sunny Singh
============================


 I aim to guide you through the components needed in order to successfully deploy Azure Infrastructure using Terraform via an Azure DevOps Pipeline.

 The idea here is to help understand how you may be able to automate the deployment and updating of your cloud infrastructure hosted in Azure.

 The basic idea behind Terraform , is that it enables the use of Infrastructure as Code (IaC) tooling in one language to deploy to multiple Cloud Platforms with ease, known as ‘Providers’ in Terraform. Terraform has hundreds of providers, with Azure being just one. 

 The ability to deploy, destroy, redeploy is made very simple with the use of a ‘tfstate’ file 
 
 This would enable Terraform to know the ‘state’ since the last deployment and only implement the changes implied by a code update. There is actually a feature in Terraform known as ‘PLAN’ that actually just reports the changes it will make before you ‘APPLY’ them.

Azure DevOps is a set of ‘technologies’ that enable you to improve your business productivity, reliability, scale and robustness if used correctly. 
DevOps is consists of 3 fundamental principals: People, Process and Technology.
Azure DevOps very much sits in the ‘Technology’ part of the triangle.

Azure DevOps gives us multiple ‘tooling’: Azure Boards - Azure Repos - Azure Pipelines - Azure test Plans - Azure Artifacts

=================================
I will be focusing on ‘Pipelines’
=================================

Together this tooling offers a set of features in order to automate the deployment of our infrastructure with checks based on a ‘Trigger’, thus giving us a great way to ensure that our code is Tested and deployed, within a workflow if necessary, and this provides an auditable, repeatable and reliable mechanism to avoid any human error etc.


My aim is to bring these four together - (GitHub, Azure, Terraform, and Azure DevOps) to enable us to start to design and automate infrastructure deployment and management into Azure.

We are going to look to deploy the following basic landing zone into our Azure Subscription.

1               – GitHub Repo
2               – Azure Subscription to contain the infrastructure we are going to deploy
3.1             – Terraform Code to deploy Azure Infrastructure from local machine (JONNYCHIPS-APP01):
                    Resource Group
                    Virtual Network
                    Virtual Machine
                    Storage Account
3.2             – Terraform Code to deploy Azure Infrastructure with a shared state file.
                    The items in Resource Group Jonnychipz-INFRA will need to be created outside of Terraform, within this article I will show the AZCLI commands to create:
                    Resource Group
                    Storage Account
                    Key Vault (With access key for Storage Account)
4               – Azure DevOps Organisation
5               – Azure DevOps Pipeline

Create Your GitHub Repo and clone to local machine

I have logged into github.com and created a simple repo with a README.md file, the repo is available at https://github.com/sunnys1ngh/t3rraf0rm

Please follow the following steps

1. Create a repo on Github.
2. Select Clone "Clone or download" on Github, copy the link
3. In Visual Studio Code do [ctrl '] for Terminal Window
4. Type [git --version]
5. CD to Parent Directory
6. In Terminal Window, Type...
7. [git config --global [user.name](http://user.name/) <github userID>]
8. [git config --global user.email <github email address>]
9. Check Config [git config --global --list]
10. [git clone <URL from github link copied earlier>]
11. That should be all that's required. any newly created file should be available on github after stage/commit/push.


Make sure you have access to an Azure Subscription that can be utilised to deploy infrastructure into.

Within VSCode Terminal browse to your newly cloned Repo folder on your local machine and type code . (don’t forget the fullstop!)

This opens up our working folder in VS Code.

Next type ‘az login‘

This will take you off to an OAUTH web page for you to enter your Azure Credentials to authenticate your terminal session. Remember at this stage we are going to authenticate our local machine so we can test our Terraform code before we look to deploy it using Azure DevOps Pipelines.

You will be presented with your subscription in JSON format, if you have multiple subscriptions then you will need to set the context to the subscription you would like to use. We can use either the Subscription ID, or the Subscription name.

To set Context from my subscription I type: ‘az account set —subscription “MPN – John Lunn”‘

Now lets get on with our Terraform Code, I will keep this deployment simple by storing all my configuration in a single ‘main.tf’ file, so right click your open folder and create a new file called ‘main.tf‘

We will first run a ‘Terraform init‘ which initialises the backend.....

Now we will check to see what Terraform is going to do in our Azure Environment by running a ‘Terraform plan‘ 
This output is longer than this screen shot, but this is the start:

You can see that the ‘PLAN’ command outputs to screen what we are about to do in our environment, this being to add 6 items in my case.

So lets see if we can deploy successfully from our local machine. To do this we run the ‘Terraform apply‘ command. This will take a couple of minutes to run through, but at the end you should find you have all the resources in the resource group as we expected!





