# audioreach-workflows

This repository contains reusable CI/CD workflow templates for the AudioReach organisation. It is designed to serve as a central source for best-practice GitHub Actions workflows, which can be referenced and reused across multiple AudioReach repositories.

## Usage

To use a workflow from this repository in your own repository, reference it in your workflow YAML file using the `uses` keyword. For example:

```yaml
name: Example Workflow
on:
  push:
    branches: [ main ]
jobs:
  build:
    uses: Audioreach/audioreach-workflows/.github/workflows/build.yml@master
    with:
      # Set workflow inputs here if required
    secrets:
      # Pass any required secrets here
```

> **Note:** Replace `build.yml` with the actual workflow file you want to use. Make sure to specify the correct branch or tag.


### Available Workflows

#### 1. `build.yml`
Reusable workflow for build instructions.

**Required Inputs:**
- `build_args` (string): Required build arguments.

**Optional Inputs:**
- `build_script` (string, default: `ci/build.sh`): Optional build script to run inside the docker container.
- `sync_script` (string, default: `ci/sync.sh`): Optional sync script to run inside the docker container.
- `apply_patch_script` (string, default: `ci/apply_patch.sh`): Optional script to apply patches before building.
- `config_script` (string, default: `ci/config.sh`): Optional script to configure the build environment.
- `event_name` (string, default: `${{ github.event_name }}`): Optional event name to use for building.
- `pr_ref` (string, default: `${{ github.event.pull_request.head.ref }}`): Optional pull request reference.
- `pr_repo` (string, default: `${{ github.event.pull_request.head.repo.full_name }}`): Optional pull request repository.
- `base_ref` (string, default: `${{ github.ref_name }}`): Optional base reference.

---

#### 2. `test.yml`
Triggers LAVA tests on the build.

**Inputs:**
No required or optional inputs defined for `workflow_call`.

---

#### 3. `process_image.yml`
Processes image for build URL.

**Optional Inputs:**
- `files_to_copy` (string, default: `ci/files_to_copy.sh`): Optional argument for specifying files to copy to the extracted precompiled meta-ar flat_build image.
- `event_name` (string, default: `${{ github.event_name }}`): Optional event name.
- `pr_ref` (string, default: `${{ github.event.pull_request.head.ref }}`): Optional pull request reference.
- `pr_repo` (string, default: `${{ github.event.pull_request.head.repo.full_name }}`): Optional pull request repository.
- `base_ref` (string, default: `${{ github.ref_name }}`): Optional base reference.

---

#### 4. `loading.yml`
Loads required parameters for subsequent jobs.

**Optional Inputs:**
- `build_options_file` (string, default: `ci/build_options.txt`): The file containing build options.
- `event_name` (string, default: `${{ github.event_name }}`): Optional event name.
- `pr_ref` (string, default: `${{ github.event.pull_request.head.ref }}`): Optional pull request reference.
- `pr_repo` (string, default: `${{ github.event.pull_request.head.repo.full_name }}`): Optional pull request repository.
- `base_ref` (string, default: `${{ github.ref_name }}`): Optional base reference.

---

#### 5. `qcom-preflight-checks.yml`
Runs code quality and compliance checkers. For more info check [qcom-actions](https://github.com/qualcomm/qcom-actions)

**Inputs:**
No required or optional inputs for the workflow itself, but the called job [qcom-reusable-workflows](https://github.com/qualcomm/qcom-reusable-workflows) accepts:
- `repolinter` (default: true)
- `semgrep` (default: true)
- `copyright-license-detector` (default: true)
- `pr-check-emails` (default: true)
- `dependency-review` (default: true)

**Secrets:**
- `SEMGREP_APP_TOKEN` (required for Semgrep checks)

_Update this list as new workflows are added._


## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) and [Code of Conduct](CODE-OF-CONDUCT.md) before submitting changes.

To add or update a workflow:

1. Fork this repository and create a new branch.
2. Add or modify workflow files under `.github/workflows/`.
3. Update this README with usage instructions if needed.
4. Open a pull request describing your changes.

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
