
# Github Actions Workshop

In this workshop we will utilize a tool called [super-linter](https://github.com/github/super-linter), a tool provided by github that is a combination of various linters to help validate our source code. We will use the [Closet Service](https://github.com/MensWearhouse/closet-service) repo to create our action and watch it run on our forked repository.

### Goals

 - High level understanding of Github Actions
 - Hands on implementation of an action
 - Knowledge of creating workflows and some best practices
 - Linting action implemented for closet service to help streamline code reviews

## Prerequisties

 - Fork [Closet Service](https://github.com/MensWearhouse/closet-service) 

## Table of Contents

1. [Step 1: Workflow Yaml File](#WorkflowYaml)
2. [Step 2: Naming Job](#NamingJob)
3. [Step 3:  Starting Job](#StartingJob)
4. [Step 4:  Load steps](#LoadSteps)
5. [Step 5: Checking out code](#Checking)
6. [Step 6: Running Linter](#RunningLinter)
7. [Step 7: Adding Env variables](#AddingEnv)
8. [Step 8: Opening your Pull Request](#OpeningPR)
9. [Step 9: Watching jobs in Github UI](#WatchingJobs)
10. [Recap](#Recap) 


## Step 1: Workflow Yaml 
<a name="WorkflowYaml"></a>
In your repository you should have a `.github/workflows`. In that directory, create `linter.yml`: `.github/workflows/linter.yml`
<img width="264" alt="Screen Shot 2022-08-09 at 1 54 57 PM" src="https://user-images.githubusercontent.com/7821732/183759543-b43fe1e4-9ec8-4733-a76b-9c1af7a499cf.png">

### Best Practices
- A workflow is a configurable automated process made up of one or more Jobs; this is represented as a single YAML file and is triggered by one or more Git Events
- Set the Workflow filename to the same as the Workflow name 
- The first 3 lines within the Workflow file should be commented lines with
   - A concise description outlining what the Workflow is running and when (trigger + branch)
   - The Author or List of Collaborators for this Workflow
   - The Current Version of the Workflow
## Step 2: Naming Job
<a name="NamingJob"></a>
## Step 3: Starting Job
<a name="StartingJob"></a>
## Step 4: Load Steps
<a name="LoadSteps"></a>
## Step 5: Checking out code
<a name="Checking"></a>
## Step 6: Running Linter 
<a name="RunningLinter"></a>
## Step 7: Adding Env variables
<a name="AddingEnv"></a>
## Step 8: Opening your Pull Request
<a name="OpeningPR"></a>
## Step 9: Watching jobs in Github UI
<a name="WatchingJobs"></a>
## Recap
