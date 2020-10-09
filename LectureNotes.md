# TO DO RIGHT BEFORE LAB

***Open the security group in AWS to 0.0.0.0/0***

# Welcome to Security Mountaineering Week 4: DevSecOps! 

## Intro
Today we are going to try to do a TON in 1 hour, so lets get going. There will be LOTS of hands-on today, so buckle up!
-	(5min) Hello/Attendance/Signup/Reminder of pathway objectives

## Prerequisites:
- If you’re going to participate today, you need a browser that’s off network. Bonus points if you want to use a git client on your machine too. 

## (5min) Refresher on last time
-	DevOps = mindset
-	...Empowerment, enablement, reliability, automation, end-to-end ownership
-	Helps to solve those big manual difficult release processes
-	Core tenets include frequent small changes
-	Often complimented by microservices, Git, and a sea of tools
-	...DevOps != a tool, set of tools, or procedure
-	Labs using Git:
-	...Commits, merges, working with remotes, and working with teams
-	CI/CD Vocab
-	...CI: Continuous Integration = merging to a central repo, running tests, and helping find bugs faster and closer to deployment time
-	...Continuous Delivery = publishes built/tested artifacts, ready to deploy. May deploy into an Acceptance Testing/UAT style environment and wait for validation.
-	...Continuous Deployment = all the above, but there’s no human gate before deploying to production. From git push to deployed into production may be a few minutes to perhaps an hour. 
-	IaC Overview
-	...Config Mgmt
-	...Resource Deployment/Bootstrap Tools

## (5min) CI-CD
-	We covered these concepts last week, but let’s dive deeper now that we did the git hands-on. 
-	Recall those merge conflict labs (learngitbranching.js.org - in the Remote Tab, Advanced section, lab 2 is a GREAT example) how difficult this can be even on a simple example. Now consider that in the legacy release environment all these commits might be done 6 months in the past, and you’ve worked/delivered dozens of features, and are working on something totally different today, but it’s time for the release. If there’s a problem, this can be a BIG issue to resolve. 
-	Enter the CI side of things. With your small changes that are frequently brought together with others’ work and frequently tested, you get fast feedback loops and continuous testing on your code. 
•	Changes automatically picked up & ran
•	We’ll dive into HOW to test your code more in 2 weeks when we go over PyLint, PyTest, etc. with Michael Lysaght

Handy Resources: 
- Want to see CI/CD explained in 100 seconds? https://www.youtube.com/watch?v=scEDHsr3APg
- How about a 35 minute walkthrough of Modules for later viewing? https://www.youtube.com/watch?v=7jnuTdhxjhw
- Creating a GitOps workflow with TF + Jenkins: https://www.hashicorp.com/resources/how-create-gitops-workflow-terraform-jenkins
- TF Modules Docs: https://www.terraform.io/docs/modules/index.html
- TF Learning: TF Modules Walkthrough: https://learn.hashicorp.com/tutorials/terraform/module-use


**POP QUIZ**
-	Who can name at least one tool we use for a git repository here at work?
-	Someone else: who can name one orchestrator/build tool we use here? 

o	Some examples here at work

	Jenkins + BitBucket

	Jenkins + BitBucket + Terraform Community

	Jenkins + BitBucket + Hashicorp Vault + TFE

	AWS CodeCommit/CodeBuild/CodePipeline

o	Since we’re all here to get to the Cloud safely, we’re going to use Terraform today. Specifically, we’ll use something very similar to what we have at work, since toady we’ll be using GitHub (in place of BitBucket), Jenkins, and Terraform Community. Secrets we won’t talk about. They’re secret. 

Quick note on GitHub: You're welcome to use GitHub Desktop, but realize that we don't have something like that internally, and it doesn't look like we will soon. I'd suggest not using it for this unless you're already really comfortable with git. That said, it would be helpful if at least one person on your team has some sort of client, whether that be GitHub Desktop, a terminal window, etc. 

