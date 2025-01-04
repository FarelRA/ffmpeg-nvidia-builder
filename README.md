# FFmpeg Build with Nvidia Hardware Acceleration (GitHub Actions)

This repository contains a GitHub Actions workflow that automatically builds FFmpeg with Nvidia hardware acceleration enabled. The resulting binaries are uploaded as artifacts and can also be published as GitHub releases.

## Features

*   **Builds FFmpeg with Nvidia Hardware Acceleration:** Leverages Nvidia CUDA for enhanced encoding and decoding performance.
*   **Scheduled Builds:**  The workflow is scheduled to run every two weeks, ensuring you have relatively up-to-date builds.
*   **Manual Triggers:** You can manually trigger the workflow to build FFmpeg on demand.
*   **Caching:**  The `ffmpeg-build-script` directory is cached between runs, significantly speeding up subsequent builds by avoiding recompilation of libraries.
*   **Artifact Upload:** Built binaries (ffmpeg, ffprobe, ffplay) are uploaded as GitHub Actions artifacts, allowing you to download them directly from the workflow run.
*   **GitHub Releases:** When manually triggered with a tag, the workflow creates a GitHub release and uploads a zip archive containing the compiled binaries.

## How to Use

### Automatic Scheduled Builds

The workflow is configured to run automatically every other Monday at 00:00 UTC. You don't need to do anything; the latest build will be available in the "Actions" tab of your repository under the workflow run.

### Manual Trigger

You can manually trigger the workflow to build FFmpeg whenever you need it:

1. Go to the "Actions" tab in your GitHub repository.
2. Find the "Build FFmpeg with Nvidia Acceleration" workflow in the left sidebar.
3. Click the "Run workflow" button.
4. (Optional) If you want to create a GitHub release, enter a **Release Tag** (e.g., `v1.0.0` or a date like `2023-10-27`).

### Accessing the Built Binaries

**Artifacts:**

After a successful workflow run, you can download the built binaries from the "Artifacts" section of the workflow run summary. Look for an artifact named `ffmpeg-binaries`.

**Releases:**

If you manually triggered the workflow with a "Release Tag", a new release will be created in the "Releases" section of your repository. The release will contain a zip archive named `ffmpeg-bin.zip` containing the `ffmpeg`, `ffprobe`, and `ffplay` binaries.

## Workflow Details

The workflow performs the following steps:

1. **Checkout Repository:** Checks out the repository containing the workflow file.
2. **Install Dependencies:** Installs necessary packages like `git`, `nvidia-cuda-toolkit`, `nvidia-cuda-dev`, and `zip`.
3. **Get CUDA Compute Capability:** Detects the CUDA compute capability of the available Nvidia GPU.
4. **Cache ffmpeg-build-script:** Caches the `ffmpeg-build-script` directory to speed up subsequent builds.
5. **Clone ffmpeg-build-script:** Clones the `ffmpeg-build-script` repository if it's not already cached.
6. **Build FFmpeg:** Executes the build script with Nvidia hardware acceleration enabled.
7. **Upload Artifacts:** Uploads the compiled binaries as a workflow artifact.
8. **Create Release (Manual Trigger):** Creates a GitHub release if the workflow was manually triggered with a tag.
9. **Archive Binaries for Release (Manual Trigger):** Creates a zip archive of the `bin` directory for the release.
10. **Upload Release Asset (Manual Trigger):** Uploads the `ffmpeg-bin.zip` archive to the created release.

## Customization

You can customize the workflow to suit your needs:

*   **Schedule:** Modify the `cron` expression in the `schedule` trigger to change the build frequency.
*   **FFmpeg Build Options:** Edit the `build-ffmpeg` command in the "Build FFmpeg" step to add or remove FFmpeg build options. Refer to the `ffmpeg-build-script` documentation for available options.
*   **Dependencies:** Adjust the packages installed in the "Install dependencies" step if you need additional libraries.
*   **Ubuntu Version:** The workflow currently uses `ubuntu-22.04`. You can change this in the `runs-on` section of the `build` job.

## Disclaimer

This workflow relies on the `ffmpeg-build-script` project by markus-perl. Ensure you understand the licensing terms of FFmpeg and any dependencies.
