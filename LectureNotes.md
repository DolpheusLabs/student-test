**Intro**
Today we are going to try to do a TON in 1 hour, so lets get going. There will be LOTS of hands-on today, so buckle up!
-	(5min) Hello/Attendance/Signup/Reminder of pathway objectives

**Prerequisites:**
- If you’re going to participate today, you need a browser that’s off network. Bonus points if you want to use a git client on your machine too. 

**(5min) Refresher on last time**
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

**(xxxmin) CI-CD**
-	We covered these concepts last week, but let’s dive deeper now that we did the git hands-on. 
-	Recall those merge conflict labs (learngitbranching.js.org - in the Remote Tab, Advanced section, lab 2 is a GREAT example) how difficult this can be even on a simple example. Now consider that in the legacy release environment all these commits might be done 6 months in the past, and you’ve worked/delivered dozens of features, and are working on something totally different today, but it’s time for the release. If there’s a problem, this can be a BIG issue to resolve. 
-	Enter the CI side of things. With your small changes that are frequently brought together with others’ work and frequently tested, you get fast feedback loops and continuous testing on your code. 
•	Changes automatically picked up & ran
•	We’ll dive into HOW to test your code more in 2 weeks when we go over PyLint, PyTest, etc. with Michael Lysaght

** POP QUIZ **
-	Who can name at least one tool we use for a git repository here at work?
-	Someone else: who can name one orchestrator/build tool we use here? 

o	Some examples at Citi
	Jenkins + BitBucket
	Jenkins + BitBucket + Terraform Community
	Jenkins + BitBucket + Hashicorp Vault + TFE
	AWS CodeCommit/CodeBuild/CodePipeline
o	Since we’re all here to get to the Cloud safely, we’re going to use Terraform today. Specifically, we’ll use something very similar to what we have at work, since we’ll use GitHub (in place of BitBucket), Jenkins, and Terraform Community. Secrets we won’t talk about. They’re secret. 


***Go to securitymountaineering.com***

- You can find your credentials at the security sherpa site: *teams.md is the filename*. 
- Get logged in to all 3 systems (Jenkins, GitHub, AWS) and have the Sherpa GitHub up in another tab. Please don't mess about. 
- This will be a lighter weight use of Git -- if you want to use the CLI using these creds, PLEASE feel free! 
- you're on your own for merge conflicts ;) 
- You're intentionally in a team sharing one repo. It was easier than trying to make n logins for y teams and such. 
- What we have here is a basic jenkins instance. You even have a hello world. 

I've taken the liberty to go ahead and link this to your GitHub profiles, and I've got it set to run on commits. This is the first part of CI; we need to ensure that when changes are pushed to the repo (though you may happen to do them straight in the repo, this is still going from your browser to the repo). SO... while most times you'd instead set this to watch only protected branches, the master/main branch, or similar, here it's going to watch everything. 

Starting off, you have a hello world set up. Go ahead and elect one person on your team to make a change to the echo statement there to something else. Once you save that, your team should all be able to observe a new build kicking off. This is the beginning of a strong CI foundation. 

We're climbing the mountain that leads us into the cloud, so we also need to talk about Terraform. Now, it's a bit of a short training to learn CI/CD/GitOps/Terraform/Terraform Modules/security testing, so we're going to combine several things. We're going to iterate through some terraform changes and setup while using Jenkins in order to learn both at once. Any git issues that come up will be a great opportuntiy to practice your git scientist skills. Remember through all of this: this is *also* the beginning of Infrastructure as Code, Compliance as Code, etc.; since everything is stored in git and has historical state-in-time information, we can look back to see at any given moment what was configured how. 

**Lab 1: 
- new branch - dev.tfvars - main.tf - copy/paste contents of dolpheus/dev/lab1/main.tf
