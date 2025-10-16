# Workflows Documentation

This document provides details for all reusable workflows available in this repository.

---

## build.yml
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

**Example Usage:**
```yaml
jobs:
  build:
    uses: AudioReach/audioreach-workflows/.github/workflows/build.yml@main
    with:
      build_args: "--target release"
```

---

## test.yml
Triggers LAVA tests on the build artifact.

**Inputs:**
- No required or optional inputs for `workflow_call`.

**Example Usage:**
```yaml
jobs:
  test:
    uses: AudioReach/audioreach-workflows/.github/workflows/test.yml@main
```

---

## process_image.yml
Processes the build image for a given build URL.

**Optional Inputs:**
- `event_name` (string, default: `${{ github.event_name }}`): Optional event name.
- `pr_ref` (string, default: `${{ github.event.pull_request.head.ref }}`): Optional pull request reference.
- `pr_repo` (string, default: `${{ github.event.pull_request.head.repo.full_name }}`): Optional pull request repository.
- `base_ref` (string, default: `${{ github.ref_name }}`): Optional base reference.

**Example Usage:**
```yaml
jobs:
  process_image:
    uses: AudioReach/audioreach-workflows/.github/workflows/process_image.yml@main
```

---

## loading.yml
Loads required parameters for subsequent jobs.

**Optional Inputs:**
- `build_options_file` (string, default: `ci/build_options.txt`): The file containing build options.
- `event_name` (string, default: `${{ github.event_name }}`): Optional event name.
- `pr_ref` (string, default: `${{ github.event.pull_request.head.ref }}`): Optional pull request reference.
- `pr_repo` (string, default: `${{ github.event.pull_request.head.repo.full_name }}`): Optional pull request repository.
- `base_ref` (string, default: `${{ github.ref_name }}`): Optional base reference.

**Example Usage:**
```yaml
jobs:
  loading:
    uses: AudioReach/audioreach-workflows/.github/workflows/loading.yml@main
```

---

## qli-checkers.yml
Runs code quality and compliance checkers using Qualcomm's multi-checker action.

**Inputs:**
- No required or optional inputs for the workflow itself.
- The called job (`multi-checker.yml`) accepts:
  - `repolinter` (default: true)
  - `semgrep` (default: true)
  - `copyright-license-detector` (default: true)
  - `pr-check-emails` (default: true)

**Secrets:**
- `SEMGREP_APP_TOKEN` (required for Semgrep checks)

**Example Usage:**
```yaml
jobs:
  checker:
    uses: AudioReach/audioreach-workflows/.github/workflows/qli-checkers.yml@main
```

# Custom Actions Documentation

This repository uses several custom GitHub Actions under `.github/actions/`.

---

## aws-s3-exchanger
**Description:** Uploads and downloads files from AWS S3 buckets, and can generate pre-signed URLs for uploaded files.

**Inputs:**
- `s3_bucket` (**required**): S3 Bucket Name
- `local_file` (optional, default: `''`): Local file path to upload
- `download_filename` (**required**, default: `''`): Name of the file to download or upload
- `location` (**required**, default: `''`): Location (folder path) in the S3 bucket
- `mode` (**required**, default: `upload`): Operation mode, either `upload` or `download`

**Behavior:**
- In `upload` mode, uploads the specified file to S3 and generates a pre-signed URL (saved as `presigned_url.txt`).
- In `download` mode, downloads the specified file from S3 and sets permissions.

**Outputs:**
- `presigned_url.txt` (artifact, upload mode only): Contains the pre-signed URL for the uploaded file.

---

## build
**Description:** Executes the main build process inside a Docker container using a specified image.

**Inputs:**
- `docker_image` (**required**): Docker image to use for the build.

**Behavior:**
- Loads build arguments and environment variables, sources optional scripts, and runs the main build script inside the Docker container.

---

## build_docker_image
**Description:** Builds the Docker image used for CI jobs, using a Dockerfile from the `ar-image` repository.

**Outputs:**
- `image_name`: Name of the built Docker image (default: `ar-image:latest`).

**Behavior:**
- Checks out the Dockerfile, sets up Buildx, builds the image, and outputs the image name.

---

## loading
**Description:** Loads build parameters and options for downstream jobs by parsing a build options file.

**Inputs:**
- `build_options_file` (**required**, default: `ci/build_options.txt`): File containing build options (one per line).

**Outputs:**
- `build_args`: The parsed build arguments (all options joined as a single string).

**Behavior:**
- Reads the options file, joins lines into a single string, and sets as output for use in subsequent jobs.

---

## sync
**Description:** Syncs the repository or codebase before building or testing, based on the event type and refs.

**Inputs:**
- `event_name` (**required**): Event type that triggered the workflow (e.g., `pull_request_target`, `push`, `workflow_call`)
- `pr_ref` (optional): PR branch ref
- `pr_repo` (optional): PR repo full name
- `base_ref` (optional): Base branch ref

**Behavior:**
- Checks out the appropriate branch and repository depending on the event and refs provided, ensuring the correct codebase is used for the workflow.

---

## Usage Example
Each action is referenced in the workflows using the `uses:` keyword, e.g.:

```yaml
- name: Build docker Image
  uses: AudioReach/audioreach-workflows/.github/actions/build_docker_image@main
```

Refer to each action's `action.yml` or this documentation for detailed input/output information.
