
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
10. [Step 10: Add Badge to repo](#badge)
11. [Recap](#Recap) 


## Step 1: Workflow Yaml 
<a name="WorkflowYaml"></a>
In your repository you should have a `.github/workflows`. In that directory, create `lintCodeBase.yml`: `.github/workflows/lintCodeBase.yml`

<img width="264" alt="Screen Shot 2022-08-09 at 1 54 57 PM" src="https://user-images.githubusercontent.com/7821732/183759543-b43fe1e4-9ec8-4733-a76b-9c1af7a499cf.png">

### Best Practices
- A workflow is a configurable automated process made up of one or more Jobs; this is represented as a single YAML file and is triggered by one or more Git Events
- Set the Workflow filename to the same as the Workflow name 
- The first 3 lines within the Workflow file should be commented lines with
   - A concise description outlining what the Workflow is running and when (trigger + branch)
   - The Author or List of Collaborators for this Workflow
   - The Current Version of the Workflow
  <img width="668" alt="Screen Shot 2022-08-09 at 2 04 41 PM" src="https://user-images.githubusercontent.com/7821732/183761174-79da2d33-79aa-4347-b82d-33fecde7b03f.png">

## Step 2: Naming Job
<a name="NamingJob"></a>
Create the name of the job in `lintCodeBase.yml` by defining the `name:` key and set it to `Lint Code Base`: `name: Lint Code Base`
<img width="671" alt="Screen Shot 2022-08-09 at 2 06 47 PM" src="https://user-images.githubusercontent.com/7821732/183761514-b8d308c6-a69e-49ce-931a-3824362858bd.png">

### Best Practices
- For Workflow name use kebab-case
  - this should be a unique name across all Workflows for a particular repository
  - the name should relate to the trigger event and the branch, such as ‘push-feature-branch’
  - as stated above this will also be the Workflow file basename

## Step 3: Starting Job
<a name="StartingJob"></a>
A Job is event based meaning it is triggered `on` some event. A Git event, such as ‘pull request opened', or ‘push’; this may or may not be further defined to one or more specific branches

Tell github how to start the Job in `lintCodeBase.yml` by defining the `on` block which follows the `name` key

<img width="575" alt="Screen Shot 2022-08-09 at 2 12 15 PM" src="https://user-images.githubusercontent.com/7821732/183762351-38736cc1-b843-4f11-803a-30d2c309a71f.png">

### Best Practices
- For each major Git Event (related to CICD, such as PR open and Push) and each branch (or set of branches as deemed appropriate) there should be exactly one Action Workflow

## Step 4: Load Steps
<a name="LoadSteps"></a>
A Job is a collection of sequential Steps to run that should perform a discrete piece of work in the CICD pipeline (example, Lint, Build, Test, Deploy, etc.)

Create a job using the `jobs` key in `lintCodeBase.yml` following the `on` event defined in step 3: 

<img width="292" alt="Screen Shot 2022-08-09 at 2 26 00 PM" src="https://user-images.githubusercontent.com/7821732/183764445-546df892-6d3e-4b0c-8194-6d30db1c2de5.png">


### Best Practices
- Each Job should be related to a single discrete piece of work in the CICD pipeline; for example, in a Workflow called ‘pull-request-open-on-main’, there may be the following Jobs: 
   - build-for-staging 
   - deploy-to-staging 
   - integration-tests 
   - performance-tests 
- For Job names use kebab-case
  - job names need not be unique across all Workflows, but must be unique within the same Workflow

 
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
## Step 10: Add Badge to repo
<a name="badge"></a>
## Recap
<a name="Recap"></a>