Recall that Terraform is a bootstrap tool - it's not a CI or CD tool in and of itself, or a configuration management tool -- these would be more like your git repo, Jenkins, and Ansible, respectively. Terraform simply creates resources as they are declared in a file, or as we'll cover more today, a set of files. It's generally pretty vendor neutral, and that's why it's so darn popular. 
- AWS = CloudFormation Templates
- Azure = Resource Manager
- GCP = Deployment Manager
- Kubernetes = Helm Charts, but that's only the apps, that's not the servers underneath
- I suppose you could answer that with CoreOS, as it has a bootstrap tool to get OpenShift running, so you *could* use KVM, LIBVERT, RedHat ClusterManager, and a lot of effort to get that stood up to then throw a helm chart down on
- ORRRRRRRRR.... Just use Terraform to do that in those (and more) clouds, deploy your apps if desired, etc. It's pretty dope. That's why we use it, and that's what we're working on today. 

Before you get too worried -- Terraform is actually pretty straightforward. You'll see. Think of today more as a CI/CD class where instead of iterating on a dummy webapp we'll be iterating on some infrastructure code instead -- that way we learn that along the way. Neat!


### Open your browsers, and go to securitymountaineering.com

- You can find your credentials at the security sherpa site: *teams.md is the filename*. 
- Get logged in to all 3 systems (Jenkins, GitHub, AWS) and have the Sherpa GitHub up in another tab. Please don't mess about. 
- AWS: Whenever given an option to do so, please select us-east-2. That'll save me some time cleaning up later. Note: this is a personal AWS account, and work didn't send any AWS or iTunes gift cards my way, so kindly don't launch things that aren't in the free tier or totally free. 
- This will be a lighter weight use of Git -- if you want to use the CLI using these creds, PLEASE feel free! 
- you're on your own for merge conflicts ;) 
- You're intentionally in a team sharing one repo. It was easier than trying to make n logins for y teams and such. 
- What we have here is a basic jenkins instance. You even have a hello world. 

I've taken the liberty to go ahead and link this to your GitHub profiles, and I've got it set to run on commits. This is the first part of CI; we need to ensure that when changes are pushed to the repo (though you may happen to do them straight in the repo, this is still going from your browser to the repo). SO... while most times you'd instead set this to watch only protected branches, the master/main branch, or similar, here it's going to watch everything. 

Starting off, you have a hello world set up. Go ahead and elect one person on your team to make a change to the echo statement there to something else. Once you save that, your team should all be able to observe a new build kicking off. This is the beginning of a strong CI foundation. 

We're climbing the mountain that leads us into the cloud, so we also need to talk about Terraform. Now, it's a bit of a short training to learn CI/CD/GitOps/Terraform/Terraform Modules/security testing, so we're going to combine several things. We're going to iterate through some terraform changes and setup while using Jenkins in order to learn both at once. Any git issues that come up will be a great opportuntiy to practice your git scientist skills. Remember through all of this: this is *also* the beginning of Infrastructure as Code, Compliance as Code, etc.; since everything is stored in git and has historical state-in-time information, we can look back to see at any given moment what was configured how. 

## Lab 1:
- Create a new branch with your name as the branch name
- Click the .gitignore. *Explain gitignore, and how this one is set up for a TF project. You can have them in different directories too.*
- Click back, now click Add File -> Create new file. 
- ... One of you should create a file called dev.tfvars
- ... Another of you should create a file called main.tf
- ... the 3rd person in your group should take this moment to open the GitHub repo from the Sherpa - you'll need to open main.tf from there and copy the contents. Paste that into person 2's new file, then edit it -- *Please make the new name of the bucket have your team name in it, such as security-mountaineering-team5-lab-bucket1*
- ... the 4th person in your group should open the GitHub repo from the Sherpa, open Jenkinsfile, and copy those contents; you're about to put them into your team's Jenkinsfile. But YOU are going to do this right from master, with a twist. Go ahead and open/enter edit mode, and paste, but do _not_ commit/save. Feel free to prepare your commit message though ;)

Since we're talking about it, there's a little text box for a commit message, as well as an extended description. Get in the habit of using these, because when it's days or months later, you don't want to remember what it was or go through a git diff process. Do future you a solid. Use your commits. Plus, make sure your team can understand what you did! 

