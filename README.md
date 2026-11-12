# Remote Compute

A public workspace for builds and compute tasks impractical to run locally. GitHub Actions runs each task on a fresh hosted runner; this is not a persistent machine.

## Run a task

1. Open **Actions** and select **Remote Compute**.
2. Select **Run workflow** on the `main` branch.
3. Choose a mode, provide the command and optional working directory/artifact paths, then start the run.
4. Follow the job log and download artifacts from the completed run.

The workflow uses the standard GitHub-hosted `ubuntu-24.04` runner (4 vCPU, 16 GB RAM). Standard runners are free for public repositories, subject to GitHub usage policies and service limits.

## Inputs

- **Mode**: build, test, or custom. This labels the run; the command defines the work.
- **Command**: required Bash command. Non-zero exit fails the job.
- **Working directory**: relative to repository root; defaults to `.`. Paths outside the checkout are rejected.
- **Artifact paths**: optional newline-separated paths/globs relative to repository root. Only these are uploaded; retention is 7 days.
- **Timeout**: 1-360 minutes; defaults to 60.

Runs start from a clean checkout. Files created during a run are temporary unless uploaded. Put reusable code and scripts in the repository; place outputs in a directory such as `out/`.

## Public repository safety

Everything committed here is public. Never commit credentials, private source, personal data, or other material that should not be public. Do not put secrets in workflow inputs or commands; inputs and logs may be visible to repository users. The job has read-only repository contents permission and does not publish or deploy anything.

## Examples

CMake: mode `build`, working directory `.`, command `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build --parallel 4`, artifact `build/my-program`.

Script: mode `custom`, command `bash scripts/run-task.sh`, artifact `out/**`.

For specialized workloads, add a script and document its dependencies rather than relying on undocumented runner state.
