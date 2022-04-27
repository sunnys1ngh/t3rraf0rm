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

I will be focusing on ‘Pipelines’

Together this tooling offers a set of features in order to automate the deployment of our infrastructure with checks based on a ‘Trigger’, thus giving us a great way to ensure that our code is Tested and deployed, within a workflow if necessary, and this provides an auditable, repeatable and reliable mechanism to avoid any human error etc.


My aim is to bring these four together - (GitHub, Azure, Terraform, and Azure DevOps) to enable us to start to design and automate infrastructure deployment and management into Azure.

We are going to look to deploy the following basic landing zone into our Azure Subscription.

1               – GitHub Repo
2               – Azure Subscription to contain the infrastructure we are going to deploy
3.1             – Terraform Code to deploy Azure Infrastructure from local machine (sunnysingh-APP01):
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

To set Context from my subscription I type: ‘az account set —subscription “MPN – Sunny Singh”‘

Now lets get on with our Terraform Code, I will keep this deployment simple by storing all my configuration in a single ‘main.tf’ file, so right click your open folder and create a new file called ‘main.tf‘

We will first run a ‘Terraform init‘ which initialises the backend.....

Now we will check to see what Terraform is going to do in our Azure Environment by running a ‘Terraform plan‘ 
This output is longer than this screen shot, but this is the start:

You can see that the ‘PLAN’ command outputs to screen what we are about to do in our environment, this being to add 6 items in my case.

So lets see if we can deploy successfully from our local machine. To do this we run the ‘Terraform apply‘ command. This will take a couple of minutes to run through, but at the end you should find you have all the resources in the resource group as we expected!

So now we know that our Terraform code is working that is perfect. 
However, when we ran the Terraform Apply, a few new files were created in our local folder:

pay close attention to the ‘terraform.tfstate’ file. This file is where all the current configuration, that has been deployed into your subscription. This is the file that is used to compare any differences in your Terraform code and your main.tf file….. so the point here is, that currently, I would have to use my local PC all the time in order to deploy changes to our environment. This is fine if I have a small environment that is personal to me, or just for testing, but if I want to collaborate, or in fact use a service like Azure DevOps Pipelines to run my commands, I really need to store this state file in a location that is available to all, as well as securely store the credentials and update my Terraform code appropriately!

That’s what we are going to look at in the next section. So we can use the ‘Terraform DESTROY‘ command to remove all the infrastructure from our subscription, so we can look at moving our state file to a centralised area.

Terraform Code to deploy Azure Infrastructure with a shared state file.

The next step in this process is to separate the Terraform State file off into a centralised location such as an Azure Storage account.

This brings with it a few points we should be aware off.

The storage account needs to be created before Terraform code is applied (we will use a bash script for this as a one off activity)
Terraform accesses this Storage account via a Shared Secret key. This needs to be protected and not held in a script or certainly not stored in a Git Repository, we will explore options for storing this Key.

access_key: 1NE2Sa3lPmzYEJ/gb4Pl/1r3C70AijIMzetf97Ac0CDyNZ/qy1xrvcyDNbPoV4IavbMRYk0NTaCOO3Ei42XwLQ==~

If we check our Azure subscription, we can now see we have the storage account and container created ready to store our Terraform State file:

Configure state backend – Terraform

Great, so our next task is to modify our main.tf Terraform script to enable Terraform to utilise this shared state location and accessing it via the Key Vault by configuring what is known as the ‘state back end’. As mentioned previously, we could put the Storage Account access key directly into our Terraform file, but clearly that is not best practise as we will be storing out main.tf in a Git Repo so that would be a security concern so hence the Key Vault!

For now, until we move into the Azure DevOps Pipelines we will create the backend config with the raw Access Key, to demonstrate we just need to add the following to our terraform main.tf file to allow terraform to stare the state file in a certral location, our Azure Storage Account.

If we now run Terraform init and Terraform plan, we should see that we successfully create a Plan:

And in fact, our state file now does not exist locally, if we check the container of our storage account in Azure, here it is!

Success!

Make sure you commit and push your changes to your GitHub Repo, for the purposes of this part, 

I have included a ‘.gitignore’ file, in order to ensure no files such as the the Terraform Provider EXE is uploaded into Github.

Ok, so the next part of our journey now that we have successfully deployed our infrastructure using a shared location for our Terraform State, is to look to automat this based on the checking trigger to the ‘main’ GitHub repo branch.

We also need to ensure we remove the Storage Account Access Key as part of the following process.

The first thing we need to do is set ourselves up with an Azure DevOps Organization by visiting the following site:

sunnysingh-dev and Terraform-Testing are the Organisation and Project I have created for the purposes of this article.

5 – Azure DevOps Pipeline

The first thing we are going to do is create a service principle name (SPN) to allow our Azure DevOps Organisation project to deploy our environment.

Within our Azure DevOps Project we can select Project Settings -> Service Connections:

Click Create Service Connection -> Azure Resource Manager -> Next:

Then Select Service principal (automatic) -> Next:

These are the scope settings for my SPN that were used:

sunnysingh-dev and Terraform-Testing are the Organisation and Project I have created for the purposes of this article.

The first thing we are going to do is create a service principle name (SPN) to allow our Azure DevOps Organisation project to deploy our environment.

Within our Azure DevOps Project we can select Project Settings -> Service Connections:

Click Create Service Connection -> Azure Resource Manager -> Next:

Then Select Service principal (automatic) -> Next:

Scope settings for my SPN

You can confirm configuration of your SPN by reviewing the following output:

Allow this SPN COntributer access to my Sub

With all that in place its now time to create our pipeline

Select Pipelines -> Create Pipeline:

Select GitHub:

 I will use the classic editor as it saves a little processing of YAML files if you are not familiar with those.

Select Github and logon:

Logon to your GitHub Account:

Scroll down to Repository Access and select your repo, then click Approve and install:

This will authorise ADO to access your GitHub Repo, next we want to select Github:

For the purposes of this article we will set up a single stage to our pipeline which will run through :

Install Terraform
Run Terraform init
Run Terraform plan
Run Terraform validate & apply to deploy our infrastructure to our subscription.
The trigger for this pipeline run will be the commit of code to our ‘main’ branch in our repo.

We will select an Empty Pipeline to start creating our pipeline:

We are then presented with a pipeline to start building:

Next we want to select each Task and configure as follows.

Install Terraform:

Terraform: INIT

In this task, we can configure the Terraform Backend that we have in our main.tf as follows:

Terraform: PLAN

Terraform VALIDATE & APPLY

Once we have completed that configuration we can save it and we are presented by a pipeline yet to be run:


Once we have completed that configuration we can save it and we are presented by a pipeline yet to be run:


So, before we run our pipeline we could manually kick this off by selecting:

However, in the spirit of CI/CD we can modify the CI enabled flag on our pipeline

Now when we modify our code and commit it to our ‘master’ branch in GitHub this Pipeline should run and deploy our environment for us………

I commit a change via VS Code:

And Push to my GitHub Repo:

We wait for all our tasks to complete (and hope we dont have any errors!)

Our Job has completed successfully.

If we check our Azure Subscription, we can see that our Application infrastructure has been deployed as we would expect!

The concept that the pipeline is actually created as a file with a .yaml extension leads to even more interesting concepts