OK, so at this point, there's 3 branches - main/master, Ryan, Russell (or whatever the names are, and John is still waiting patiently on main to commit the jenkinsfile. FOR YOU - go ahead and switch the radio button to the second option, which will create a new branch and start a pull request. Hit Propose Changes button. 

Recall from 2 weeks ago in the Git Scientist section, pull requests are asking the main branch to kindly pull your changes into theirs. A nice message here helps them know what you're changing, and a good maintainer will of course vet your changes as they prepare to merge them. Go ahead and hit "Create pull request"

Now, you're all logged in as the one user/maintainer/admin of the github account, so this probably approximates someone that gets sidetracked more than represetinga team. that's Fine. it should still show no conflicts, so go ahead and hit "marge pull request." and merge it all in. You can even delete the branch, because now it's already in main, and there's no need to leave this dangling. 

Ok, the rest of the class should see that there's now warnings that some branch (will be your teammates' names) had recent pushes moments ago, so we want to compare and pull request those. Go ahead and each of you follow this workflow for yours. There should be no conflicts if we're following instructions. 

Once we're all done, this should leave us back with only a main branch and some shiny new files. How cool! 

The astute among you may have noticed that your Jenkins page is going a bit beserk. Indeed, it is. Let's all open the most recent build from your project. If it's prompting you to Proceed or Abort, go ahead and click Proceed. In fact, assume that you should always do this today, because aborting will leave our shetland Jenkins environment in a bit of a pigstye. If you did that all correctly and if nobody else's jobs are getting in the way, you may have also been successful at creating a bucket! 

- How did that get there? Well, I am doing a little magic behind the scenes and taking care of your AWS authentication. Secrets make *great* friends.

*How'd everyone do?*
- Are all your branches merged? 
- Are all your changes and logins working? 
- Did your runner go? If not, feel free to cancel it and give it another go
- Did you see anything about your bucket? ***Tell me what you notice about that bucket.***


## Lab 2
So that bucket is public read, which isn't always bad -- securitymountaineering.com is a public read bucket, but then again a website kinda needs to be able to be read. Let's assume this bucket is going to house some data that we'd rather not let be public. 

The first way we can iterate on and improve this is to go ahead and change that bucket ACL from public-read to private. 
- One person on your team should open main.tf, make this change, and commit. It's best practice to NOT make changes right on the main branch, so I recommend getting in the habit of making a new branch, albeit before the change or as part of the commit process. The risk to the second way is that you might accidentally commit right to master. 

**POP QUIZ** 
If you want to go back 1 commit and make that the new master: 
- what command would you use? 
- ... (could use git reset <commit SHA> for absolute || for relative: git reset current~1 for back 1 commit)
- What happens to the commit you were on? 
- ... Nothing, it stays there, one commit ahead of the master. Remember, git reset is somply *moving the pointer*. The files remain unchanged. 

Read more and see visuals here: 
https://opensource.com/article/18/6/git-reset-revert-rebase-commands



OK, There's another way you could do this - you could write a bucket policy, or can use some fancy terraform overrides. You can read about those here: 
- https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_policy
- https://github.com/terraform-aws-modules/terraform-aws-s3-bucket/blob/master/main.tf


But I think we should try to make something a little more reusable that's got that security built into it already. This is where Terraform Modules come in. Modules are just what they sound like: modular entities that are designed for you to make really reusable code that's more dynamic and sharable. 

***What’s a TF Module?***

•	Main.tf is actually a module; it’s the root module

•	It contains a collection of resources; you can create additional splat.tf files which also have collections of resources; those would be modules. 

•	It’s generally not a great idea to make a module per each concept (try not to use a wrapper; e.g.: here’s my t2.micro module, or even EC2 instance)

•	Typically you want to wrap a higher-level thing together, e.g.:

   o	Team Jenkins server + s3 bucket + subnet
    
   o	3-tier webapp
    
... but we're going to, because we don't have time to write a whole webapp today. Sometimes here at work, through, we do write narrowly-defined modules like this.

•	Most often contained in a folder called “modules”. Let’s say we have 2 modules to create: webservice and backend-service

