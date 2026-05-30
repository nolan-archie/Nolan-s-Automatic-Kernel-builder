# Nolan's Automatic Kernel Builder

This project provides a fully automated CI/CD pipeline for building Android kernels with integrated **SukiSU-Ultra** and **SUSFS**. It's designed to keep your kernel up-to-date with upstream security and root features every week without manual intervention.

## 🚀 Key Features

*   **Weekly Automated Builds**: Scheduled via GitHub Actions (Every Monday at 00:00 UTC).
*   **Intelligent Upstream Sync**: Automatically fetches the latest commits from SukiSU-Ultra and SUSFS.
*   **Surgical Patching**: Custom engine to inject SUSFS dispatch logic into `supercall.c` while preserving device-specific bridge files.
*   **Automatic Changelogs**: Generates release notes based on fetched commits from dependencies.
*   **Resource Optimized**: Built-in script to maximize GitHub Actions runner disk space (~30GB+ freed).
*   **Dual-Branch Support**: Matrix builds for both standard and permissive SELinux branches.

## 📂 Project Structure

*   `.github/workflows/kernel_builder.yml`: The orchestrator that manages the build lifecycle, matrix, and releases.
*   `scripts/automate_updates.sh`: The logic engine for syncing, patching, and committing dependency updates.
*   `scripts/maximize_space.sh`: Cleanup utility for GitHub Actions runners.
*   `README_AUTOMATION.md`: Detailed technical documentation for maintaining the automation.

## 🛠 Setup Instructions

1.  **Repository Setup**:
    Copy the `.github/` and `scripts/` directories into your kernel source repository.

2.  **Configuration**:
    Update the `matrix` in `.github/workflows/kernel_builder.yml` to match your repository branches.

3.  **Permissions**:
    Ensure your GitHub repository has "Read and write permissions" enabled under *Settings > Actions > General > Workflow permissions*.

## 📖 How it Works

The workflow uses `git worktree` to create a clean, isolated build environment separate from the main repository. It performs a "Research -> Patch -> Build" cycle:
1.  **Maximize Space**: Removes pre-installed bloat from the runner.
2.  **Setup Legacy Env**: Provisions Python 2.7 (via `deadsnakes` PPA) required by older kernel build systems.
3.  **Update & Patch**: Clones upstreams, patches `supercall.c`, and commits changes back to the repo if updates are found.
4.  **Build**: Executes your `build.sh`.
5.  **Release**: Creates a GitHub Release with the compiled artifacts and the auto-generated changelog.

---
*Created for the A165F project and the Android kernel community.*
