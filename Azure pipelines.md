# [Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)

# [1 What is Azure Pipelines?](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)

Azure Pipelines automatically builds and tests code projects to make them available to others. It works with just about any language or project type. Azure Pipelines combines continuous integration (CI) and continuous delivery (CD) to test and build your code and ship it to any target.

Continuous Integration (CI) is the practice used by development teams of automating merging and testing code. Implementing CI helps to catch bugs early in the development cycle, which makes them less expensive to fix. Automated tests execute as part of the CI process to ensure quality. Artifacts are produced  from CI systems and fed to release processes to drive frequent deployments. The Build service in Azure DevOps Server helps you set up and manage CI for your applications.

Continuous Delivery (CD) is a process by which code is built, tested, and deployed to one or more test and production environments. Deploying and testing in multiple environments increases quality. CI systems produce deployable artifacts, including infrastructure and apps. Automated release processes consume these artifacts to release new versions and fixes to existing systems. Monitoring and alerting systems run continually to drive visibility into the entire CD process.

Continuous Testing (CT) on-premises or in the cloud is the use of automated build-deploy-test workflows, with a choice of technologies and frameworks, that test your changes continuously in a fast, scalable, and efficient manner.

#### Version control systems

The starting point for configuring CI and CD for your applications is to have your source code in a version control system. Azure DevOps supports two forms of version control - GitHub and Azure Repos. Any changes you push to your version control repository will be automatically built and validated.

#### Languages

You can use many languages with Azure Pipelines, including Python, Java, JavaScript, PHP, Ruby, C#, C++, and Go.

#### Application types

You can use Azure Pipelines with most application types, such as Java, JavaScript, Node.js, Python, .NET, C++, Go, PHP, and XCode.

Azure DevOps has a number of tasks to build and test your application. For example, tasks exist to build .NET, Java, Node, Android, XCode, and C++ applications. Similarly, these are tasks to run tests using a number of testing frameworks and services. You can also run command line, PowerShell, or Shell scripts in your automation.

Deployment targets

Continuous testing

Package formats

What do I need to use Azure Pipelines?

Why should I use Azure Pipelines?

Next steps

# 2 Use Azure Pipelines

Automate tests, builds, and delivery

Define pipelines using YAML syntax

Define pipelines using the Classic interface

Feature availability

Next steps



# 3 Get started

## [3-1 Sign up for Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)



## [3-2 Create your first pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=java%2Ctfs-2018-2%2Cbrowser)



## [3-3 Customize your pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops)

### Prerequisite

### Understand the `azure-pipelines.yml` file

### Change the platform to build on

### Add steps

### Build across multiple platforms

### Build using multiple versions

### Customize CI triggers

### Pipeline settings

### Create work item on failure

### Next steps



## [3-4 Multi-stage pipelines user experience](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops)



Navigating pipelines

Pipelines landing page

View pipeline details

View pipeline run details

Manage security

Task insights for failed pipeline runs

Next steps

# 4 Pipeline basics

## [4-1 Key concepts](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops)

**Azure DevOps Services**

Learn about the key concepts and components that make up a pipeline. Understanding the basic terms and parts of a pipeline can help you deliver better code more efficiently and reliably.

**Key concepts overview**

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/key-concepts-overview.svg?view=azure-devops)

* A trigger tells a Pipeline to run.
* A pipeline is made up of one or more stages. A pipeline can deploy to one or more environments.
* A stage is a way of organizing jobs in a pipeline and each stage can have one or more jobs.
* Each job runs on one agent. A job can also be agentless.
* Each agent runs a job that contains one or more steps.
* A step can be a task or script and is the smallest building block of a pipeline.
* A task is a pre-packaged script that performs an action, such as invoking a REST API or publishing a build artifact.
* An artifact is a collection of files or packages published by a run.

**Azure Pipelines terms**

#### Agent

When your build or deployment runs, the system begins one or more jobs. An agent is computing infrastructure with installed agent software that runs one job at a time. For example, your job could run on a Microsoft-hosted Ubuntu agent.