ProjectDirectory
|- .gitignore
|- main.tf
|- readme.md
|-/modules/
      |-/webservice/
            |- main.tf
            |- vars.tf
            |- output.tf
            |- README.md (optional)
      |-/backend-service/
            |- main.tf
            |- vars.tf
            |- output.tf
            |- README.md (optional)
            
            
A few things that are a bit different about modules: 

•	Because they’re reusable and shared so much, you declare a terraform block at the top, then specify a required version (or minimum) so that you can ensure compatibility

•	Because you’re trying to build extensibility, you want the user to pass in specifics, so you’ll use variables as much as possible.
•	Variables get properly defined in vars.tf. 

•	If you need outputs, such as for instance needing to attach a webserver to an ALB, you’d need to use the outputs.tf file because the ALB needs to know what the instances are. You do this by specifying the output var. 

   o	In this example, you could create webserver with this between the TF block + the resource block: 
        output “webserver” {
        value = aws_instance.webserver
        description “my AWS instance for the webserver”
        }
        
BUT… that would be hard to debug and trace all of them. SO, you stick it in the outputs file

o	Access by adding in the root module
resource “aws_elb” “main”{
    instances = module.wills_webserver.instance.id
}

o	This is why TF Plan is such a big deal – it’s able to look at all of this and sort out what order things need to go in, how it’s going to do everything, what dependencies there are, etc. 

•	Call the module from the root module by using the keyword “module”

    o	The name assigned is whatever; the config arguments have only 1 required (source), then the rest are optional/as-needed. E.g.: 

module “ryans_webserver” {
    source = “../modules/webserver”
}

***Friendly Tips***
- Don’t forget to run terraform init every time you add/modify a child module
-	Can call module in a module, but TF recommends very flat – too much nesting introduces complexity + risk of infinite loop. 

There are also some other files to be aware of, and ensure that you don't distribute them as part of your module:

•	terraform.tfstate and terraform.tfstate.backup: These files contain your Terraform state, and are how Terraform keeps track of the relationship between your configuration and the infrastructure provisioned by it.

•	.terraform: This directory contains the modules and plugins used to provision your infrastructure. These files are specific to a specific instance of Terraform when provisioning infrastructure, not the configuration of the infrastructure defined in .tf files.

•	*.tfvars: Since module input variables are set via arguments to the module block in your configuration, you don't need to distribute any *.tfvars files with your module, unless you are also using it as a standalone Terraform configuration.


^^ This means add these to your .gitignore
Sample: https://github.com/github/gitignore/blob/master/Terraform.gitignore 


OK... Let's get back into some hands on. This next bit pretty much follows this link: 
https://learn.hashicorp.com/tutorials/terraform/module-create

There's a few things we won't do from that, but that's where this part comes from. 

1. Between each other in your team, ensure the following gets accomplished: 
- create ./modules/main.tf
- create ./modules/variables.tf
- create ./modules/outputs.tf
- edit the readme to state that this is a module for provisioning S3 buckets with some security built in.

**before you commit that, you may wish to get to step 3 ;)**

2. Note, you normally would see another level of directories; modules typically holds the set of modules. Users will copy those in, but for now we're just going to use one module, so we aren't too worried about that. 

3. Between each other in your team, ensure the following gets accomplished: 
- edit ./modules/main.tf to add the following: 

```
resource "aws_s3_bucket" "s3_bucket" {
  bucket = var.bucket_name
  acl    = "public-read"
  policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::${var.bucket_name}/*"
            ]
        }
    ]
}
EOF

  website {
    index_document = "index.html"
    error_document = "error.html"
  }

  tags = var.tags
}
```

*skip this code*
variable "bucket_name" {

  description = "Name of the s3 bucket. Must be unique."
  
  type = string
  
}

variable "tags" {
  description = "Tags to set on the bucket."
  type = map(string)
  default = {}
}

*end skip code*

- create ./modules/variables.tf to add the following: 

```
variable "bucket_name" {
  description = "Name of the s3 bucket. Must be unique."
  type = string
}

variable "tags" {
  description = "Tags to set on the bucket."
  type = map(string)
  default = {}
}
```

*skip this code*

variable "bucket_name" {
  description = "Name of the s3 bucket. Must be unique."
  type = string
}

