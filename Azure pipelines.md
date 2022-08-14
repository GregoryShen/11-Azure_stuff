# [Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)

# [1 :ballot_box_with_check: What is Azure Pipelines?](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)

Azure Pipelines automatically builds and tests code projects to make them available to others. It works with <u>just about</u>[^1-1]  any language or project type. Azure Pipelines combines continuous integration (CI) and continuous delivery (CD) to test and build your code and ship it to any target.

==Continuous Integration (CI) is the practice used by development teams of automating merging and testing code.== Implementing CI helps to catch bugs early in the development cycle, which makes them less expensive to fix. Automated tests execute as part of the CI process to ensure quality. Artifacts are produced  from CI systems and fed to release processes to drive frequent deployments. 

==Continuous Delivery (CD) is a process by which code is built, tested, and deployed to one or more test and production environments.== Deploying and testing in multiple environments increases quality. CI systems produce deployable artifacts, including infrastructure and apps. Automated release processes consume these artifacts to release new versions and fixes to existing systems. Monitoring and alerting systems run continually to drive visibility into the entire CD process.

==Continuous Testing (CT)== on-premises or in the cloud ==is the use of automated build-deploy-test workflows==, with a choice of technologies and frameworks, ==that test your changes continuously== in a fast, scalable, and efficient manner.

**Version control systems**

The starting point for configuring CI and CD for your applications is to have your source code in a version control system. Azure DevOps supports two forms of version control - Git and Azure Repos. Any changes you push to your version control repository will be automatically built and validated.

**Languages**

You can use many languages with Azure Pipelines, including Python, Java, JavaScript, PHP, Ruby, C#, C++, and Go.

**Application types**

You can use Azure Pipelines with most application types, such as Java, JavaScript, Node.js, Python, .NET, C++, Go, PHP, and XCode.

Azure DevOps has many tasks to build and test your application. For example, tasks exist to build .NET, Java, Node, Android, XCode, and C++ applications. Similarly, these are tasks to run tests using many testing frameworks and services. You can also run command line, PowerShell, or Shell scripts in your automation.

**Deployment targets**

Use Azure Pipelines to deploy your code to multiple targets. Targets include virtual machines, environments, containers, on-premises and cloud platforms, or PaaS services. You can also publish your mobile application to a store.

Once you have continuous integration in place, the next step is to create a release definition to automate the deployment of your application to one or more environments. This automation process is again defined as a collection of tasks.

**Continuous testing**

Whether your app is on-premises or in the cloud, you can automate build-deploy-test workflows and choose the technologies and frameworks, then [test your changes continuously](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops#run-your-tests) in a fast, scalable, and efficient manner.

* Maintain quality and find problems as you develop. Continuous testing with Azure DevOps Server ensures your app will works after every check-in and build, enabling you to find problems earlier by running tests automatically with each build.
* Any test type and any test framework. Choose the test technologies and frameworks you prefer to use.
* Rich analytics and reporting. When your build is done, review your test results to start resolving the problems you find. Rich and actionable build-on-build reports let you instantly see if your builds are getting healthier. But it's not just about speed - detailed and customizable test results measure the quality of your app.

**Package formats**

To produce packages that can be consumed by others, you can publish NuGet, npm, or Maven packages to the built-in package management repository in Azure Pipelines. You also can use any other package management repository of your choice.

**What do I need to use Azure Pipelines?**

To use Azure Pipelines, complete the following tasks：

* Create or already have an organization in Azure DevOps.
* Store your source code in a version control system.

**Pricing**

* **Azure DevOps Services**

  If you use public projects, Azure Pipelines is free. For more information, see [What is a public project?](https://docs.microsoft.com/en-us/azure/devops/organizations/public/about-public-projects?view=azure-devops) If you use private projects, you can run up to 1,800 minutes (30 hours) of pipeline jobs for free every month. For more information about how the pricing works based on [parallel jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/licensing/concurrent-jobs?view=azure-devops). For more information, see [Pricing for Azure DevOps Services](https://azure.microsoft.com/pricing/details/devops/azure-devops-services/).

* **Azure DevOps Server**

  If you have five or less active users, [Azure DevOps Express](https://visualstudio.microsoft.com/downloads/) is free, simple to set up, and installs on both client and server operating systems. It supports all the same features as Azure DevOps Server 2019.

  For more information, see [Pricing for Azure DevOps Server](https://azure.microsoft.com/pricing/details/devops/server/).

**Why should I use Azure Pipelines?**

Implementing CI and CD pipelines helps to ensure consistent and quality code that's readily available to users. And Azure Pipelines provides a quick, easy, and safe way to automate building your projects and making them available to users.

Use Azure Pipelines because it supports the following scenarios:

* Works with any language or platform
* Deploys to different types of targets at the same time
* Integrates with Azure deployments
* Builds on Windows, Linux, or Mac machines
* Integrates with GitHub
* Works with open-source projects.

**Next steps**

Use Azure Pipelines

# 2 :ballot_box_with_check: Use Azure Pipelines

Azure Pipelines supports continuous integration (CI) and continuous delivery (CD) to continuously test, build, and deploy your code. You accomplish this by defining a pipeline.

The latest way to build pipelines is with the YAML pipeline editor. You can also use Classic pipelines with the Classic editor.

**Automate tests, builds, and delivery**

Continuous integration (CI) automates tests and builds for your project. CI helps to catch bugs or issues early in the development cycle, when they're easier and faster to fix. Items known as artifacts are produced from CI systems. They're used by the continuous delivery release pipelines to drive automatic deployments.

Continuous delivery automatically deploys and tests code in multiple stages to help drive quality. Continuous integration systems produce deployable artifacts, which include infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fixes to the target of your choice.

| Continuous integration (CI)                     | Continuous delivery (CD)                   |
| :---------------------------------------------- | :----------------------------------------- |
| Increase code coverage                          | Automatically deploy code to production    |
| Build faster by splitting test and build runs   | Ensure deployment targets have latest code |
| Automatically ensure you don't ship broken code | Use tested code from CI process            |
| Run tests continually                           |                                            |

**Define pipelines using YAML syntax**

You define your pipeline in a YAML file called `azure-pipelines.yml` with the rest of your app.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/pipelines-image-yaml.png?view=azure-devops)

* The pipeline is versioned with your code. It follows the same branching structure. You get validation of your changes through code reviews in pull requests and branch build policies.
* Every branch you use can modify the pipeline by modifying the `azure-pipelines.yml` file. Learn more about [branch consideration for YAML pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops#branch-considerations).
* A change to the build process might cause a break or result in an unexpected outcome. Because the change is in version control with the rest of your codebase, you can more easily identify the issue.

Follow these basic steps:

1. Configure Azure Pipelines to use your Git repo.
2. Edit your `azure-pipelines.yml` file to define your build.
3. Push your code to your version control repository. This action kicks off the default trigger to build and deploy and then monitor the results.

Your code is now updated, built, tested, and packaged. It can be deployed to any target.

**Define pipelines using the Classic interface**

Create and configure pipelines in the Azure DevOps web portal with the Classic user interface editor. You define a <u>build pipeline</u> to build and test your code, and then to publish artifacts. You also define a <u>release pipeline</u> to consume and deploy those artifacts to deployment targets.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/pipelines-image-designer.png?view=azure-devops)

Follow these basic steps:

1. Configure Azure Pipelines to use your Git repo.
2. Use the Azure Pipelines classic editor to create and configure your build and release pipelines.
3. Push your code to your version control repository. This action triggers your pipeline and runs tasks such as building or testing code.

The build creates an artifact that's used by the rest of your pipeline to run tasks such as deploying to staging or production.

Your code is now updated, built, tested, and packaged. It can be deployed to any target.

**Feature availability**

Certain[^2-1] pipeline features are only available when using YAML or when defining build or release pipelines with the Classic interface. The following table indicates which features are supported and for which tasks and methods.

|        Feature        | YAML | Classic Build | Classic Release | Notes                                                        |
| :-------------------: | :--: | :-----------: | :-------------: | ------------------------------------------------------------ |
|        Agents         | Yes  |      Yes      |       Yes       | Specifies a required resource on which the pipeline runs.    |
|       Approvals       | Yes  |      No       |       Yes       | Defines a set of validations required prior to completing a deployment stage. |
|       Artifacts       | Yes  |      Yes      |       Yes       | Supports publishing or consuming different package types.    |
|        Caching        | Yes  |      Yes      |       No        | Reduces build time by allowing outputs or downloaded dependencies from one run to be reused in later runs. In Preview, available with Azure Pipelines only. |
|      Conditions       | Yes  |      Yes      |       Yes       | Specifies conditions to be met prior to running a job.       |
|    Container jobs     | Yes  |      No       |       No        | Specifies jobs to run in a container.                        |
|        Demands        | Yes  |      Yes      |       Yes       | Ensures pipeline requirements are met before running a pipeline stage. Requires self-hosted agents. |
|     Dependencies      | Yes  |      Yes      |       Yes       | Specifies a requirement that must be met in order to run the next job or stage. |
|   Deployment groups   | Yes  |      No       |       Yes       | Defines a logical set of deployment target machines.         |
| Deployment group jobs |  No  |      No       |       Yes       | Specifies a job to release to a deployment group.            |
|    Deployment jobs    | Yes  |      No       |       No        | Defines the deployment steps.                                |
|      Environment      | Yes  |      No       |       No        | Represents a collection of resources targeted for deployment. Available with Azure Pipelines only. |
|         Gates         |  No  |      No       |       Yes       | Supports automatic collection and evaluation of external health signals prior to completing a release stage. Available with Classic Release only. |
|         Jobs          | Yes  |      Yes      |       Yes       | Defines the execution sequence of a set of steps.            |
|  Service connections  | Yes  |      Yes      |       Yes       | Enables a connection to a remote service that is required to execute tasks in a job. |
|  Service containers   | Yes  |      No       |       No        | Enables you to manage the lifecycle of a containerized service. |
|        Stages         | Yes  |      No       |       Yes       | Organizes jobs within a pipeline.                            |
|      Task groups      |  No  |      Yes      |       Yes       | Encapsulates a sequence of tasks into a single reusable task. If using YAML, see templates. |
|         Tasks         | Yes  |      Yes      |       Yes       | Defines the building blocks that make up a pipeline.         |
|       Templates       | Yes  |      No       |       No        | Defines reusable content, logic, and parameters.             |
|       Triggers        | Yes  |      Yes      |       Yes       | Defines the event that causes a pipeline to run.             |
|       Variables       | Yes  |      Yes      |       Yes       | Represents a value to be replaced by data to pass to the pipeline. |
|    Variable groups    | Yes  |      Yes      |       Yes       | Use to store values that you want to control and make available across multiple pipelines. |

**Next steps**

[Create your first pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline?view=azure-devops)

# 3 :ballot_box_with_check: Get started

## [3-1 :ballot_box_with_check: Sign up for Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)

Sign up for an Azure DevOps organization and Azure Pipelines to begin managing CI/CD to deploy your code with high-performance pipelines.

For more information about Azure Pipelines, see [What is Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops).

You can sign up with either a Microsoft account or a GitHub account.

**Sign up with a Microsoft account**

To sign up for Azure DevOps with a Microsoft account, complete the following steps.

1. Check that your account is up to date by logging into your Microsoft account.

2. Open <u>Azure Pipelines</u> and select <u>Start free</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/start-free-azure-pipelines.png?view=azure-devops)

3. Log into your Microsoft account.

4. To get started with Azure Pipelines, select <u>Continue</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/sign-up-azure-devops.png?view=azure-devops)

5. Enter a name for your organization, select a host location from the drop-down menu, enter the characters you see, and then select <u>Continue</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/almost-done-name-organization.png?view=azure-devops)