For more in-depth information about the different types of agents and how to use them, see [Azure Pipelines Agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops).

#### Approvals

[Approvals](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/approvals?view=azure-devops) define a set of validations required before a deployment runs. Manual approval is a common check performed to control deployments to production environments. When checks are configured on an environment, pipelines will stop before starting a stage that deployed to the environment until all the checks are completed successfully.

#### Artifact

An artifact is a collection of files or packages published by a run. Artifacts are made available to subsequent tasks, such as distribution or deployment. For more information, see [Artifacts in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/artifacts/artifacts-overview?view=azure-devops).

#### Continuous delivery

Continuous delivery (CD) is a process by which code is built, tested, and deployed to one or more test and production stages. Deploying and testing in multiple stages helps drive quality. Continuous integration systems product deployable artifacts, which include infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fixes to existing systems. Monitoring and alerting systems run constantly to drive visibility into the entire CD process. This process ensures that errors are caught often and early.

#### Continuous integration

Continuous integration (CI) is the practice used by development teams to simplify the testing and building of code. CI helps to catch bugs or problems early in the development cycle, which makes them easier and faster to fix. Automated tests and builds are run as part of the CI process. The process can run on a set schedule, whenever code is pushed, or both. Items known as artifacts are produced from CI systems. They're used by the continuous delivery release pipelines to drive automatic deployments.

#### Deployment

For Classic pipelines, a deployment is the action of running the tasks for one stage, which can include running automated tests, deploying build artifacts, and any other actions are specified for that stage.

For YAML pipelines, a deployment typically refers to a deployment job. A deployment job is a collection of steps that are run sequentially against an environment. You can use strategies like run once, and canary for deployment jobs.

#### Deployment group

A deployment group is a set of deployment target machines that have agents installed. A deployment group is just another grouping of agents, like an [agent pool](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops). You can set the deployment targets in a pipeline for a job using a deployment group. Learn more about provisioning agents for [deployment groups](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/deployment-groups/howto-provision-deployment-group-agents?view=azure-devops).

#### Environment

An environment is a collection of resources, where you deploy your application. It can contain one or more virtual machines, containers, web apps, or any service that's used to host the application being developed. A pipeline might deploy the app to one or more environments after build is completed and tests are run.

#### Job

A stage contains one or more jobs. Each job runs on an agent. A job represents an execution boundary of a set of steps. All of the steps run together on the same agent. Jobs are most useful when you want to run a series of steps in different environments. For example, you might want to build two configurations - x86 and x64. In this case, you have one stage and two jobs. One job would be for x86 and the other job would be for x64.

#### Pipeline

A pipeline defines the continuous integration and deployment process for your app. It's made up of one or more stages. It can be thought of as a workflow that defines how your test, build, and deployment steps are run.

#### Release

For Classic pipelines, a release is a versioned set of artifacts specified in a pipeline. The release includes a snapshot of all the information required to carry out all the tasks and actions in the release pipeline, such as stages, tasks, policies such as triggers and approvers, and deployment options. You can create a release manually, with a deployment trigger, or with the REST API.

For YAML pipelines, the build and release stages are in one, multi-stage pipeline.

#### Run

A run represents one execution of a pipeline. It collects the logs associated with running the steps and the results of running tests. During a run, Azure Pipelines with first process the pipeline and then send the run to one or more agents. Each agent will run jobs. Learn more about the pipeline run sequence.

#### Script

A script runs code as a step in your pipeline using command line, PowerShell, or Bash. You can write cross-platform scripts for macOS, Linux, and Windows. Unlike a task, a script is custom code that is specific to your pipeline.

#### Stage

A [stage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/stages?view=azure-devops) is a logical boundary in the pipeline. It can be used to mark separation of concerns (for example, Build, QA, and production). Each stage contains one or more jobs. When you define multiple stages in a pipeline, by default, they run one after the other. You can specify the conditions for when a stage runs. When you are thinking about whether you need a stage, ask yourself:

