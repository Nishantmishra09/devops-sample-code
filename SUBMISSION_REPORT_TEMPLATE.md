# Exercise 9 Submission Report — Jenkins Multi-Stage Python Pipeline

**Name:**  
**Date:**  
**GitHub Repository:**  
**Jenkins Job:** `Python-MultiStage-Pipeline`

## Objective
Automate build, unit testing, mock deployment, application startup, and HTTP verification of a Python Flask app using a Jenkins Declarative Pipeline.

## Implementation
- Build: Created a Python virtual environment and installed dependencies.
- Test: Ran the `unittest` suite.
- Deploy: Copied the app and virtual environment to `python-app-deploy`.
- Run Application: Started Flask as a background process.
- Test Application: Verified HTTP status 200 and the expected response.
- Post actions: Stopped the process and archived the log.

## Evidence checklist
- [ ] GitHub repository showing the project files and `Jenkinsfile`.
- [ ] Jenkins stage view showing successful stages.
- [ ] Console output showing unit tests passed.
- [ ] Console output showing HTTP status `200` and expected response.
- [ ] `Finished: SUCCESS`.
- [ ] Archived `app.log` artifact, if available.

## Result
Complete after running the pipeline: ______________________________________
