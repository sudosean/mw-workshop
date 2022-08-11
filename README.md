
# Github Actions Workshop

In this workshop we will utilize a tool called [super-linter](https://github.com/github/super-linter), a tool provided by github that is a combination of various linters to help validate our source code. We will use the [Closet Service](https://github.com/MensWearhouse/closet-service) repo to create our action and watch it run on our forked repository.

### Goals

 - High level understanding of Github Actions
 - Hands on implementation of an action
 - Knowledge of creating workflows and some best practices
 - Linting action implemented for closet service to help streamline code reviews


## Prerequisties

 - Fork [Closet Service](https://github.com/MensWearhouse/closet-service) 

## Introduction
GitHub Actions gives developers the ability to automate their workflows across issues, pull requests, and more—plus native CI/CD functionality.

GitHub Actions is a CI/CD tool for the GitHub flow. You can use it to
integrate and deploy code changes to a third-party cloud application
platform as well as test, track, and manage code changes. GitHub Actions
also supports third-party CI/CD tools, the container platform Docker, and
other automation platforms.

**Why having a GitHub-native CI/CD tool is helpful**: The most basic
answer is simplicity—if you’re already hosting a project on GitHub, you
have a built-in CI/CD tool that works right alongside your code.

## Table of Contents

1. [Step 1: Workflow Yaml File](#WorkflowYaml)
2. [Step 2: Naming Workflow](#NamingWorkflow)
3. [Step 3:  Starting Job](#StartingJob)
4. [Step 4:  Load Steps](#LoadSteps)
5. [Step 5: Checking out code](#Checking)
6. [Step 6: Running Linter](#RunningLinter)
7. [Step 7: Adding Env variables](#AddingEnv)
8. [Step 8: Opening your Pull Request](#OpeningPR)
9. [Step 9: Watching Jobs in Github UI](#WatchingJobs)
10. [Step 10: Add Badge to repo](#badge)
11. [Recap](#Recap) 
12. [Secuirty Considerations](#Security)


## Step 1: Workflow Yaml 
<a name="WorkflowYaml"></a>
In your repository you should have a `.github/workflows`. In that directory, create `lint-code-base.yml`: `.github/workflows/lint-code-base.yml`

<img width="264" alt="Screen Shot 2022-08-09 at 1 54 57 PM" src="https://user-images.githubusercontent.com/7821732/183759543-b43fe1e4-9ec8-4733-a76b-9c1af7a499cf.png">

### Best Practices
- A workflow is a configurable automated process made up of one or more Jobs; this is represented as a single YAML file and is triggered by one or more Git Events
- Set the Workflow filename to the same as the Workflow name 
- The first 3 lines within the Workflow file should be commented lines with
   - A concise description outlining what the Workflow is running and when (trigger + branch)
   - The Author or List of Collaborators for this Workflow
   - The Current Version of the Workflow
  <img width="668" alt="Screen Shot 2022-08-09 at 2 04 41 PM" src="https://user-images.githubusercontent.com/7821732/183761174-79da2d33-79aa-4347-b82d-33fecde7b03f.png">

## Step 2: Naming Workflow
<a name="NamingWorkflow"></a>
Create the name of the Workflow in `lint-code-base.yml` by defining the `name:` key and set it to `lint-code-base`: `name: lint-code-base`

<img width="644" alt="Screen Shot 2022-08-11 at 12 16 40 PM" src="https://user-images.githubusercontent.com/7821732/184221309-33eff08c-5673-4398-8463-fe03805228fe.png">


### Best Practices
- For Workflow name use kebab-case
  - this should be a unique name across all Workflows for a particular repository
  - the name should relate to the trigger event and the branch, such as ‘push-feature-branch’
  - as stated above this will also be the Workflow file basename

## Step 3: Starting Job
<a name="StartingJob"></a>
A Job is event based meaning it is triggered `on` some event. A Git event, such as ‘pull request opened', or ‘push’; this may or may not be further defined to one or more specific branches

Tell github how to start the Job in `lint-code-base.yml` by defining the `on` block which follows the `name` key

<img width="575" alt="Screen Shot 2022-08-09 at 2 12 15 PM" src="https://user-images.githubusercontent.com/7821732/183762351-38736cc1-b843-4f11-803a-30d2c309a71f.png">

### Best Practices
- There should be only one Action Workflow per Git Event + Branch type, there are no issues with having multiple branch / multiple Git events in a single workflow,

## Step 4: Load Steps
<a name="LoadSteps"></a>
A Job is a collection of sequential Steps to run that should perform a discrete piece of work in the CICD pipeline (example, Lint, Build, Test, Deploy, etc.)

Create a Job using the `Jobs` key in `lint-code-base.yml` following the `on` event defined in step 3: 

<img width="292" alt="Screen Shot 2022-08-09 at 2 26 00 PM" src="https://user-images.githubusercontent.com/7821732/183764445-546df892-6d3e-4b0c-8194-6d30db1c2de5.png">


### Best Practices
- Each Job should be related to a single discrete piece of work in the CICD pipeline; for example, in a Workflow called ‘pull-request-open-on-main’, there may be the following Jobs: 
   - build-for-staging 
   - deploy-to-staging 
   - integration-tests 
   - performance-tests 
- For Job names use kebab-case
  - Job names need not be unique across all Workflows, but must be unique within the same Workflow

## Step 5: Checking out code
<a name="Checking"></a>
In this next part, we will define the `Steps` that belong to the `Job`. Usually the first step is to checkout the code from the repo. A step is an individual task within a Job; this may be a Public Action, a Custom Action, or a Run Step

`Steps` is the next key down from `build` and is a list usually with the following keys (among others): `name`, `uses`, `with`

In `lint-code-base.yml` under `Jobs -> Steps` add the following to checkout the codebase:

<img width="916" alt="Screen Shot 2022-08-09 at 2 45 36 PM" src="https://user-images.githubusercontent.com/7821732/183767363-c6fad692-b324-47ee-8758-7a22cb49f7cd.png">

NOTE: step ids are used to reference the step in contexts if needed

### Best Pracitces
- For Step Names you can have spaces but keep it simple and avoid the use of non-alpha characters
- For Step Ids use kebab-case

## Step 6: Running Linter 
<a name="RunningLinter"></a>

The second "Step" in our `Steps` section is to actually run the linter. Under `Steps` in `lint-code-base.yml` add a new list following the code checkout step as seen here:

<img width="468" alt="Screen Shot 2022-08-09 at 2 48 53 PM" src="https://user-images.githubusercontent.com/7821732/183767873-9259f28c-2505-4bfc-8d9d-96251714a7e6.png">

**NOTE**: **There is no need to set the GitHub Secret as it is automatically set by GitHub, it only needs to be passed to the action**. If you pass the Environment variable GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} in your workflow, then the GitHub Super-Linter will mark the status of each individual linter run in the Checks section of a pull request. Without this you will only see the overall status of the full run. 
### Best Practices
- Same as step 5

## Step 7: Adding Env variables
<a name="AddingEnv"></a>

Env vars can be scoped to the Workflow level, Job level and Step level. Because super linter comes with a myraid of programming languages, we want to set java to true at the `step` level. We also want to exclude generated files, files ignored by git and scope down linting to the `src` file. Super linter provides a number of env vars for this step so please check documentation for reference.

Add the following env vars to the `env` block of the step 6:

<img width="487" alt="Screen Shot 2022-08-09 at 3 15 42 PM" src="https://user-images.githubusercontent.com/7821732/183771404-9bad1d81-13cd-4ddb-b85a-68d553e63745.png">

Note: All the VALIDATE_[LANGUAGE] variables behave in a very specific way:
- If none of them are passed, then they all default to true.
- If any one of the variables are set to true, we default to leaving any unset variable to false (only validate those languages).
- If any one of the variables are set to false, we default to leaving any unset variable to true (only exclude those languages).
- If there are VALIDATE_[LANGUAGE] variables set to both true and false. It will fail.

### Best Practices
- Again, Env vars can be scoped to the Workflow level, Job level and Step level
- Use environment variables within the narrowest scope possible
- Be aware that if you create new environment variables and append them to the $GITHUB_ENV variable then they will be available to all subsequent Steps within that same Job
- For Environment Variables use SNAKE_CASE (All Caps)

## Step 8: Opening your Pull Request
<a name="OpeningPR"></a>
For this github action to run, it will need to go through a pr process. Open a pr and you should see the new "check" running at the bottom of the pr. Then, head to the next step to see the linter run in the worklfow!

Note: We configured this job in step 3 to trigger `on` a pr and a merge (of non main branches). 

## Step 9: Watching jobs in Github UI
<a name="WatchingJobs"></a>
There are a few ways to fiew the workflow running. Either on the pr, under the "Checks" section or you can click on the actions tab at the top of the repo and find your workflow running there

Click the "Actions" tab at the top of the repo
<img width="848" alt="Screen Shot 2022-08-10 at 2 09 21 PM" src="https://user-images.githubusercontent.com/7821732/184020581-4024fe06-1652-402a-80fa-66cd33a43cc5.png">

Click the "lint code base" tab under "Workflows"
<img width="285" alt="Screen Shot 2022-08-10 at 2 13 24 PM" src="https://user-images.githubusercontent.com/7821732/184021267-ba5273be-db9c-4d62-95d4-655c0f0ee229.png">

Click into the workflow at the top

## Step 10: Add Badge to repo
<a name="badge"></a>
Click Actions at the top, then pick the workflow you want to add a badge for. Click on the three dots to the right of the search bar at the top and click "create status badge"

<img width="983" alt="Screen Shot 2022-08-10 at 2 16 00 PM" src="https://user-images.githubusercontent.com/7821732/184021684-90188c7c-4b71-442e-a937-61acba2b0279.png">

You will see a modal similar to this:

<img width="648" alt="Screen Shot 2022-08-11 at 12 05 40 PM" src="https://user-images.githubusercontent.com/7821732/184219528-05d0f0b2-4e7c-4c66-a548-c9dec92823dc.png">

Choose the branch you want to display the badge for (usually main or master) and leave Event set to Default.

Copy the Markdown provided and paste it at the top of your readme

For more info on badges in repos please see [this documentation](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge)

## Recap
<a name="Recap"></a>

In this workshop we created a github action using super linter. We created the Worflow, defined the Job and Steps to do durning that Job. You can see that in a few lines of yaml you can begin to build an entire CI process for your project. You can even do deployments right from GitHub Actions and there is an entire [Marketplace](https://github.com/marketplace?type=actions) with predefined actions all ready to use for your project! 

Keep an eye on the best practices documentation found [here](https://wiki.tailoredbrands.com/pages/viewpage.action?spaceKey=WW&title=Github+Actions+Best+Practices)

## Security Considerations
<a name="Security"></a>

#### Where does the Job actually run?

GitHub Action Jobs by default run on Virtual Machines managed by GitHub themselves. These are referred to as *GitHub-hosted Runners*.

The runners are located within GitHub's own Microsoft Azure account and can access any resource that is public facing. For those resources that are not publicly accessible, due to security policies or the like, then there is the option to use a 'self-hosted' runner.  
These are virtual machines that are built, hosted and maintained by your own company, and are generally located within the environment / infrastructure that they need to operate within.  

For example, you may have a 'build & deliver' Job that updates a Kubernetes Cluster inside your own Data Centre.  
Using a GitHub-hosted runner would require the networking team to open up the Kubernetes Cluster to the public Internet, something that would violate the company security policies.  
So instead you would build a VM with the requisite software and tools, host this VM from within the Data Centre, and then using the Agent software (the software used by the runners to talk back to the GitHub Services), join the VM to the relevant repository that you would like to use that runner on.  
If desired, you can instead register the VM at the Organisation level with GitHub Enterprise.

Links of interest:

- [Runner Images](https://github.com/actions/runner-images): this repository has the scripts that are used by GitHub to create their own runners, and with minimal effort you can use these scripts yourself to create your 'self-hosted' runner VMs
- [GitHub-hosted Runners information](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)

#### Secrets inside the Workflow

Just like you wouldn't hardcode sensitive information inside your application code, neither should you hardcode sensitive information inside your Workflows.  
One solution is to use GitHub's inbuilt mechanism to securely inject secrets directly into your Workspaces.  
You only need to create the Secret, either at the Organization level or at the Repository level, then reference the secret via an automatically created environment variable, which the Workflow will have access to.  
This secret environment variable is masked in all logs produced but it is available to the Steps / Actions you choose to expose the Secret to, so you do need to be careful as to how you pass your secret on.  

Using the GitHub Secret is a convenient way to inject Secrets into your workflow.  
Another solution you can use is to access other Secret Services, such as Hashicorp's Vault service.  
The Vault Service even has its own Action available in the Marketplace which minimizes the work required.

Links of interest:

- [GitHub Encrypted Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [Vault Service](https://www.vaultproject.io/)
- [Vault Action](https://github.com/hashicorp/vault-action)