* Do separate groups manage different parts of this pipeline? For example, you could have a test manager that manages the jobs that relate to testing and a different manager that manages jobs related to production deployment. In this case, it makes sense to have separate stages for testing and production.
* Is there a set of approvals that are connected to a specific job or set of jobs? If so, you can use stages to break your jobs into logical groups that require approvals.
* Are there jobs that need to run a long time? If you have part of your pipeline that will have an extended run time, it makes sense to divide them into their own stage.

#### Step

A step is the smallest building block of a pipeline. For example, a pipeline might consist of build and test steps. A step can either be a script or a task. A task is simply a pre-created script offered as a convenience to you. To view the available tasks, see the [Build and release tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/?view=azure-devops) reference. For information on creating custom tasks, see [Create a custom task](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-build-task?view=azure-devops).

#### Task

A task is the building block for defining automation in a pipeline. A task is packaged script or procedure that has been abstracted with a set of inputs.

#### Trigger

A trigger is something that's set up to tell the pipeline when to run. You can configure a pipeline to run upon a push to a repository, at scheduled times, or upon the completion of another build. All of these actions are known as triggers. For more information, see [build triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops) and [release triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops).

#### Library

The Library includes **secure files** and **variable groups**. Secure files are a way to store files and share them across pipelines. You may need to save a file at the DevOps level and then use it during build or deployment. In that case, you can save the file within Library and use it when you need it. Variable groups store values and secrets that you might want to be passed into a YAML pipeline or make available across multiple pipelines.

## [4-2 YAML pipeline editor](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/yaml-pipeline-editor?view=azure-devops)

Azure Pipelines provides a YAML pipeline editor that you can use to author and edit your pipelines. The YAML editor is based on the [Monaco Editor](https://github.com/microsoft/monaco-editor). The editor provides tools like Intellisense[^4-2-1] support and a task assistant to provide guidance while you edit a pipeline.

### Edit a YAML pipeline

To access the YAML pipeline editor, do the following steps.

1. Sign in to your organization (`http://dev.azure.com/{yourorganization}`).

2. Select your project, choose **Pipelines**, and then select the pipeline you want to edit. You can browse pipelines by **Recent**, **All**, and **Runs**. For more information, see [Pipelines landing page](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops#pipelines-landing-page).

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-landing-page.png?view=azure-devops)

3. Choose **Edit**.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-edit.png?view=azure-devops)

4. Make edits to your pipeline using [Intellisense](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/yaml-pipeline-editor?view=azure-devops#use-keyboard-shortcuts) and the [task assistant](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/yaml-pipeline-editor?view=azure-devops#use-task-assistant) for guidance.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-editor.png?view=azure-devops)

5. Choose **Save**. You can commit directly to your branch, or create a new branch and optionally start a pull request.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-save.png?view=azure-devops)

#### Use keyboard shortcuts

The YAML pipeline editor provides several keyboard shortcuts, which we show in the following examples.

* Choose **Ctrl+Space** for Intellisense support while you're editing the YAML pipeline.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-intellisense.png?view=azure-devops)

* Choose **F1** (**Fn**+**F1** on Mac) to display the command palette[^4-2-2] and view the available keyboard shortcuts.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-editor-command-palette.png?view=azure-devops)

### Use task assistant

The task assistant provides a method for adding tasks to your YAML pipeline.

* To display the task assistant, edit your YAML pipeline and choose **Show assistant**.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-task-assistant-show.png?view=azure-devops)

* To hide the task assistant, choose **Hide assistant**.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-task-assistant-hide.png?view=azure-devops)

* To use the task assistant, browse or search for tasks in the **Tasks** pane.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-task-assistant-search.png?view=azure-devops)

* Select the desired task and configure its inputs.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-task-assistant-add.png?view=azure-devops)

* Choose **Add** to insert the task YAML into your pipeline.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-task-assistant-task-added.png?view=azure-devops)

* You can edit the YAML to make more configuration changes to the task, or you can choose **Settings** above the task in the YAML pipeline editor to configure the inserted task in the task assistant.

