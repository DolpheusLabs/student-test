**Intro**
***Go to securitymountaineering.com***
- You have your credentials. Get logged in to both systems. Please don't mess about. 
- This will be a lighter weight use of Git -- if you want to use the CLI using these creds, PLEASE feel free! 
- you're on your own for merge conflicts ;) 
- You're intentionally in a team sharing one repo. It was easier than trying to make n logins for y teams and such. 
- What we have here is a basic jenkins instance. You even have a hello world. 

I've taken the liberty to go ahead and link this to your GitHub profiles, and I've got it set to run on commits. This is the first part of CI; we need to ensure that when changes are pushed to the repo (though you may happen to do them straight in the repo, this is still going from your browser to the repo). SO... while most times you'd instead set this to watch only protected branches, the master/main branch, or similar, here it's going to watch everything. 

Starting off, you have a hello world set up. Go ahead and elect one person on your team to make a change to the echo statement there to something else. Once you save that, your team should all be able to observe a new build kicking off. This is the beginning of a strong CI foundation. 

We're climbing the mountain that leads us into the cloud, so we also need to talk about Terraform. Now, it's a bit of a short training to learn CI/CD/GitOps/Terraform/Terraform Modules/security testing, so we're going to combine several things. We're going to iterate through some terraform changes and setup while using Jenkins in order to learn both at once. Any git issues that come up will be a great opportuntiy to practice your git scientist skills. Remember through all of this: this is *also* the beginning of Infrastructure as Code, Compliance as Code, etc.; since everything is stored in git and has historical state-in-time information, we can look back to see at any given moment what was configured how. 

**Lab 1: 
- new branch - dev.tfvars - main.tf - copy/paste contents of dolpheus/dev/lab1/main.tf
