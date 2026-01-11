17. Continuous integration
==========================

1. Webhooks
-----------

A webhook is a lightweight, event-driven communication that automatically sends data between applications via HTTP. Triggered by specific events, 
webhooks automate communication between application programming interfaces (APIs) and can be used to activate workflows, such as in GitOps 
environments.

2. GitHub actions
-----------------

GitHub actions is a continuous integration/continuous deployment (CI/CD) delivery platform allowing you to automate your build, test, and deployment
pipeline. You can create workflows that build and test every pull request, or deploy merged pull requests to production.

You can configure a workflow to be triggered when an event occurs in your repository, such as a pull request being opened or an issue being created. Workflows are defined in YAML files stored in the `.github/workflows` directory of your repository.
It contains one or more jobs that can be run in series or in parallel. Each job runs its own virtual machine runner or in a container, and has one
or more steps that run a script you define or run an action, which is a reusable extension.

Workflows are configurable, automated processes that will run one or more jobs, defined in a YAML file. Workflows are defined under the 
`.github/workflows` directory in a repository. A repository can have multiple workflow files.

An event is a specific activity in a repositoru that triggers a workflow to run.

A job is a set of steps in a workflow that is executed on the same runner. Each step is either a shell script or an action. Steps are executed in
the order they are defined in the workflow file. You can configure a job's dependencies on other jobs in the workflow.

An action is a pre-defined, reusable set of jobs or code that performs specific tasks within a workflow.