Use the following URL to sign in to your organization at any time:

`https://dev.azure.com/{yourorganization}`

You're now prompted to [create a project](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops#create-project).

**Sign up with a GitHub account**

To sign up for Azure DevOps with a GitHub account, complete the following steps.

> :imp:Important
>
> If your GitHub email address is associated with an Azure AD-backed organization in Azure DevOps, you can't sign in with your GitHub account, rather you must sign in with your Azure AD account.

1. Check that your account is up to date by logging into your GitHub account.

2. Open Azure Pipelines and select <u>Start free with GitHub</u>. If you're already part of an Azure DevOps organization, choose <u>Start free</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/start-free-github-pipelines.png?view=azure-devops)

3. Enter your GitHub account credentials, and then select <u>Sign in</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/enter-github-credentials.png?view=azure-devops)

4. Select <u>Authorize Microsoft-corp</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/authorize-microsoft-corp.png?view=azure-devops)

5. Select <u>Next</u> to create a new Microsoft account linked to your GitHub credentials.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/link-microsoft-account.png?view=azure-devops)

   For more information about GitHub authentication, see [FAQs](https://docs.microsoft.com/en-us/azure/devops/organizations/security/faq-github-authentication?view=azure-devops).

6. Fill in your name, email address, and country.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/fill-in-details-devops-github.png?view=azure-devops)

7. To get stated with Azure Pipelines, select <u>Continue</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/sign-up-azure-devops.png?view=azure-devops)

8. Enter a name for your organization, select a host location from the drop-down menu, enter the characters you see, and then select <u>Continue</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/almost-done-name-organization.png?view=azure-devops)

Use the following URL to sign in to your organization at any time:

`https://dev.azure.com/{yourogranization}`

You're now prompted to create a project.

**Create a project**

