To set up a basic GitHub Actions workflow, you only need a few essential components. Here's a quick guide to get you started:

---

### 🧱 1. **Workflow File Location**
- Must be placed in:  
  ```
  .github/workflows/<your-workflow-name>.yml
  ```

---

### 📝 2. **Basic Workflow Structure**

```yaml
name: Example Workflow

on: [push, pull_request]  # Triggers

jobs:
  build:
    runs-on: ubuntu-latest  # Runner environment

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run a script
        run: echo "Hello, GitHub Actions!"
```

---

### 🔑 3. **Key Components Explained**

| Section       | Purpose                                                                 |
|---------------|-------------------------------------------------------------------------|
| `name`        | Optional. A human-readable name for the workflow.                      |
| `on`          | Defines the event(s) that trigger the workflow (e.g., `push`, `pull_request`, `schedule`, `workflow_dispatch`). |
| `jobs`        | A collection of tasks that run in parallel or sequentially.            |
| `runs-on`     | Specifies the type of runner (e.g., `ubuntu-latest`, `windows-latest`). |
| `steps`       | Individual commands or actions to run inside a job.                    |
| `uses`        | Calls a prebuilt action from the GitHub Marketplace.                   |
| `run`         | Executes shell commands directly.                                      |

---

### 🧪 4. **Optional Enhancements**
- **Environment variables**: `env:`
- **Secrets**: `secrets.GITHUB_TOKEN`, `secrets.MY_SECRET`
- **Matrix builds**: Run jobs across multiple versions or OSes
- **Reusable workflows**: Use `workflow_call` to modularize logic

---

### 🏃‍♂️ GitHub Actions Runners: Usage and Setup Guidelines

To execute jobs defined in GitHub Actions workflows, runners are required. These runners are the compute environments where your jobs actually run.

#### 🖥️ GitHub-Hosted Runners
GitHub provides hosted runners (e.g., `ubuntu-latest`, `windows-latest`) that are suitable for:
- Lightweight jobs
- Short-duration tasks
- Workflows that do not require high computational resources

These runners are ideal for quick builds, tests, and automation tasks that complete within a few minutes.

#### 🏗️ Self-Hosted Runners
For jobs that are:
- Resource-intensive
- Long-running
- Dependent on specific hardware or internal infrastructure

It is recommended to use self-hosted runners. These are machines managed within your organization and configured to run GitHub Actions workflows.

You can also set your dev-compute machine as one of the self-hosted runners to get started with: see docs: [Setup self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners)

#### 🛠️ Setting Up Runners for a New Repository
To set up runners for a new repository, coordination with the internal IT team is required. They can:
- Add existing self-hosted runners to the repository
- Grant access to appropriate runner groups
- Assist in provisioning new runners if needed

For assistance with runner setup or access, please reach out to the following point of contact:
- **IT Team** : A ticket can be raised to the IT team for assistance.

They can guide you through the process and ensure the necessary runners are available for your workflows.

---

### 🔐 Accessing Secrets for GitHub Actions Workflows

Once you're familiar with the basic structure of GitHub Actions workflows and the necessary runners have been set up for your repository, the next step is often configuring access to required secrets.

#### 🔧 Secrets in GitHub Actions
Secrets are encrypted environment variables used to securely pass sensitive information—such as tokens, credentials, or API keys—into workflows. These are stored in the repository or organization settings and accessed in workflows using the `secrets` context.

#### 📦 AudioReach Use Case: LAVA Testing
In the case of the **AudioReach** project, we integrate with **LAVA** (Linaro Automated Validation Architecture) for testing. This requires a secure token, stored as:

```
secrets.LAVATOKEN
```

This token must be added to the repository’s secrets to enable LAVA-related workflows to function correctly.

#### 👥 Requesting Access or Adding Secrets
To add the `LAVATOKEN` or request access to it for a new repository, please contact the below:

- **Neeraj Jetha**
- **Nageswara Rao Bobba**

These individuals can help you with the process of adding the token to the repository’s secrets or granting you access to it if you need to run LAVA-related workflows.

They can assist with provisioning the token securely and ensuring your workflows have the necessary access.

### 🚀 Defining and Reusing Workflows with `audioreach-workflows`

With the setup complete—runners configured and secrets in place—you’re now ready to define your own GitHub Actions workflows or reuse existing ones from the centralized [`audioreach-workflows`](https://github.com/AudioReach/audioreach-workflows) repository.

#### 📚 Where to Start
- **Workflow Documentation**: Refer to the documentation in the [`audioreach-workflows`](workflows_usage.md) repository to understand the available workflows, their inputs, and usage patterns.
- **README Overview**: The repository’s README provides a high-level summary of the workflows and their intended use cases.

#### 🔁 Reusing Shared Workflows
The `audioreach-workflows` repository includes reusable workflows tailored for **audio build pipelines**. To use them:
- Reference the shared workflow in your repository’s workflow YAML.
- Pass the required scripts (e.g., `build.sh`, `sync.sh`, `apply_patch.sh`) as inputs.
- Define your own workflows for:
  - **Pre-merge jobs** (e.g., pull request validation)
  - **Post-merge jobs** (e.g., mainline builds or deployments)

#### 📥 Input Requirements
Make sure to review the **`scripts` input section** in the workflow documentation to understand what needs to be provided for successful execution.
