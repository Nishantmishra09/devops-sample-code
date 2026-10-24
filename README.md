# Exercise 8 — Jenkins Hello World Job

## Aim
Create a shell script in a Git repository and configure a Jenkins Freestyle project to execute it and display its output.

## Files
- `hello-world.sh` — shell script to execute.
- `README.md` — setup instructions and verification procedure.
- `SUBMISSION_REPORT_TEMPLATE.md` — report template and verification checklist.
- `screenshots/` — place your screenshots here after completing the exercise.

## Prerequisites
- Git installed on your computer
- GitHub account
- Docker Desktop and Jenkins running at `http://localhost:8080`
- Jenkins must run on a Linux environment/agent that supports Bash and the Freestyle **Execute shell** build step

> Important for this setup: if Jenkins is running in the official Linux Jenkins Docker container on Docker Desktop, `Execute shell` runs inside that Linux container. The Jenkins container must be able to reach GitHub. If Jenkins is installed on Windows directly, `Execute shell` may not behave like Windows PowerShell; use a Linux agent or a compatible shell configuration.

## 1. Test the script locally
Open a Bash terminal (Git Bash, WSL, or Linux). In this folder, run:

```bash
chmod +x hello-world.sh
./hello-world.sh
```

Expected output:

```text
Hello, Jenkins!
```

On Windows PowerShell, you can inspect the script but should run it using Git Bash or WSL for a local test.

## 2. Create the GitHub repository
Create a **public** GitHub repository named `devops-sample-code`. Do not initialize it with a README if you plan to push this folder as the first commit.

Repository URL format:

```text
https://github.com/YOUR_USERNAME/devops-sample-code.git
```

Replace `YOUR_USERNAME` with your GitHub username.

## 3. Push this script to GitHub
Open PowerShell or Git Bash in this extracted exercise folder. Replace the remote URL with your actual repository URL.

If this folder is not already a Git repository:

```bash
git init
git add hello-world.sh README.md SUBMISSION_REPORT_TEMPLATE.md
git status
git commit -m "Add hello-world.sh"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/devops-sample-code.git
git push -u origin main
```

If Git says the remote `origin` already exists, inspect it first:

```bash
git status
git remote -v
```

Do not run `git init` or `git remote add origin` repeatedly without checking the existing repository state. Authenticate using Git Credential Manager, GitHub CLI, or another secure method. Do not put a personal access token in the repository, script, or README.

If the GitHub repository already contains a README or other commit, Git may reject the first push because the histories differ. For this exercise, the simplest route is to create an empty repository or follow Git's guidance to reconcile the histories.

## 4. Confirm Jenkins is running
Open:

http://localhost:8080

Sign in and confirm the Jenkins dashboard is accessible.

## 5. Create a Freestyle project
1. On the Jenkins dashboard, click **New Item**.
2. Enter `HelloWorld`.
3. Select **Freestyle project** and click **OK**.
4. In Description, enter: `Hello World! Jenkins job`.
5. Under **Source Code Management**, select **Git**.
6. In **Repository URL**, enter your actual repository URL, for example:
   `https://github.com/YOUR_USERNAME/devops-sample-code.git`
7. If the repository is public, credentials can normally remain **None**. If it is private, add Jenkins credentials securely; never paste a token into a README or script.
8. Under **Build Steps**, choose **Add build step → Execute shell**.
9. Enter:

```bash
sh hello-world.sh
```

10. Click **Save**.

### Important shell/agent note
The Jenkins controller/agent that executes the build must have Git and a POSIX-compatible shell. With the official Jenkins Linux container, `Execute shell` uses a Linux shell. If Jenkins reports `sh: not found` or cannot run the script, check the node/agent configuration and installed tools.

## 6. Run and verify the job
1. Open the `HelloWorld` job.
2. Click **Build Now**.
3. Wait for the build to finish.
4. Open the build number under **Build History**.
5. Click **Console Output**.

Expected output includes:

```text
Hello, Jenkins!
Finished: SUCCESS
```

The job is verified only after a real build succeeds and its Console Output contains the greeting and final success status.

## 7. Screenshot evidence
Capture your own screenshots and place them in `screenshots/`:
1. Jenkins `HelloWorld` job page showing successful build status.
2. Jenkins Console Output showing `Hello, Jenkins!` and `Finished: SUCCESS`.
3. Optional: GitHub repository page showing `hello-world.sh`.

Suggested filenames:
- `jenkins-job-success.png`
- `jenkins-console-output.png`
- `github-repository.png`

## 8. Troubleshooting
- **Repository URL invalid:** copy the HTTPS clone URL from the repository's GitHub page.
- **Authentication failed:** use Git Credential Manager or GitHub CLI; do not embed a token in the URL or files.
- **Could not find revision / checkout failed:** confirm the script was pushed to the `main` branch and the repository URL is correct.
- **`hello-world.sh` not found:** verify that `hello-world.sh` is at the repository root.
- **Shell command fails:** verify the build is running on a Linux node/agent with Bash or `sh`.
- **Build stays queued:** check **Build Executor Status** and available executors.
- **Build failed after checkout:** open **Console Output** and find the first error.

## 9. Stop Jenkins when done (optional)
To stop the container while preserving Jenkins configuration:

```powershell
docker stop jenkins
```

Start it again later with:

```powershell
docker start jenkins
```

## Result
After successful completion, the script is committed to the `devops-sample-code` GitHub repository, the Jenkins Freestyle project checks out the repository and executes `sh hello-world.sh`, and Console Output shows `Hello, Jenkins!` followed by `Finished: SUCCESS`.