You can create public or private projects. To learn more about public projects, see [What is a public projects?](https://docs.microsoft.com/en-us/azure/devops/organizations/public/about-public-projects?view=azure-devops).

1. Enter a name for your project, select the visibility, and optionally provide a description. Then choose <u>Create project</u>.

   ![](https://docs.microsoft.com/en-us/azure/devops/boards/get-started/media/sign-up/nf-create-project.png?view=azure-devops)

   Special characters aren't allowed in the project name (such as / : \\ ~ & % ; @ ' " ? <> | # $ * } { , + = [ ]). The project name also can't begin with an underscore, can't begin or end with a period, and must be 64 characters or less. Set  your project visibility to either public or private. Public visibility allows for anyone on the internet to view your project. Private visibility is for only people who you give access to your project.

2. When your project is created, if you signed up with a Microsoft account, the wizard to create a new pipeline automatically starts. If you signed up with a GitHub account, you're asked to select which services to use.

You're now set to create your first pipeline, or invite other users to collaborate with your project.

**Invite team members - optional**

Add and invite others to work on our project by adding their email address to your organization and project.

1. From your project web portal, choose Azure DevOps > Organization settings.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/settings/open-admin-settings-vert-2.png?view=azure-devops)

2. Select Users > Add users.

   ![](https://docs.microsoft.com/en-us/azure/devops/media/add-new-users.png?view=azure-devops)

3. Complete the form by entering or selecting the following information:

   * Users: Enter the email addresses (Microsoft accounts) or GitHub IDs for the users. You can add several email addresses by separating them with a semicolon (;).
   * Access level: Assign one of the following access levels:
     * Basic: Assign to users who must have access to all Azure Pipelines features. You can grant up to five users Basic access for free.
     * Stakeholder: Assign to users for limited access to features to view, add, and modify work items. You can assign an unlimited amount of users Stakeholder access for free.
     * Visual Studio Subscriber: Assign to users who already have a Visual Studio subscription.
   * Add to project: Select the project you named in the preceding procedure.
   * Azure DevOps groups: Select one of the following security groups, which will determine the permissions the users have to do select tasks. To learn more, see [Azure Pipelines resources](https://docs.microsoft.com/en-us/azure/devops/pipelines/security/resources?view=azure-devops).
     * Project Readers: Assign to users who only require read-only access.
     * Project Contributors: Assign to users who will contribute fully to the project.
     * Project Administrators: Assign to users who will configure project resources.

   > :notes:Note
   >
   > Add email addresses for Microsoft accounts and IDs for GitHub accounts unless you plan to use Azure Active Directory (Azure AD) to authenticate users and control organization access. If a user doesn't have a Microsoft or GitHub account, ask the user to sign up for a Microsoft account or a GitHub account.

4. When you're done, select <u>Add</u> to complete your invitation.

For more information, see [Add organization users for Azure DevOps Services](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/add-organization-users?view=azure-devops).

**Change organization or project settings **

You can rename and delete your organization, or change the organization location. For more information, see the following articles:

* [Manage organizations](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/organization-management?view=azure-devops)
* [Rename an organization](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/rename-organization?view=azure-devops)
* [Change the location of your organization](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/change-organization-location?view=azure-devops)

You can rename your project or change its visibility. To learn more about managing projects, see the following articles:

* [Manage projects](https://docs.microsoft.com/en-us/azure/devops/organizations/projects/about-projects?view=azure-devops)
* [Rename a project](https://docs.microsoft.com/en-us/azure/devops/organizations/projects/rename-project?view=azure-devops)
* [Change the project visibility, public or private](https://docs.microsoft.com/en-us/azure/devops/organizations/public/make-project-public?view=azure-devops)

**Next steps**

[Create your first pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline?view=azure-devops)

**Related articles**

* [What is Azure Pipelines?](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)
* [Key concepts for new Azure Pipelines users](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops)
* [Customize your pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops)

## [3-2 :ballot_box_with_check: Create your first pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=java%2Ctfs-2018-2%2Cbrowser)

This is a step-by-step guide to using Azure Pipelines to build a sample application.

This guide uses YAML pipelines configured with the YAML pipeline editor. If you'd like to use Classic pipelines instead, see [Define your Classic pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/define-multistage-release-process?view=azure-devops).

**Prerequisites - Azure DevOps**

Make sure you have the following items:

* A GitHub account where you can create a repository.
* An Azure DevOps organization. If your team already has one, then make sure you're an administrator of the Azure DevOps project that you want to use.
* An ability to run pipelines on Microsoft-hosted agents. You can either purchase a parallel job or you can request a free tier.

**Create your first pipeline**

* **<u>CSharp</u>**

  * **Get the .NET sample code**

    To get started, fork the following repository into your GitHub account.

    https://github.com/MicrosoftDocs/pipelines-dotnet-core

  * **Create your first .NET Core pipeline**

    1. Sign-in to your Azure DevOps organization and go to your project.

    2. Go to <u>Pipelines</u>, and then select <u>New pipeline</u>.

    3. Do the steps of the wizard by first selecting <u>GitHub</u> as the location of your source code.

    4. You might be redirected to GitHub to sign in. If so, enter your GitHub credentials.

    5. When you see the list of repositories, select your repository.

    6. You might be redirected to GitHub to install the Azure Pipelines app. If so, select <u>Approve & install</u>.

    7. Azure Pipelines will analyze your repository and recommend the <u>ASP.NET Core</u> pipeline template.

    8. When your new pipeline appears, take a look at the YAML to see what it does. When you're ready, select <u>Save and run</u>.

    9. You're prompted to commit a new `azure-pipelines.yml` file to your repository. After you're happy with the message, select `Save and run` again.

       If you want to watch your pipeline in action, select the build job.

       You just created and ran a pipeline that we automatically created for you, because your code appeared to be a good match for the <u>ASP.NET Core</u> template.

       You now have a working YAML pipeline (`azure-pipelines.yml`) in your repository that's ready for you to customize!

    10. When you're ready to make changes to your pipeline, select it in the <u>Pipelines</u> page, and then <u>Edit</u> the `azure-pipelines.yml` file.

    Learn more about [working with .NET Core](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops) in your pipeline.

* **<u>Python</u>**

  **Get the Python sample code**

  https://github.com/Microsoft/python-sample-vscode-flask-tutorial

  **Create your first Python pipeline**

  1. Sign-in to your Azure DevOps organization and go to your project.

  2. Go to **Pipelines**, and then select **New pipeline**.

  3. Do the steps of the wizard by first selecting **GitHub** as the location of your source code.

  4. You might be redirected to GitHub to sign in. If so, enter your GitHub credentials.

  5. When you see the list of repositories, select your repository.

  6. You might be redirected to GitHub to install the Azure Pipelines app. If so, select **Approve & install**.

  7. Azure Pipelines will analyze your repository and recommend the **Python package** pipeline template.

  8. When your new pipeline appears, take a look at the YAML to see what it does. When you're ready, select **Save and run**.

  9. You're prompted to commit a new `azure-pipelines.yml` file to your repository. After you're happy with the message, select **Save and run** again.

     If you want to watch your pipeline in action, select the build job.

     You just created and ran a pipeline that we automatically created for you, because your code appeared to be a good match for the [Python package](https://github.com/microsoft/azure-pipelines-yaml/blob/master/templates/python-package.yml) template.

     You now have a working YAML pipeline (`azure-pipelines.yml`) in your repository that's ready for you to customize!

  10. When you're ready to make changes to your pipeline, select it in the **Pipelines** page, and then **Edit** the `azure-pipelines.yml` file.

  Learn more about [working with Python](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python?view=azure-devops) in your pipeline.

**Add a status badge to your repository**

Many developers like to show that they're keeping their code quality high by displaying a status badge in their repo.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/azure-pipelines-succeeded.png?view=azure-devops)

To copy the status badge to  your clipboard:

1. In Azure Pipelines, go to the <u>Pipelines</u> page to view the list of pipelines. Select the pipeline you created in the previous section.
2. Select ..., and then select <u>Status badge</u>.
3. Select <u>Status badge</u>.
4. Copy the sample Markdown from the Sample markdown section.

Now with the badge Markdown in your clipboard, take the following steps in GitHub:

1. Go to the list of files and select `Readme.md`. Select the pencil icon to edit.
2. Paste the status badge Markdown at the beginning of the file.
3. Commit the change to the `main` branch.
4. Notice that the status badge appears in the description of your repository.

To configure anonymous access to badges for private projects:

1. Navigate to **Project Settings**
2. Open the **Settings** tab under **Pipelines**
3. Toggle[^3-2-1] the **Disable anonymous access to badges** slider[^3-2-2] under **General**

> :notes:Note
>
> Even in a private project, anonymous badge access is enabled by default. With anonymous badge access enabled, users outside your organization might be able to query information such as project names, branch names, job names, and build status through the badge status API.

Because you just changed the `Readme.md` file in this repository, Azure Pipelines automatically builds your code, according to the configuration in the `azure-pipelines.yml` file at the root of your repository. Back in Azure Pipelines, observe that a new run appears. Each time you make an edit, Azure Pipelines starts a new run.

**Next steps**

You've just learned how to create your first pipeline in Azure. Learn more about configuring pipelines in the language of your choice:

* [.NET Core](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops)
* [Go](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/go?view=azure-devops)
* [Java](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/java?view=azure-devops)
* [Node.js](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/javascript?view=azure-devops)
* [Python](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python?view=azure-devops)
* [Containers](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/containers/build-image?view=azure-devops)

Or, you can proceed to <u>customize the pipeline</u> you just created.

To run your pipeline in a container, see [Container jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=azure-devops).

For details about building GitHub repositories, see [Build GitHub repositories](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops).

To learn how to publish your Pipeline Artifacts, see [Publish Pipeline Artifacts](https://docs.microsoft.com/en-us/azure/devops/pipelines/publish-pipeline-artifact?view=azure-devops).

To find out what else you can do in YAML pipelines, see [YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema).

**Clean up**

If you created any test pipelines, they are easy to delete when you are done with them.

* **<u>Browser</u>**

  To delete a pipeline, navigate to the summary page for that pipeline, and choose **Delete** from the **...** menu at the top-right of the page. Type the name of the pipeline to confirm, and choose **Delete**.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/media/get-started-yaml/delete-pipeline.png?view=azure-devops)

* **<u>Azure DevOps CLI</u>**

  To delete a pipeline using Azure CLI, you can use the [az pipeline delete](https://docs.microsoft.com/en-us/cli/azure/pipelines#ext-azure-devops-az-pipelines-delete) command. This command requires the `id` of the pipeline to delete, which you can get using the [az pipeline list](https://docs.microsoft.com/en-us/cli/azure/pipelines#ext-azure-devops-az-pipelines-list) command.

  > :notes:Note
  >
  > If this is your first time using [az pipelines](https://docs.microsoft.com/en-us/cli/azure/pipelines) commands, see [Get started with Azure DevOps CLI](https://docs.microsoft.com/en-us/azure/devops/cli/?view=azure-devops).

  * **List pipelines**

    You can list your pipelines using the [az pipelines list](https://docs.microsoft.com/en-us/cli/azure/pipelines#ext-azure-devops-az-pipelines-list) command.

    ```powershell
    az pipelines list [--detect {false, true}]
                      [--folder-path]
                      [--name]
                      [--org]
                      [--project]
                      [--query-order {ModifiedAsc, ModifiedDesc, NameAsc, NameDesc, None}]
                      [--repository]
                      [--repository-type {bitbucket, git, github, githubenterprise, svn, tfsgit, tfsversioncontrol}]
                      [--top]
    ```

    * **Parameters**
      * **detect**: Automatically detect organization. Accepted values: **false**, **true**
      * **folder-path**: If specified, filters to definitions under this folder.
      * **name**: Limit results to pipelines with this name or starting with this name. Examples: "FabCI" or "Fab*".
      * **org** or **organization**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up via git config. Example: `https://dev.azure.com/MyOrganizationName/`.
      * **project** or **p**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up via git config.
      * **query-order**: Order of the results. Accepted values: **ModifiedAsc**, **ModifiedDesc**, **NameAsc**, **NameDesc**, **None**
      * **repository**: Limit results to pipelines associated with this repository.
      * **repository-type**: Limit results to pipelines associated with this repository type. It is mandatory to pass **repository** argument along with this argument. Accepted values: **bitbucket**, **git**, **github**, **githubenterprise**, **svn**, **tfsgit**, **tfsversioncontrol**
      * **top**: Maximum number of pipelines to list.

  * **Delete pipeline**

    You can delete a pipeline using the [az pipelines delete](https://docs.microsoft.com/en-us/cli/azure/pipelines#ext-azure-devops-az-pipelines-delete) command.

    ```powershell
    az pipelines delete --id
                        [--detect {false, true}]
                        [--org]
                        [--project]
                        [--yes]
    ```

    * **Parameters**
      * **id**: (Required) ID of the pipeline.
      * **detect**: Automatically detect organization. Accepted values: **false**, **true**
      * **org** or **organization**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up via git config. Example: `https://dev.azure.com/MyOrganizationName/`.
      * **project** or **p**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up via git config.
      * **yes** or **y**: Do not prompt for confirmation.

  * **Example**

    The following example lists pipelines in table format, and then deletes the pipeline with an ID of 6. This example uses the following default configuration:

    `az devops configure --defaults organization=https://dev.azure.com/fabrikam-tailspin project=FabrikamFiber`

    ```powershell
    az pipelines list --output table
    
    ID    Path    Name           Status    Default Queue
    ----  ------  -------------  --------  ------------------
    6     \       FabrikamFiber  enabled   Hosted Ubuntu 1604
    
    az pipelines delete --id 6
    
    Are you sure you want to delete this pipeline? (y/n): y
    Pipeline 6 was deleted successfully.
    ```

**FAQ**

* **Where can I read articles about DevOps and CI/CD?**

  [What is Continuous Integration?](https://docs.microsoft.com/en-us/devops/develop/what-is-continuous-integration)

  [What is Continuous Delivery?](https://docs.microsoft.com/en-us/devops/deliver/what-is-continuous-delivery)

  [What is DevOps?](https://azure.microsoft.com/overview/what-is-devops/)

* **What version control system can I use?**

  When you're ready to get going with CI/CD for your app, you can use the version control system of your choice:

  * Clients
    * [Visual Studio Code for Windows, macOS, and Linux](https://code.visualstudio.com/)
    * [Visual Studio with Git for Windows](https://docs.microsoft.com/en-us/azure/devops/repos/git/share-your-code-in-git-vs?view=azure-devops) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/visual-studio-mac/)
    * [Eclipse](https://docs.microsoft.com/en-us/azure/devops/repos/git/share-your-code-in-git-eclipse?view=azure-devops)
    * [Xcode](https://docs.microsoft.com/en-us/azure/devops/repos/git/share-your-code-in-git-xcode?view=azure-devops)
    * [IntelliJ](https://docs.microsoft.com/en-us/previous-versions/azure/devops/all/java/download-intellij-plug-in)
    * [Command line](https://docs.microsoft.com/en-us/azure/devops/repos/git/share-your-code-in-git-cmdline?view=azure-devops)
  * Services
    * [Azure Pipelines](https://visualstudio.microsoft.com/team-services/)
    * Git service providers such as GitHub and Bitbucket Cloud
    * Subversion

* **How can I delete a pipeline?**

  To delete a pipeline, navigate to the summary page for that pipeline, and choose **Delete** from the **...** menu in the top-right of the page. Type the name of the pipeline to confirm, and choose **Delete**.

* **What else can I do when I queue a build?**

  You can queue builds [automatically](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops) or manually.

  When you manually queue a build, you can, for a single run of the build:

  * Specify the [pool](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops) into which the build goes.
  * Add and modify some [variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops).
  * Add [demands](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/demands?view=azure-devops).
  * In a Git repository
    * Build a [branch](https://docs.microsoft.com/en-us/azure/devops/repos/git/create-branch?view=azure-devops) or a [tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging).
    * Build a [commit](https://docs.microsoft.com/en-us/azure/devops/repos/git/commits?view=azure-devops).

*  **Where can I learn more about pipeline settings?**

  To learn more about pipeline settings, see:

  * [Getting sources](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/?view=azure-devops)
  * [Tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/?view=azure-devops)
  * [Variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops)
  * [Triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops)
  * [Retention[^3-2-3]](https://docs.microsoft.com/en-us/azure/devops/pipelines/policies/retention?view=azure-devops)
  * [History](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/history?view=azure-devops)

*  **How do I programmatically create a build pipeline?**

  [REST API Reference: Create a build pipeline](https://docs.microsoft.com/en-us/azure/devops/integrate/?view=azure-devops)

  > :notes:Note
  >
  > You can also manage builds and build pipelines from the command line or scripts using the <u>Azure Pipelines CLI</u>.

## [3-3 :ballot_box_with_check: Customize your pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops)

This is a step-by-step guide on common ways to customize your pipeline.

**Prerequisite**

Follow instructions in <u>Create your first pipeline</u> to create a working pipeline.

**Understand the `azure-pipelines.yml` file**

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

**Change the platform to build on**

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

**Add steps**

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

**Build across multiple platforms**

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

**Build using multiple versions**

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

**Customize CI triggers**

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

**Pipeline settings**

There are some pipeline settings that you don't manage in your YAML file, such as the YAML file path and enabled status of your pipeline. To configure these settings, navigate to the [pipeline details page](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops#view-pipeline-details) and choose **More actions**, **settings**. For more information on navigating and browsing your pipelines, see [Navigating pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops).

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

**Create work item on failure**

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

**Next steps**

Or, to grow your CI pipeline to a CI/CD pipeline, include a [deployment job](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) with steps to deploy your app to an [environment](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops).

To learn more about the topics in this guide see Jobs, Tasks, Catalog of Tasks, Variables, Triggers, or Troubleshooting.

To learn what else you can do in YAML pipelines, see [YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/).

## [3-4 :ballot_box_with_check: Multi-stage pipelines user experience](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops)

The multi-stage pipelines experience brings improvements and ease of use to the Pipelines portal UI. This article shows you how to view and manage your pipelines using this new experience.

**Navigating pipelines**

You can view and manage your pipelines by choosing <u>Pipelines</u> from the left-hand menu.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipelines-overview.png?view=azure-devops)

You can drill down and view pipeline details, run details, pipeline analytics, job details, logs, and more.

At the top of each page is a breadcrumb navigation bar. Select the different areas of the bar to navigate to different areas of the portal. The breadcrumb navigation is a convenient way to go back one or more steps.

 ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/breadcrumb-navigation.png?view=azure-devops)

1. This area of the breadcrumb navigation shows you what page you're currently viewing. In this example, the page is the run summary for run number 20191209.3.
2. One level up is a link to the pipeline details for that run.
3. The next level up is the pipelines landing page.
4. This link is to the FabrikamFiber project, which contains the pipeline for this run.
5. The root breadcrumb link is to the Azure DevOps fabrikam-tailspin organization, which contains the project that contains the pipeline.

Many pages also contain a back button that takes you to the previous page.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipelines-back-button.png?view=azure-devops)

**Pipelines landing page**

From the pipelines landing page you can view pipelines and pipeline runs, create and import pipelines, manage security, and drill down into pipeline and run details.

Choose Recent to view recently run pipelines (the default view), or choose All to view all pipelines.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/view-pipelines.png?view=azure-devops)

Select a pipeline to manage that pipeline and view its runs. Select the build number for the last run to view the results of that build, select the branch name to view the branch for that run, or select the context menu to run the pipeline and perform other management actions.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipelines-overview-pipeline-context-menu.png?view=azure-devops)

Select Runs to view all pipeline runs. You can optionally filter the displayed runs.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/all-pipeline-runs.png?view=azure-devops)

Select a pipeline run to view information about that run.

Once you're done, use the breadcrumb navigation bar to navigate to the pipeline's details page.

**View pipeline details**

The details page for a pipeline allows you to view and manage that pipeline.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-overview.png?view=azure-devops)

Choose Edit to edit your pipeline. For more information, see YAML pipeline editor.

* **Pipeline settings**

You can view and configure pipeline settings from the More actions menu on the pipeline details page.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-more-actions.png?view=azure-devops)

* Manage security - Manage security

* Rename/move - Edit your pipeline name and folder location.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/rename-move-pipeline.png?view=azure-devops)

* Status badge - [Add a status badge to your repository](https://docs.microsoft.com/en-us/azure/devops/pipelines/create-first-pipeline?view=azure-devops&preserve-view=true#add-a-status-badge-to-your-repository)

* Settings - [Pipeline settings](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops#pipeline-settings)

* Delete - Deletes the pipeline including all builds and associated artifacts.

* Scheduled runs - [Scheduled runs view](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops#scheduled-runs-view)

* **Runs**

Select <u>Runs</u> to view the runs for that pipeline. You can optionally filter the displayed runs.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-runs.png?view=azure-devops)

You can choose to <u>Retain</u> or <u>Delete</u> a run from the context menu. For more information on run retention, see [Build and release retention policies](https://docs.microsoft.com/en-us/azure/devops/pipelines/policies/retention?view=azure-devops).

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-run-context-menu.png?view=azure-devops)

* **Branches**

Select <u>Branches</u> to view the history or run for that branch. Hover over the <u>History</u> to view a summary for each run, and select a run to navigate to the details page for that run.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-branches.png?view=azure-devops)

* **Analytics**

Select <u>Analytics</u> to view pipeline metrics such as pass rate and run duration. Choose <u>View full report</u> for more information on each metric.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-analytics.png?view=azure-devops)

**View pipeline run details**

From the pipeline run summary you can view the status of your run, both while it is running and when it is complete.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-run-summary.png?view=azure-devops)

From the summary pane you can download artifacts, and navigate to linked commits, test results, and work items.

* **Cancel and re-run a pipeline**

If the pipeline is running, you can cancel it by choosing <u>Cancel</u>. If the run has completed, you can re-run the pipeline by choosing <u>Run new</u>.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/cancel-pipeline-run.png?view=azure-devops)

* **Pipeline run more actions menu**

From the <u>More actions</u> menu you can download logs, add tags, edit the pipeline, delete the run, and configure retention for the run.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-run-summary-context-menu.png?view=azure-devops)

> :information_desk_person:Note
>
> You can't delete a run if the run is retained. If you don't see <u>Delete</u>, choose <u>Stop retaining run</u>, and then delete the run. If you see both <u>Delete</u> and <u>View retention releases</u>, one or more configured retention policies still apply to your run. Choose <u>View retention releases</u>, delete the policies (only the policies for the selected run are removed), and then delete the run.

* **Jobs and stages**

The job pane displays an overview of the status of your stages and jobs. This pane may have multiple tabs depending on whether your pipeline has stages and jobs, or just jobs. In this example, the pipeline has two stages named <u>Build</u> and <u>Deploy</u>. You can drill down into the pipeline steps by choosing the job from either the <u>Stages</u> or <u>Jobs</u> pane.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-jobs-pane.png?view=azure-devops)

Choose a job to see the steps for that job.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-steps-list.png?view=azure-devops)

From the steps view, you can review the status and details of each step. From the <u>More actions</u> you can toggle timestamps or view a raw log of all steps in the pipeline.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipeline-steps-context-menu.png?view=azure-devops)

**Manage security**

You can configure pipelines security on a project level from <u>More actions</u> on the pipelines landing page, and on a pipeline level on the pipeline details page.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/pipelines-context-menu.png?view=azure-devops)

To support security of your pipeline operations, you can add users to a built-in security group, set individual permissions for a user or group, or add users to predefined roles. You can manage security for Azure Pipelines in the web portal, either from the user or admin context. For more information on configuring pipelines security, see [Pipeline permissions and security roles](https://docs.microsoft.com/en-us/azure/devops/pipelines/policies/permissions?view=azure-devops).

**Task insights for failed pipeline runs**

Azure DevOps provides a <u>Task Insights for Failed Pipeline Runs</u> setting, that when enabled, provides pop-up notifications of build failures with a link to view a report.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/task-insights.png?view=azure-devops)

To configure this setting, navigate to [Preview features](https://docs.microsoft.com/en-us/azure/devops/project/navigation/preview-features?view=azure-devops), find <u>Task Insights for Failed Pipeline Runs</u>, and choose the desired setting.

![](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/media/task-insights-setting.png?view=azure-devops)

**Next steps**

Learn more about configuring pipelines in the language of your choice:

* .NET Core
* Go
* Java
* Node.js
* Python
* Containers and Container jobs

Learn more about building Azure Repos and GitHub repositories.

To learn what else you can do in YAML pipelines, see <u>Customize your pipeline</u>, and for a complete reference see [YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema).

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

**Agent**

When your build or deployment runs, the system begins one or more jobs. An agent is computing infrastructure with installed agent software that runs one job at a time. For example, your job could run on a Microsoft-hosted Ubuntu agent.

For more in-depth information about the different types of agents and how to use them, see [Azure Pipelines Agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops).

**Approvals**

[Approvals](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/approvals?view=azure-devops) define a set of validations required before a deployment runs. Manual approval is a common check performed to control deployments to production environments. When checks are configured on an environment, pipelines will stop before starting a stage that deployed to the environment until all the checks are completed successfully.

**Artifact**

An artifact is a collection of files or packages published by a run. Artifacts are made available to subsequent tasks, such as distribution or deployment. For more information, see [Artifacts in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/artifacts/artifacts-overview?view=azure-devops).

**Continuous delivery**

Continuous delivery (CD) is a process by which code is built, tested, and deployed to one or more test and production stages. Deploying and testing in multiple stages helps drive quality. Continuous integration systems product deployable artifacts, which include infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fixes to existing systems. Monitoring and alerting systems run constantly to drive visibility into the entire CD process. This process ensures that errors are caught often and early.

**Continuous integration**

Continuous integration (CI) is the practice used by development teams to simplify the testing and building of code. CI helps to catch bugs or problems early in the development cycle, which makes them easier and faster to fix. Automated tests and builds are run as part of the CI process. The process can run on a set schedule, whenever code is pushed, or both. Items known as artifacts are produced from CI systems. They're used by the continuous delivery release pipelines to drive automatic deployments.

**Deployment**

For Classic pipelines, a deployment is the action of running the tasks for one stage, which can include running automated tests, deploying build artifacts, and any other actions are specified for that stage.

For YAML pipelines, a deployment typically refers to a deployment job. A deployment job is a collection of steps that are run sequentially against an environment. You can use strategies like run once, and canary for deployment jobs.

**Deployment group**

A deployment group is a set of deployment target machines that have agents installed. A deployment group is just another grouping of agents, like an [agent pool](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops). You can set the deployment targets in a pipeline for a job using a deployment group. Learn more about provisioning agents for [deployment groups](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/deployment-groups/howto-provision-deployment-group-agents?view=azure-devops).

**Environment**

An environment is a collection of resources, where you deploy your application. It can contain one or more virtual machines, containers, web apps, or any service that's used to host the application being developed. A pipeline might deploy the app to one or more environments after build is completed and tests are run.

**Job**

A stage contains one or more jobs. Each job runs on an agent. A job represents an execution boundary of a set of steps. All of the steps run together on the same agent. Jobs are most useful when you want to run a series of steps in different environments. For example, you might want to build two configurations - x86 and x64. In this case, you have one stage and two jobs. One job would be for x86 and the other job would be for x64.

**Pipeline**

A pipeline defines the continuous integration and deployment process for your app. It's made up of one or more stages. It can be thought of as a workflow that defines how your test, build, and deployment steps are run.

**Release**

For Classic pipelines, a release is a versioned set of artifacts specified in a pipeline. The release includes a snapshot of all the information required to carry out all the tasks and actions in the release pipeline, such as stages, tasks, policies such as triggers and approvers, and deployment options. You can create a release manually, with a deployment trigger, or with the REST API.

For YAML pipelines, the build and release stages are in one, multi-stage pipeline.

**Run**

A run represents one execution of a pipeline. It collects the logs associated with running the steps and the results of running tests. During a run, Azure Pipelines with first process the pipeline and then send the run to one or more agents. Each agent will run jobs. Learn more about the pipeline run sequence.

**Script**

A script runs code as a step in your pipeline using command line, PowerShell, or Bash. You can write cross-platform scripts for macOS, Linux, and Windows. Unlike a task, a script is custom code that is specific to your pipeline.

**Stage**

A [stage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/stages?view=azure-devops) is a logical boundary in the pipeline. It can be used to mark separation of concerns (for example, Build, QA, and production). Each stage contains one or more jobs. When you define multiple stages in a pipeline, by default, they run one after the other. You can specify the conditions for when a stage runs. When you are thinking about whether you need a stage, ask yourself:

* Do separate groups manage different parts of this pipeline? For example, you could have a test manager that manages the jobs that relate to testing and a different manager that manages jobs related to production deployment. In this case, it makes sense to have separate stages for testing and production.
* Is there a set of approvals that are connected to a specific job or set of jobs? If so, you can use stages to break your jobs into logical groups that require approvals.
* Are there jobs that need to run a long time? If you have part of your pipeline that will have an extended run time, it makes sense to divide them into their own stage.

**Step**

A step is the smallest building block of a pipeline. For example, a pipeline might consist of build and test steps. A step can either be a script or a task. A task is simply a pre-created script offered as a convenience to you. To view the available tasks, see the [Build and release tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/?view=azure-devops) reference. For information on creating custom tasks, see [Create a custom task](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-build-task?view=azure-devops).

**Task**

A task is the building block for defining automation in a pipeline. A task is packaged script or procedure that has been abstracted with a set of inputs.

**Trigger**

A trigger is something that's set up to tell the pipeline when to run. You can configure a pipeline to run upon a push to a repository, at scheduled times, or upon the completion of another build. All of these actions are known as triggers. For more information, see [build triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops) and [release triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops).

**Library**

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

### 4-3-1 Supported repositories

### 4-3-2 Azure Repos Git

4-3-3 GitHub

4-3-4 GitHub Enterprise Server

4-3-5 Pipeline options for Git repositories

4-3-6 Bitbucket Cloud

4-3-7 Bitbucket Server

4-3-8 TFVC

4-3-9 Subversion

4-3-10 Multiple repositories

## [4-4 Build history](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/history?view=azure-devops)



## 4-5 Triggers

### [4-5-1 :ballot_box_with_check: Type of triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops)

Use triggers to run a pipeline automatically. Azure Pipelines supports many types of triggers. Based on your pipeline's type, select the appropriate trigger from the list below:

**Classic build pipelines and YAML pipelines**

Continuous integration (CI) triggers vary based on the type of repository you build in your pipeline.

* [CI triggers in Azure Repos Git](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/azure-repos-git?view=azure-devops#ci-triggers)
* [CI triggers in GitHub](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops#ci-triggers)
* [CI triggers in Bitbucket Cloud](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/bitbucket?view=azure-devops#ci-triggers)
* [CI triggers in TFVC](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/tfvc?view=azure-devops#ci-triggers)

Pull request validation (PR) triggers also vary based on the type of repository.

* [PR triggers in Azure Repos Git](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/azure-repos-git?view=azure-devops#pr-triggers)
* [PR triggers in GitHub](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops#pr-triggers)
* [PR triggers in Bitbucket Cloud](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/bitbucket?view=azure-devops#pr-triggers)

[Gated check-in](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/tfvc?view=azure-devops#gated) is supported for TFVC repositories.

[Comment triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops#comment-triggers) are supported only for GitHub repositories.

[Scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops) are independent of the repository and allow you to run a pipeline according to a schedule.

[Pipeline triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?view=azure-devops) in YAML pipelines and [build completion triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers-classic?view=azure-devops) in classic build pipelines allow you to trigger one pipeline upon the completion of another.

**Branch consideration for triggers in YAML pipelines**

YAML pipelines can have different versions of the pipeline in different branches, which can affect which version of the pipeline's triggers are evaluated and which version of the pipeline should run.

| Trigger type                                                 | Pipeline YAML version                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| CI triggers (`trigger`)                                      | The version of the pipeline in the pushed branch is used.    |
| PR triggers (`pr`)                                           | The version of the pipeline in the source branch for the pull request is used. |
| GitHub pull request comment triggers                         | The version of the pipeline in the source branch for the pull request is used. |
| Scheduled triggers                                           | See [Branch considerations for scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?tabs=yaml&view=azure-devops#branch-considerations-for-scheduled-triggers). |
| Pipeline completion triggers                                 | See [Branch considerations for pipeline completion triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?tabs=yaml&view=azure-devops#branch-considerations). |
| [Azure Pipelines Action](https://github.com/marketplace/actions/azure-pipelines-action) | See [Branch considerations](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/github-actions?view=azure-devops#branch-considerations). |

**Classic release pipelines**

[Continuous deployment triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops#release-triggers) help you start classic releases after a classic build or YAML pipeline completes.

[Scheduled release triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops#scheduled-triggers) allow you to run a release pipeline according to a schedule.

[Pull request release triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops) are used to deploy a pull request directly using classic releases.

[Stage triggers in classic release](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops#env-triggers) are used to configure how each stage in a classic release is triggered.

### [4-5-2 :ballot_box_with_check: Scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml)

Azure Pipelines provides several types of triggers to configure how your pipeline starts.

1. Scheduled triggers start your pipeline based on a schedule, such as a nightly build. This article provides guidance on using scheduled triggers to run your pipelines based on a schedule.
2. Event-based triggers start your pipeline in response to events, such as creating a pull request or pushing to a branch. For information on using event-based triggers, see [Triggers in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops).

You can combine scheduled and event-based triggers in your pipelines, for example to validate the build every time a push is made ([CI trigger](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops#ci-triggers)), when a pull request is made ([PR trigger](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops#pr-triggers)), and a nightly build (Scheduled trigger). If you want to build your pipeline only on a schedule, and not in response to event-based triggers, ensure that your pipeline doesn't have any other triggers enabled. For example, YAML pipelines in a GitHub repository have CI triggers and PR triggers enabled by default. For information on disabling default triggers, see [Triggers in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops) and navigate to the section that covers your repository type.

**Scheduled triggers**

* <u>YAML</u>

  > :information_source: Important
  >
  > Scheduled triggers defined using the pipeline settings UI take precedence over YAML scheduled triggers.
  >
  > If your YAML pipeline has both YAML scheduled triggers and UI defined scheduled triggers, only the UI defined scheduled triggers are run. To run the YAML defined scheduled triggers in your YAML pipeline, you must remove the scheduled triggers defined in the pipeline settings UI. Once all UI scheduled triggers are removed, a push must be made in order for the YAML scheduled triggers to start being evaluated.
  >
  > To delete UI scheduled triggers from a YAML pipeline, see [UI settings override YAML scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/troubleshooting/troubleshooting?view=azure-devops#ui-settings-override-yaml-scheduled-triggers).

  Scheduled triggers configure a pipeline to run on a schedule defined using [cron syntax](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml#cron-syntax).

  ```yaml
  schedules:
  - cron: string # cron syntax defining a schedule
    displayName: string # friendly name given to a specific schedule
    branches:
      include: [ string ] # which branches the schedule applies to
      exclude: [ string ] # which branches to exclude from the schedule
    always: boolean # whether to always run the pipeline or only if there have been source code changes since the last successful scheduled run. The default is false.
  ```

  Scheduled pipelines in YAML have the following constraints.

  * The time zone for cron schedules is UTC.
  * If you specify an `exclude` clause without an `include` clause for `branches`, it's equivalent to specifying `*` in the `include` clause.
  * You can't use pipeline variables when specifying schedules.
  * If you use [templates in your YAML file](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops), then the schedules must be specified in the main YAML file and not in the template files.

* <u>Classic</u>

  Select the days and times when you want to run the build using the classic editor.
  
  If your repository is Azure Repos Git, GitHub, or Other Git, then you can also specify branches to include and exclude. If you want to use wildcard characters, then type the branch specification (for example, `features/modules/*`) and then press Enter.
  
  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-india.png?view=azure-devops)

**Examples**

* <u>YAML</u>

  The following example defines two schedules:

  ```yaml
  schedules:
  - cron: "0 0 * * *"
    displayName: Daily midnight build
    branches:
      include:
      - main
      - releases/*
      exclude:
      - releases/ancient/*
  - cron: "0 12 * * 0"
    displayName: Weekly Sunday build
    branches:
      include:
      - releases/*
    always: true
  ```

  The first schedule, **Daily midnight build**, runs a pipeline at midnight every day, but only if the code has changed since the last successful scheduled run, for `main` and all `releases/*` branches, except the branches under `releases/ancient/*`.

  he second schedule, **Weekly Sunday build**, runs a pipeline at noon on Sundays, whether the code has changed or not since the last run, for all `releases/*` branches.

  > :notes:Note
  >
  > The time zone for cron schedules is UTC, so in these examples, the midnight build and the noon build are at midnight and noon in UTC.

  For more examples, see [Migrating from the classic editor](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml#migrating-from-the-classic-editor).

* <u>Classic</u>

  <u>Example: Nightly build of Git repo in multiple time zones</u>
  
  In this example, the classic editor scheduled trigger has two entries, which produce the following builds.
  
  * Every Monday - Friday at 3:00 AM (UTC + 5:30 time zone), build branches that meet the `features/india/*` branch filter criteria
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-india.png?view=azure-devops)
  
  * Every Monday - Friday at 3:00 AM (UTC - 5:00 time zone), build branches that meet the `features/nc/*` branch filter criteria
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-nc.png?view=azure-devops)
  
  <u>Example: Nightly build with different frequencies</u>
  
  In this example, the classic editor scheduled trigger has two entries, producing the following builds.
  
  * Every Monday - Friday at 3:00 AM UTC, build branches that meet the `main` and `releases/*` branch filter criteria
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-week-day-night.png?view=azure-devops)
  
  * Every Sunday at 3:00 AM UTC, build the `releases/lastversion` branch, even if the source or pipeline hasn't changed
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-weekly-night.png?view=azure-devops)

**Cron syntax**

* <u>YAML</u>

  Each Azure Pipelines scheduled trigger cron expression is a space-delimited expression with five entries in the following order.

  ```shell
  mm HH DD MM DW
   \  \  \  \  \__ Days of week
    \  \  \  \____ Months
     \  \  \______ Days
      \  \________ Hours
       \__________ Minutes
  ```

  | Field        | Accepted values                                              |
  | :----------- | :----------------------------------------------------------- |
  | Minutes      | 0 through 59                                                 |
  | Hours        | 0 through 23                                                 |
  | Days         | 1 through 31                                                 |
  | Months       | 1 through 12, full English names, first three letters of English names |
  | Days of week | 0 through 6 (starting with Sunday), full English names, first three letters of English names |

  Values can be in the following formats.

  | Format          | Example          | Description                                                  |
  | :-------------- | :--------------- | :----------------------------------------------------------- |
  | Wildcard        | `*`              | Matches all values for this field                            |
  | Single value    | `5`              | Specifies a single value for this field                      |
  | Comma delimited | `3,5,6`          | Specifies multiple values for this field. Multiple formats can be combined, like `1,3-6` |
  | Ranges          | `1-3`            | The inclusive range of values for this field                 |
  | Intervals       | `*/4` or `1-5/2` | Intervals to match for this field, such as every fourth value or the range 1-5 with a step interval of 2 |

  | Example                                              | Cron expression                                              |
  | :--------------------------------------------------- | :----------------------------------------------------------- |
  | Build every Monday, Wednesday, and Friday at 6:00 PM | `0 18 * * Mon,Wed,Fri`, `0 18 * * 1,3,5`, or `0 18 * * 1-5/2` |
  | Build every 6 hours                                  | `0 0,6,12,18 * * *`, `0 */6 * * *` or `0 0-18/6 * * *`       |
  | Build every 6 hours starting at 9:00 AM              | `0 9,15,21 * * *` or `0 9-21/6 * * *`                        |

  For more information on supported formats, see [Crontab Expression](https://github.com/atifaziz/NCrontab/wiki/Crontab-Expression).

* <u>Classic</u>

  Classic schedules are defined using a graphical editor instead of cron syntax. For information on defining classic schedules, see [Examples](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=classic#examples).

**Scheduled runs view**

* <u>YAML</u>

  You can view a preview of upcoming scheduled builds by choosing **Scheduled runs** from the context menu on the [pipeline details page](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops#view-pipeline-details) for your pipeline.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-runs-menu.png?view=azure-devops)

  After you create or update your scheduled triggers, you can verify them using this view.

  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-runs.png?view=azure-devops)

  This example displays the scheduled runs for the following schedule.

  ```yaml
  schedules:
  - cron: "0 0 * * *"
    displayName: Daily midnight build
    branches:
      include:
      - main
  ```

  The **Scheduled runs** windows displays the times converted to the local time zone set on the computer used to browse to the Azure DevOps portal. This example displays a screenshot taken in the EST time zone.

* <u>Classic</u>

  You can view a preview of upcoming scheduled builds by choosing **Scheduled runs** from the context menu on the [pipeline details page](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/multi-stage-pipelines-experience?view=azure-devops#view-pipeline-details) for your pipeline.
  
  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-runs-menu-classic.png?view=azure-devops)
  
  After you create or update your scheduled triggers, you can verify them using this view.
  
  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-runs-classic.png?view=azure-devops)

**Running even when there are no code changes**

By default, your pipeline doesn't run as scheduled if there have been no code changes since the last successful scheduled run. For instance, consider that you've scheduled a pipeline to run every night at 9:00pm. During the weekdays, you push various changes to your code. The pipeline runs as per schedule. During the weekends, you don't make any changes to your code. If there have been no code changes since the scheduled run on Friday, then the pipeline doesn't run as scheduled during the weekend.

* <u>YAML</u>

  To force a pipeline to run even when there are no code changes, you can use the `always` keyword.

  ```yaml
  schedules:
  - cron: ...
    ...
    always: true
  ```

* <u>Classic</u>

  To configure the scheduled pipeline to build only if there has been a change since the last build, check **Only schedule builds if the source or pipeline has changed**.
  
  ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-india.png?view=azure-devops)

**Limits on the number of scheduled runs**

There are certain limits on how often you can schedule a pipeline to run. These limits have been put in place to prevent misuse of Azure Pipelines resources, particularly the Microsoft-hosted agents. This limit is around 1000 runs per pipeline per week.

**Migrating from the classic editor**

The following examples show you how to migrate your schedules from the classic editor to YAML.

* **<u>Example: Nightly build of Git repo in multiple time zones</u>**

  * Every Monday - Friday at 3:00 AM (UTC + 5:30 time zone), build branches that meet the `features/india/*` branch filter criteria

    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-india.png?view=azure-devops)

  * Every Monday - Friday at 3:00 AM (UTC - 5:00 time zone), build branches that meet the `features/nc/*` branch filter criteria

    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-nc.png?view=azure-devops)

  The equivalent YAML scheduled trigger is:

  ```yaml
  schedules:
  - cron: "30 21 * * Sun-Thu"
    displayName: M-F 3:00 AM (UTC + 5:30) India daily build
    branches:
      include:
      - /features/india/*
  - cron: "0 8 * * Mon-Fri"
    displayName: M-F 3:00 AM (UTC - 5) NC daily build
    branches:
      include:
      - /features/nc/*
  ```

  In the first schedule, **M-F 3:00 AM (UTC + 5:30) India daily build**, the cron syntax (`mm HH DD MM DW`) is `30 21 * * Sun-Thu`.

  * Minutes and Hours - `30 21` - This maps to `21:30 UTC` (`9:30 PM UTC`). Since the specified time zone in the classic editor is **UTC + 5:30**, we need to subtract 5 hours and 30 minutes from the desired build time of 3:00 AM to arrive at the desired UTC time to specify for the YAML trigger.
  * Days and Months are specified as wildcards since this schedule doesn't specify to run only on certain days of the month or on a specific month.
  * Days of the week - `Sun-Thu` - because of the timezone conversion, for our builds to run at 3:00 AM in the UTC + 5:30 India time zone, we need to specify starting them the previous day in UTC time. We could also specify the days of the week as `0-4` or `0,1,2,3,4`.

  In the second schedule, **M-F 3:00 AM (UTC - 5) NC daily build**, the cron syntax is `0 8 * * Mon-Fri`.

  * Minutes and Hours - `0 8` - This maps to `8:00 AM UTC`. Since the specified time zone in the classic editor is **UTC - 5:00**, we need to add 5 hours from the desired build time of 3:00 AM to arrive at the desired UTC time to specify for the YAML trigger.
  * Days and Months are specified as wildcards since this schedule doesn't specify to run only on certain days of the month or on a specific month.
  * Days of the week - `Mon-Fri` - Because our timezone conversions don't span multiple days of the week for our desired schedule, we don't need to do any conversion here. We could also specify the days of the week as `1-5` or `1,2,3,4,5`.

  > :notebook_with_decorative_cover: Important
  >
  > The UTC time zones in YAML scheduled triggers don't <u>account for</u>[^4-5-2-1] <u>daylight savings time</u>[^4-5-2-2].
  >
  > :tipping_hand_man: Tip
  >
  > When using 3 letter days of the week and wanting a span of multiple days through Sun, Sun should be considered the first day of the week e.g. For a schedule of midnight EST, Thursday to Sunday, the cron syntax is `0 5 * * Sun,Thu-Sat`

* <u>**Example: Nightly build with different frequencies**</u>

  In this example, the classic editor scheduled trigger has two entries, producing the following builds.
  
  * Every Monday - Friday at 3:00 AM UTC, build branches that meet the `main` and `releases/*` branch filter criteria
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-week-day-night.png?view=azure-devops-2022)
  
  * Every Sunday at 3:00 AM UTC, build the `releases/lastversion` branch, even if the source or pipeline hasn't changed
  
    ![](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/media/triggers/scheduled-trigger-git-weekly-night.png?view=azure-devops-2022)
  
  The equivalent YAML scheduled trigger is:
  
  ```yaml
  schedules:
  - cron: "0 3 * * Mon-Fri"
    displayName: M-F 3:00 AM (UTC) daily build
    branches:
      include:
      - main
      - /releases/*
  - cron: "0 3 * * Sun"
    displayName: Sunday 3:00 AM (UTC) weekly latest version build
    branches:
      include:
      - /releases/lastversion
    always: true
  ```
  
  In the first schedule, **M-F 3:00 AM (UTC) daily build**, the cron syntax is `0 3 * * Mon-Fri`.
  
  * Minutes and Hours - `0 3` - This maps to `3:00 AM UTC`. Since the specified time zone in the classic editor is **UTC**, we don't need to do any time zone conversions.
  * Days and Months are specified as wildcards since this schedule doesn't specify to run only on certain days of the month or on a specific month.
  * Days of the week - `Mon-Fri` - because there's no timezone conversion, the days of the week map directly from the classic editor schedule. We could also specify the days of the week as `1,2,3,4,5`.
  
  In the second schedule, **Sunday 3:00 AM (UTC) weekly latest version build**, the cron syntax is `0 3 * * Sun`.
  
  * Minutes and Hours - `0 3` - This maps to `3:00 AM UTC`. Since the specified time zone in the classic editor is **UTC**, we don't need to do any time zone conversions.
  * Days and Months are specified as wildcards since this schedule doesn't specify to run only on certain days of the month or on a specific month.
  * Days of the week - `Sun` - Because our timezone conversions don't span multiple days of the week for our desired schedule, we don't need to do any conversion here. We could also specify the days of the week as `0`.
  * We also specify `always: true` since this build is scheduled to run whether or not the source code has been updated.

**FAQ**

* <u>I defined a schedule in the YAML file. But it didn't run. What happened?</u>

  * Check the next few runs that Azure Pipelines has scheduled for your pipeline. You can find these runs by selecting the **Scheduled runs** action in your pipeline. The list is filtered down to only show you the upcoming few runs over the next few days. If this doesn't meet your expectation, it's probably the case that you've mistyped your cron schedule, or you don't have the schedule defined in the correct branch. Read the topic above to understand how to configure schedules. Reevaluate your cron syntax. All the times for cron schedules are in UTC.
  * Make a small trivial change to your YAML file and push that update into your repository. If there was any problem in reading the schedules from the YAML file earlier, it should be fixed now.
  * If you have any schedules defined in the UI, then your YAML schedules aren't honored. Ensure that you don't have any UI schedules by navigating to the editor for your pipeline and then selecting **Triggers**.
  * There's a limit on the number of runs you can schedule for a pipeline. Read more about [limits](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops-2022&tabs=yaml#limits).
  * If there are no changes to your code, they Azure Pipelines may not start new runs. Learn how to [override](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops-2022&tabs=yaml#always) this behavior.

* <u>My YAML schedules were working fine. But, they stopped working now. How do I debug this?</u>

  * If you didn't specify `always:true`, your pipeline won't be scheduled unless there are any updates made to your code. Check whether there have been any code changes and how you [configured the schedules](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops-2022&tabs=yaml#always).
  * There's a [limit](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops-2022&tabs=yaml#limits) on how many times you can schedule your pipeline. Check if you've exceeded those limits.
  * Check if someone enabled more schedules in the UI. Open the editor for your pipeline, and select **Triggers**. If they defined schedules in the UI, then your YAML schedules won't be honored.
  * Check if your pipeline is paused or disabled. Select **Settings** for your pipeline.
  * Check the next few runs that Azure Pipelines has scheduled for your pipeline. You can find these runs by selecting the **Scheduled runs** action in your pipeline. If you don't see the schedules that you expected, make a small trivial change to your YAML file, and push the update to your repository. This should resync the schedules.
  * If you use GitHub for storing your code, it's possible that Azure Pipelines may have been throttled by GitHub when it tried to start a new run. Check if you can start a new run manually.

* <u>My code hasn't changed, yet a scheduled build is triggered. Why?</u>

  * You might have enabled an option to **always** run a scheduled build even if there are no code changes. If you use a YAML file, verify the syntax for the schedule in the YAML file. If you use classic pipelines, verify if you checked this option in the scheduled triggers.
  * You might have updated the build pipeline or some property of the pipeline. This will cause a new run to be scheduled even if you haven't updated your source code. Verify the **History** of changes in the pipeline using the classic editor.
  * You might have updated the service connection used to connect to the repository. This will cause a new run to be scheduled even if you haven't updated your source code.
  * Azure Pipelines first checks if there are any updates to your code. If Azure Pipelines is unable to reach your repository or get this information, it will create an [informational run](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/information-run?view=azure-devops-2022). It's a dummy build to let you know that Azure Pipelines is unable to reach your repository.
  * Your pipeline may not have a completely successful build. In order to determine whether to schedule a new build or not, Azure DevOps looks up the last completely successful scheduled build. If it doesn't find one, it triggers a new scheduled build. Partially successful scheduled builds aren't considered successful, so if your pipeline only has partially successful builds, Azure DevOps will trigger scheduled builds, even if your code hasn't changed.

* <u>I see the planned run in the Scheduled runs panel. However, it doesn't run at that time. Why?</u>

  * The **Scheduled runs** panel shows all potential schedules. However, it may not actually run unless you have made real updates to the code. To force a schedule to always run, ensure that you have set the **always** property in the YAML pipeline, or checked the option to always run in a classic pipeline.

* <u>Schedules defined in YAML pipeline work for one branch but not the other. How do I fix this?</u>

  Schedules are defined in YAML files, and these files are associated with branches. If you want a pipeline to be scheduled for a particular branch, say `features/X`, then make sure that the YAML file **in that branch** has the cron schedule defined in it, and that it has the correct branch inclusions[^4-5-2-3] for the schedule. The YAML file in the `features/X` branch should have the following `schedule` in this example:

  ```yaml
  schedules:
  - cron: "0 12 * * 0" 	# replace with your schedule
    branches:
      include:
      - features/X
  ```

  For more information, see [Branch considerations for scheduled triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops-2022&tabs=yaml#branch-considerations-for-scheduled-triggers).

### [4-5-3 Pipeline completion triggers](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers?view=azure-devops)



### [4-5-4 Build completion triggers (classic)](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers-classic?view=azure-devops)



### [4-5-5 Release triggers (classic)](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/triggers?view=azure-devops)



## 4-6 Tasks & templates

### [4-6-1 Task types & usage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/tasks?view=azure-devops&tabs=yaml)

A **task** is the building block for defining automation in a pipeline. A task is simply a packaged script or procedure that has been abstracted with a set of inputs.

When you add a task to your pipeline, it may also add a set of **demands** to the pipeline. The demands define the prerequisites that must be installed on the [agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops) for the task to run. When you run the build or deployment, an agent that meets these demands will be chosen.

When you run a [job](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops), all the tasks are run in sequence, one after the other. To run the same set of tasks in parallel on multiple agents, or to run some tasks without using an agent, see [jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops).

By default, all tasks run in the same context, whether that's on the [host](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops) or in a [job container](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=azure-devops). You may optionally use [step targets](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/tasks?view=azure-devops&tabs=yaml#step-target) to control context for an individual task.

Learn more about how to specify properties for a task with the [built-in tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/?view=azure-devops).

**Custom tasks**

We provide some [built-in tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/?view=azure-devops) to enable fundamental build and deployment scenarios. We have also provided guidance for [creating your own custom task](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-build-task?view=azure-devops).

In addition, [Visual Studio Marketplace](https://marketplace.visualstudio.com/azuredevops) offers a number of extensions; each of which, when installed to your subscription or collection, extends the task catalog with one or more tasks. Furthermore, you can write your own [custom extensions](https://docs.microsoft.com/en-us/azure/devops/integrate/?view=azure-devops) to add tasks to Azure Pipelines or TFS.

In YAML pipelines, you refer to tasks by name. ==If a name matches both an in-box task and a custom task, the in-box task will take precedence. You can use the task GUID or a fully qualified name for the custom task to avoid this risk:==

```yaml
steps:
- task: myPublisherId.myExtensionId.myContributionId.myTaskName@1 #format exmple
- task: qetza.replacetokens.replacetokens-task.replacetoken@3 #working example
```

To find `myPublisherId` and `myExtensionId`, select **Get** on a task in the marketplace. The values after the `itemName` in your URL string are `myPublisherId` and `myExtensionId`. You can also find the fully qualified name by adding the task to a [Release pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/releases?view=azure-devops) and selecting **View YAML** when editing the task.

**Task versions**



**Task control options**



**Environment variables**



**Build tool installers (Azure Pipelines)**



**Related articles**



**Help and support**



### 4-6-2 Task groups (classic)



### [4-6-3 Template types & usage](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops)



### [4-6-4 Add a custom task extension](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-build-task?toc=%2Fazure%2Fdevops%2Fpipelines%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fpipelines%2Fbreadcrumb%2Ftoc.json&view=azure-devops)



### [4-6-5 Upload tasks to project collection](https://docs.microsoft.com/en-us/azure/devops/extend/develop/upload-tasks?toc=%2Fazure%2Fdevops%2Fpipelines%2Ftoc.json&bc=%2Fazure%2Fdevops%2Fpipelines%2Fbreadcrumb%2Ftoc.json&view=azure-devops)



## 4-7 Jobs & stages

### 4-7-1 Specify jobs in your pipeline

### 4-7-2 Define container jobs

### 4-7-3 Add stages, dependencies & conditions

### 4-7-4 Deployment jobs

### 4-7-5 Author a custom pipeline decorator

### 4-7-6 Pipeline decorator context

### 4-7-7 Specify conditions

### 4-7-8 Specify demands

## 4-8 Library, variables & secure files



## 4-9 Approvals, checks, & gates



## 4-10 Pipeline runs



## 4-11 Pipeline reports



## 4-12 Manage pipelines with Azure CLI



## 4-13 Clone or import a pipeline

## 4-14 Configure pipelines to support work tracking

## 4-15 Pipeline default branch

## 4-16 DevOps Starter



# 5 Ecosystems & integration

6 Build apps

7 Run pipeline tests

8 Deploy apps

9 Deploy apps (Classic)

10 Deploy to Azure

11 Deploy apps using containers

# 12 Consume & publish artifacts

## [12-1 Artifacts in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/artifacts/artifacts-overview?view=azure-devops&tabs=nuget)

Azure Artifacts enable developers to consume and publish different types of packages to Artifacts feeds and public registries such as NuGet.org and npmjs.com. You can use Azure Artifacts in conjunction with Azure Pipelines to deploy packages, publish build artifacts, or integrate files between your pipeline stages to build, test, or deploy your application.

### Supported artifact types

| Artifact type      | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| Build artifacts    | The files generated by a build. Example: .dll, .exe, or .PDB files |
| NuGet              | Publish NuGet packages to Azure Artifacts feeds or public registries such as nuget.org |
| npm                | Publish npm packages to Azure Artifacts feeds or public registries such as npmjs.com |
| Maven              | Publish Maven packages to Azure Artifacts feeds.             |
| PypI               | Publish Python packages to Azure Artifacts feeds or PyPI.org |
| Universal packages | Publish Universal Packages to Azure Artifacts feeds.         |
| Symbols            | Symbol files contain debugging information about the compiled executables. You can publish symbols |



**Publish and consume artifacts**



## [12-2 Publish & download pipeline artifacts](https://docs.microsoft.com/en-us/azure/devops/pipelines/artifacts/pipeline-artifacts?view=azure-devops&tabs=yaml)



## [12-3 Build artifacts](https://docs.microsoft.com/en-us/azure/devops/pipelines/artifacts/build-artifacts?view=azure-devops&tabs=yaml)



## [12-4 Release pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/releases?view=azure-devops)



## [12-5 Release artifacts and artifact sources](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/artifacts?view=azure-devops)

12-6 NuGet

12-7 npm

12-8 Maven

12-9 Python

12-10 Universal Packages

12-11 Symbols

12-12 Restore NuGet packages

12-13 Publish NuGet packages with Jenkins

# 13 Create & use resources

# 14 Manage agents & agent pools

## [14-1 About agents & agent pools](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops-2022&tabs=browser)

To build your code or deploy your software using Azure Pipelines, you need at least one agent. As you add more code and people, you'll eventually need more.

When your pipeline runs, the system begins one or more jobs. ==An agent is computing infrastructure with installed agent software that runs one job at a time.==

Jobs can be run [directly on the host machine of the agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops-2022) or [in a container](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=azure-devops-2022).

**Self-hosted agents**

An agent that you set up and manage on your own to run jobs is a **self-hosted agent**. You can use self-hosted agents in Azure Pipelines or Azure DevOps Server, formerly named Team Foundation Server (TFS). Self-hosted agents give you more control to install dependent software needed for your builds and deployments. Also, machine-level caches and configuration persist from run to run, which can boost speed.

> :information_source: Note
>
> Although multiple agents can be installed per machine, we strongly suggest to only install one agent per machine. Installing two or more agents may adversely affect performance and the result of your pipelines.

**Parallel jobs**



**Capabilities**

* **Browser**

  

* **Azure DevOps CLI**

  



**Communication**

* **Communication with TFS**

**Authentication**

* Personal Access Token (PAT):
* Integrated
* Negotiate
* Alternate

**Interactive vs. service**



**Agent account**



**Agent version and upgrades**



**FAQ**

* How do I make sure I have the latest v2 agent version?
* Can I update my v2 agents that are part of an Azure DevOps Server pool?
* Do self-hosted agents have any performance advantages over Microsoft-hosted agents?
* Can I install multiple self-hosted agents on the same machine?
* What’s the behavior of agents when the pipeline jobs are cancelled?
* How can I trigger agent updates programmatically for specific agent pool?
* URI Parameters

**Learn more**



14-2 Create & manage agent pools

14-3 Microsoft-hosted agents

14-4 Self-hosted Linux agents



15 Configure security & settings

Integrate with


[^1-1]: 几乎; 几近; 大约; 马上  almost
[^2-1]: **[only before noun]** used to talk about a particular person, thing, group of things etc without naming them or describing them exactly
[^3-2-1]: n.棒形纽扣; 套索扣; 转换键; 切换键   v.切换
[^3-2-2]: n.滑雪者，滑冰者; 会滚动之物 ; [棒]弧度不大的曲球; 滑块
[^3-2-3]: n.保留; 记忆力，保持力; 滞留，扣留; 闭尿  *formal* the act of keeping something
[^4-2-1]: n.智能感知
[^4-2-2]: [ˈpælɪt] n.调色板，颜料 a thin curved board that an artist uses to mix paints, holding it by putting his or her thumb through a hole at the edge
[^4-5-2-1]: 说明（原因、理由等）; 导致，引起; （在数量、比例上）占; 对…负责 to be the reason why something happens **SYN** explain
[^4-5-2-2]: 夏令时 time as adjusted to achieve longer evening daylight in summer by setting the clocks an hour ahead of the standard time.
[^4-5-2-3]: n.包含; 内含物 [countable] someone or something that has been included in a larger group