variable "tags" {
  description = "Tags to set on the bucket."
  type = map(string)
  default = {}
}

- edit ./modules/outputs.tf to add the following

```
output "arn" {
  description = "ARN of the bucket"
  value       = aws_s3_bucket.s3_bucket.arn
}

output "name" {
  description = "Name (id) of the bucket"
  value       = aws_s3_bucket.s3_bucket.id
}

output "website_endpoint" {
  description = "Domain name of the bucket"
  value       = aws_s3_bucket.s3_bucket.website_endpoint
}
```



- edit the ROOT LEVEL /teamx/main.tf to have the following instead of what it had before.

```
provider "aws" {
  version = 3.9
  profile = "default"
  region  = "us-east-2"
}

variable "aws_access_key" {}
variable "aws_secret_key" {}

module "website_s3_bucket" {
  source = "./modules/"
  bucket_name = "test-bucket-student-12345"
    tags = {
    Terraform   = "true"
        Environment = "dev"
    }
  }
```

- Review and become familiar with each of the files you and your teammates just shared in your project together. 

**Thoughts**

- *main.tf* You may notice that there is no provider block in this configuration. When Terraform processes a module block, it will inherit the provider from the enclosing configuration. Because of this, we recommend that you do not include provider blocks in modules.
- *variables.tf* Just like the root module of your configuration, modules will define and use variables.


Variables within modules work almost exactly the same way that they do for the root module. When you run a Terraform command on your root configuration, there are various ways to set variable values, such as passing them on the commandline, or with a .tfvars file. When using a module, variables are set by passing arguments to the module in your configuration. You will set some of these variables when calling this module from your root module's main.tf.

Variables defined in modules that aren't given a default value are required, and so must be set whenever the module is used.

When creating a module, consider which resource arguments to expose to module end users as input variables. For example, you might decide to expose the index and error documents to end users of this module as variables, but not define a variable to set the ACL , since to host a website your bucket will need the ACL to be set to "public-read".

You should also consider which values to add as outputs, since outputs are the only supported way for users to get information about resources configured by the module.

## TFE / Sentinel

Terraform Enterprise (aka TFE) is pretty cool. Note how earlier you had those state files getting created and destroyed -- well, those are kinda the crux of Terraform. Normally you need to persist those somewhere super safe; letting the deployed state differ from the state file or letting the state file get lost/corrupted makes for a LOT of manual cleanup. It's *really* not fun. Some ways people persist their state files: 
- In their Jenkins run, they'd copy that file to some share internally, set so only Jenkins and that service account can modify it, and some sort of notation either pushed back to the repo, to the dev via email, in a database/config mgmt, or as a file with the state associating the build:TF-plan.out:statefile.
- Some places even take that DB thing to the next level, and consider this crucial to their CMDB. CIS CSC #1 is always the asset inventory, so this is a great way to associate those things, plus it would make IR and investigations easier. 
- Some will do that but move the storage to something like s3. This is nice because s3 makes it easy to enable version control on the bucket, prevent accidental deletion/require MFA deletion, etc. VERY common amongst enterprises. 
- TFE offers those options, PLUS they offer to store your state files natively in the tool. 

***but wait, there's more***

TFE is also where the TF init / TF plan / TF apply also occur. So, in Jenkins, you'd just zip up those artifacts and send them up to TFE, or you can have it monitor your git repo as well. This is really nice for taking that orchestration out of places like Jenkins, especially since their RBAC is really nice. 

***CALL NOW AND WE'LL DOUBLE YOUR OFFER!!!***

TFE also has a tool called Sentinel, which is used to perform security checks of the plan. They have a bunch of the tests written for you, they're easy to write yourself, they offer GREAT version control (store in your git repo, so this is very #gitops #iac #complianceAsCode), and it's easy to selectively apply policies where needed. Admins can set baslines, other people can add additional checks, etc. Further, checks can have varying levels of stringency -- allow, warn, hard-enforce, etc. 

All the logging for all of this + correlations/associations between these is provided in TFE, and they have an excellent API to interact with it all. 

I'll show you now some of this in my personal TF-Cloud account (which used to have Sentinel). 
