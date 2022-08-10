
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
12 [Secuirty Considerations](#Security)


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
In this next part, we will define the `steps` that belong to the `job`. Usually the first step is to checkout the code from the repo. A step is an individual task within a Job; this may be a Public Action, a Custom Action, or a Run Step

`steps` is the next key down from `build` and is a list usually with the following keys (among others): `name`, `uses`, `with`

In `lintCodeBase.yml` under `jobs -> steps` add the following to checkout the codebase:

<img width="916" alt="Screen Shot 2022-08-09 at 2 45 36 PM" src="https://user-images.githubusercontent.com/7821732/183767363-c6fad692-b324-47ee-8758-7a22cb49f7cd.png">

### Best Pracitces
- For Step Names you can have spaces but keep it simple and avoid the use of non-alpha characters
- For Step Ids use kebab-case

## Step 6: Running Linter 
<a name="RunningLinter"></a>

The second "step" in our `steps` section is to actually run the linter. Under `steps` in `lintCodeBase.yml` add a new list following the code checkout step as seen here:

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

Note: we configured this job in step 3 to trigger `on` a pr and a merge. So after a merge it will run again

## Step 9: Watching jobs in Github UI
<a name="WatchingJobs"></a>

## Step 10: Add Badge to repo
<a name="badge"></a>
## Recap
<a name="Recap"></a>
## Security Considerations
<a name="Security"></a>

#### Where does the Job actually run?

GitHub Action jobs by default run on Virtual Machines managed by GitHub themselves. These are referred to as *GitHub-hosted Runners*.

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