### Validate

Validate your changes to catch syntax errors in your pipeline that prevent it from starting. Choose **More actions** > **Validate**.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-validate.png?view=azure-devops)

### Download full YAML

You can preview the fully parsed YAML document without committing or running the pipeline. Choose **More actions** > **Download full YAML**.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-validate.png?view=azure-devops)

**Download full YAML** [Runs](https://docs.microsoft.com/en-us/rest/api/azure/devops/pipelines/runs/run-pipeline?view=azure-devops-rest-6.1) the Azure DevOps REST API for Azure Pipelines and initiates a download of the rendered YAML from the editor.

### Manage pipeline variables

You can manage pipeline variables both from within your YAML pipeline and from the pipeline settings UI.

#### YAML pipeline

To manage pipeline variables, do the following steps.

1. Edit your YAML pipeline and choose Variables to manage pipeline variables.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-variables-button.png?view=azure-devops)

2. Choose from the following functions:

   * **New variable**: to add your first variable
   * **Add :heavy_plus_sign:**: to add subsequent variables.
   * *Variable name* to edit a variable.
   * Delete : to delete a variable.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-editor-manage-variables.png?view=azure-devops)

#### Pipeline settings UI

To manage pipelines variables in the UI, do the following steps.

1. Edit the pipeline and choose **More actions** > **Triggers**.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-settings-ui-menu.png?view=azure-devops)

2. Choose **Variables**.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-settings-ui-variables.png?view=azure-devops)

For more information on working with pipeline variables, see [Define variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops).

### View and edit templates

Templates are a commonly used feature in YAML pipelines. They're easy way to share pipeline snippets and are a powerful mechanism for verifying and enforcing security and governance in your pipeline. Previously, the editor didn't support templates, so authors of YAML pipelines couldn't get intellisense assistance. Now Azure Pipelines supports a YAML editor, for which we're previewing support. To enable this preview, go to preview features in your Azure DevOps organization, and enable YAML templates editor.

> **:red_circle:Important**
>
> This feature is in preview. There are known limitations. If the template has required parameters that aren't provided as inputs in the main YAML file, then the validation fails and prompts you to provide those inputs. In addition, you can't create a new template from the editor. You can only use or edit existing templates.

As you edit your main Azure Pipelines YAML file, you can either *include* or *extend* a template. As you enter the name of your template, you may be prompted to validate your template. Once validated, the YAML editor understands the schema of the template, including the input parameters.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/yaml-pipeline-editor/yaml-pipeline-editor-templates.png?view=azure-devops)

Post validation, you can go into the template by choosing **View template**, which opens the template in a new browser tab. You can make changes to the template using all the features of the YAML editor.

### Next steps

[Customize your pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops)

## 4-3 Repositories



## 4-4 Build history

## 4-5 Triggers

### [4-5-1 Type of triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops)





### [4-5-2 Scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml)



### [4-5-3 Pipeline completion triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?view=azure-devops)



### [4-5-4 Build completion triggers (classic)](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers-classic?view=azure-devops)



### [4-5-5 Release triggers (classic)](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops)



## 4-6 Tasks & templates



## 4-7 Jobs & stages



## 4-8 Library, variables & secure files



## 4-9 Approvals, checks, & gates



## 4-10 Pipeline runs



## 4-11 Pipeline reports

## 4-12 Manage pipelines with Azure CLI

## 4-13 Clone or import a pipeline

## 4-14 Configure pipelines to support work tracking

## 4-15 Pipeline default branch

## 4-16 DevOps Starter

# Ecosystems & integration

Build apps

Run pipeline tests

Deploy apps

Deploy apps (Classic)

Deploy to Azure

Deploy apps using containers

Consume & publish artifacts

Create & use resources

Manage agents & agent pools

Configure security & settings

Integrate with 

[^4-2-1]: n.智能感知
[^4-2-2]: [ˈpælɪt] n.调色板，颜料 a thin curved board that an artist uses to mix paints, holding it by putting his or her thumb through a hole at the edge