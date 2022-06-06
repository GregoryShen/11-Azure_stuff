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

This is a step-by-step guide on common ways to customize your pipeline.

### Prerequisite

Follow instructions in <u>Create your first pipeline</u> to create a working pipeline.

### Understand the `azure-pipelines.yml` file

A pipeline is defined using a YAML file in your repo. Usually, this file is named `azure-pipelines.yml` and is located at the root of your repo.

Navigate to the Pipelines page in Azure Pipelines, select the pipeline you created, and choose Edit in the context menu of the pipeline to open the YAML editor for the pipelien.

> :information_source:Note
>
> For instructions on how to view and manage your pipelines in the Azure DevOps portal, see [Navigating pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops)

Examine the contents of the YAML file.

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'
  
steps
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.11'
    jdkArchitectureOption: 'x64'
    publishJUnitResults: false
    testResultsFiles: '**/surefire-reports/TEST-*.xml'
    goals: 'package'
```

This pipeline runs whenever your team pushes a change to the main branch of your repo or creates a pull request. It runs on a Microsoft-hosted Linux machine. The pipeline process has a single step, which is to run the Maven task.

### Change the platform to build on

You can build your project on Microsoft-hosted agents that already include SDKs and tools for various development languages. Or, you can use self-hosted agents with specific tools that you need.

* Navigate to the editor for your pipeline by selecting **Edit pipeline** action on the build, or by selecting **Edit** from the pipeline's main page.

* Currently the pipeline runs on a Linux agent:

  ```yaml
  pool:
    vmImage: "ubuntu-latest"
  ```

* To choose a different platform like Windows or Mac, change the `vmImage` value:

  ```yaml
  pool:
    vmImage: "windows-latest"
  ```

  ```yaml
  pool:
    vmImage: "macos-latest"
  ```

* Select **Save** and then confirm the changes to see your pipeline run on a different platform.

### Add steps

You can add more scripts or tasks as steps to your pipeline. A task is a pre-packaged script. You can use tasks for building, testing, publishing, or deploying your app. For Java, the Maven task we used handles testing and publishing results, however, you can use a task to publish code coverage results too.

* Open the YAML editor for your pipeline.

* Add the following snippet to the end of  your YAML file.

  ```yaml
  - task: PublishCodeCoverageResults@1
    inputs:
      codeCoverageTool: "JaCoCo"
      summaryFileLocation: "$(System.DefaultWorkingDirectory)/**/site/jacoco/jacoco.xml"
      reportDirectory: "$(System.DefaultWorkingDirectory)/**/site/jacoco"
      failIfCoverageEmpty: true
  ```

* Select **Save** and then confirm the changes.

* You can view your test and code coverage results by selecting your build and going to the **Test** and **Coverage** tabs.

### Build across multiple platforms

You can build and test your project on multiple platforms. One way to do it is with `strategy` and `matrix`. You can use variables to conveniently put data into various parts of a pipeline. For this example, we'll use a variable to pass in the name of the image we want to use.

* In your `azure-pipelines.yml` file, replace this content:

  ```yaml
  pool:
    vmImage: "ubuntun-latest"
  ```

  with the following content:

  ```yaml
  strategy:
    matrix:
      linux:
        imageName: "ubuntu-latest"
      mac:
        imageName: "macOS-latest"
      windows:
        imageName: "windows-latest"
      maxParallel: 3
      
  pool:
    vmImage: $(imageName)
  ```

* Select Save and then confirm the changes to see your build run up to three jobs on three different platforms.

> Each agent can run only one job at a time. To run multiple jobs in parallel you must configure multiple agents. You also need sufficient parallel jobs.

### Build using multiple versions

To build a project using different versions of that language, you can use a `matrix` of versions and a variable. In this step, you can either build the Java project with two different versions of Java on a single platform or run different versions of Java on different platforms.

> :information_desk_person:: You can use `strategy` multiple times in a context.

* If you want to build on a single platform and multiple versions, add the following matrix to your `azure-pipelines.yml` file before the Maven task and after the `vmImage`.

  ```yaml
  strategy:
    matrix:
      jdk10:
        jdkVersion: "1.10"
      jdk11:
        jdkVersion: "1.11"
      maxParallel: 2
  ```

* Then replace this line in your maven task:

  ```yaml
  jdkVersionOption: "1.11"
  ```

  with this line:

  ```yaml
  jdkVersionOption: $(jdkVersion)
  ```

* Make sure to change the `$(imageName)` variable back to the platform of your choice.

* If you want to build on multiple platforms and versions, replace the entire content in your `azure-pipelines.yml` file before the publishing task with the following snippet:

  ```yaml
  trigger:
  - main
  
  strategy:
    matrix:
      jdk10_linux:
        imageName: "ubuntu-latest"
        jdkVersion: "1.10"
      jdk11_windows:
        imageName: "windows-latest"
        jdkVersion: "1.11"
     maxParallel: 2
     
  pool:
    vmImage: $(imageName)
    
  steps:
  - task: Maven@3
    inputs:
      mavenPomFile: "pom.xml"
      mavenOptions: "-Xmx3072m"
      javaHomeOption: "JDKVersion"
      jdkVersionOption: $(jdkVersion)
      jdkArchitectureOption: "x64"
      publishJUnitResults: true
      testResultsFiles: "**/TEST-*.xml"
      goals: "package"
  ```

* Select **Save** and then confirm the changes to see your build run two jobs on two different platforms and SDKs.

### Customize CI triggers

Pipeline triggers cause a pipeline to run. You can use `trigger`: to cause a pipeline to run whenever you push an update to a branch. YAML pipelines are configured by default with a CI trigger on your default branch (which is usually `main`). You can set up triggers for specific branches or for pull request validation. For a pull request validation trigger, just replace the `trigger`: step with `pr`: as shown in the two examples below. By default, the pipeline runs for each pull request change.

* If you'd like to set up triggers, add either of the following snippets at the beginning of your `azure-pipelines.yml` file.

  ```yaml
  trigger:
    - main
    - releases/*
  ```

  ```yaml
  pr:
    - main
    - releases/*
  ```

  You can specify the full name of the branch(for example, `main`) or a prefix-matching wildcard(for example, `releases/*`).

### Pipeline settings

There are some pipeline settings that you don't manage in your YAML file, such as the YAML file path and enabled status of your pipeline. To configure these settings, navigate to the [pipeline details page](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops#view-pipeline-details) and choose **More actions**, **settings**. For more information on navigating and browsing your pipelines, see [Navigating pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/customize-pipeline/pipeline-settings.png?view=azure-devops)

From he **Pipeline settings** pane you can configure the following settings.

* **Processing of new run requests** -  Sometimes you'll want to prevent new runs from starting on your pipeline.

  * By default, the processing of new run requests is **Enabled**. This setting allows standard processing of all trigger types, including manual runs.
  * **Paused** pipelines allow run requests to be processed, but those requests are queued without actually starting. When new request processing is enabled, run processing resumes staring with the first request in the queue.
  * **Disabled** pipelines prevent uses from starting new runs. All triggers are also disabled while this setting is applied.

* **YAML file path** - If you ever need to direct your pipeline to use a different YAML file, you can specify the path to that file. This setting can also be useful if you need to move/rename your YAML file.

* **Automatically link work items included in this run** - The changes associated with a given pipeline run may have work items associated with them. Select this option to link those work items to the run. When **automatically link work items included in this run** is selected, you must specify either a specific branch, or `*` for all branches, which is the default. If you specify a branch, work items are only associated with runs of that branch. If you specify `*`, work items are associated for all runs.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/customize-pipeline/link-work-items.png?view=azure-devops)

  * To get notifications when your runs fail, see how to [Manage notifications for a team](https://docs.microsoft.com/en-us/azure/devops/notifications/manage-team-group-global-organization-notifications?view=azure-devops).

### Create work item on failure

YAML pipelines don't have a [Create work item on failure](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/options?view=azure-devops#create-a-work-item-on-failure) settings like classic build pipelines. Classic build pipelines are single stage, and **Create work item on failure** applies to the whole pipeline. YAML pipelines can be multi-stage, and a pipeline level setting may not be appropriate. To implement **Create work item on failure** in a YAML pipeline, you can use methods such as the [Work Items - Create](https://docs.microsoft.com/en-us/rest/api/azure/devops/wit/work-items/create) REST API call or the Azure DevOps CLI [az boards work-item create](https://docs.microsoft.com/en-us/cli/azure/boards/work-item#az-boards-work-item-create) command at the desired point in your pipeline.

The following example has two jobs. The first job represents the work of the pipeline, but if it fails, the second job runs, and creates a bug in the same project as the pipeline.

```yaml
# When manually running the pipeline, you can select whether it
# succeeds or fails.
parameters:
- name: succeed
  displayName: Succeed or fail
  type: boolean
  default: false
  
trigger:
- main

pool:
  vmImage: ubuntu-latest
  
jobs:
- job: Work
  steps:
  - script: echo Hello, world!
    displayName: 'Run a one-line script'
    
    # This malformed command causes the job to fail
    # Only run this command if the succeed variable is set to false
    - script: git clone malformed input
      condition: eq(${{ parameters.succeed }}, false)
      
# This job creates a work item, and only runs if the previous job failed
- job: ErrorHandler
  dependsOn: Work
  condition: failed()
  steps:
  - bash: |
      az boards work-item create \
        --title "Build $(build.buildNumber) failed" \
        --type bug \
        --org $(System.TeamFoundationCollectionUri) \
        --project $(System.TeamProject)
    env:
      AZURE_DEVOPS_EXT_PAT: $(System.AccessToken)
    displayName: 'Create work item on failure'
```

> :notes:Note
>
> Azure Boards allows you to configure your work item tracking using several different processes, such as Agile or Basic. Each process has different work item types, and not every work item type is available in each process. For a list of work item types supported by each process, see [Work item types(WITs)](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/about-work-items?view=azure-devops#work-item-types-wits)

The previous example uses [Runtime parameters](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/runtime-parameters?view=azure-devops) to configure whether the pipeline succeeds or fails. When manually running the pipeline, you can set the value of the `succeed` parameter. The second `script` step in the first job of the pipeline evaluates the `succeed` parameter and only runs when `succeed` is set to false.

The second job in the pipeline has a dependency on the first job and only runs if the first job fails. The second job uses the Azure DevOps CLI [az boards work-item create](https://docs.microsoft.com/en-us/cli/azure/boards/work-item#az-boards-work-item-create) command to create a bug. For more information on running Azure DevOps CLI commands from a pipeline, see [Run commands in a YAML pipeline](https://docs.microsoft.com/en-us/azure/devops/cli/azure-devops-cli-in-yaml?view=azure-devops).

This example uses two jobs, but this same approach could be used across multiple stages.

> :spiral_notepad: Note
>
> You can also use a marketplace extension like [Create Bug on Release failure](https://marketplace.visualstudio.com/items?itemName=AmanBedi18.CreateBugTask) which has support for YAML multi-stage pipelines.

### Next steps

Or, to grow your CI pipeline to a CI/CD pipeline, include a [deployment job](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) with steps to deploy your app to an [environment](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops).

To learn more about the topics in this guide see Jobs, Tasks, Catalog of Tasks, Variables, Triggers, or Troubleshooting.

To learn what else you can do in YAML pipelines, see [YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/).

## [3-4 Multi-stage pipelines user experience](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops)



### Navigating pipelines

### Pipelines landing page

### View pipeline details

### View pipeline run details

### Manage security

### Task insights for failed pipeline runs

### Next steps

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