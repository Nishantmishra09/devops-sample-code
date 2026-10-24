# Exercise 9 — Jenkins Multi-Stage Pipeline: Python Flask Application

## Objective
Create a Jenkins Declarative Pipeline with Build, Test, Deploy, Run Application, and Test Application stages.

## Files
- `app.py` — Flask endpoint.
- `requirements.txt` — Python dependencies.
- `test_app.py` — unit test for `/`.
- `Jenkinsfile` — pipeline definition.
- `JENKINS_DOCKER_SETUP.md` — notes for Jenkins running in Docker.

## Prerequisites
- Jenkins is running.
- Pipeline and Git plugins are installed.
- The Jenkins agent has `git`, `python3`, and `python3-venv`.
- Jenkins can access the Git repository.

## 1. Test locally
From this project directory:

```bash
python3 --version
python3 -m venv .venv
source .venv/Scripts/activate
python -m pip install -r requirements.txt
python -m unittest discover -s . -p "test_*.py" -v
```

The activation command above is for Git Bash on Windows. On Linux/macOS use `source .venv/bin/activate`. In PowerShell use `.venv\\Scripts\\Activate.ps1`.

## 2. Push to GitHub
If you are adding this to your existing `devops-sample-code` repository, copy the exercise files into that repository's working directory. Do not run `git init` again if the repository already exists.

```bash
git status
git add app.py requirements.txt test_app.py Jenkinsfile .gitignore README.md JENKINS_DOCKER_SETUP.md SUBMISSION_REPORT_TEMPLATE.md
git commit -m "Add Python multi-stage Jenkins pipeline"
git push
```

For a brand-new repository, create it on GitHub first, then run:

```bash
git init
git add .
git commit -m "Add Python multi-stage Jenkins pipeline"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/devops-sample-code.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username. If `origin` already exists, inspect `git remote -v` rather than adding it again.

## 3. Configure Jenkins
1. Dashboard → **New Item**.
2. Name: `Python-MultiStage-Pipeline`; choose **Pipeline** → **OK**.
3. Under **Pipeline**, choose **Pipeline script from SCM**.
4. SCM: **Git**.
5. Repository URL: `https://github.com/YOUR_USERNAME/devops-sample-code.git`.
6. Branch Specifier: `*/main`.
7. Script Path: `Jenkinsfile`.
8. Save.

For a private repository, configure Git credentials.

## 4. Run and verify
Click **Build Now**, then open **Console Output**. A successful run should show:
- Unit tests passed (`OK`).
- HTTP status `200`.
- Response body `Hello, Jenkins Multi-Stage Pipeline!`.
- `Pipeline completed successfully!`
- `Finished: SUCCESS`.

The build may expose `python-app-deploy/app.log` under **Artifacts**.

## Stage summary
- **Build:** Creates `.venv` and installs dependencies.
- **Test:** Runs unit tests.
- **Deploy:** Copies application files and virtual environment to a mock deployment directory inside the Jenkins workspace.
- **Run Application:** Starts Flask on port 5000.
- **Test Application:** Makes an HTTP request and checks the status and response.
- **Post actions:** Stops the app process and archives its log.

## Troubleshooting
- `python3: not found`: install Python on the Jenkins agent or use the custom image guide.
- `venv`/`ensurepip` errors: install the matching `python3-venv` package.
- Git checkout failure: verify repository URL, branch, credentials, and Git plugin.
- Port 5000 is busy: stop the conflicting process or update the port in both `app.py` and `Jenkinsfile`.
- Pip install failure: check agent internet access and Python/pip compatibility.

This is a mock deployment in the Jenkins workspace, not production deployment. The app is stopped after the HTTP check to avoid leaving a background process running.
