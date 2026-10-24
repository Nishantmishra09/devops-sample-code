# Exercise 8 Report — Jenkins Hello World Job

## 1. Aim
To create a shell script in a Git repository and configure a Jenkins Freestyle project to execute the script and display its output.

## 2. Objectives
- Create and execute a shell script.
- Understand the connection between source control and Jenkins.
- Configure a Jenkins Freestyle job.
- Execute a build and inspect Console Output.

## 3. Tools and Technologies
- Git:
- GitHub:
- Jenkins:
- Docker:
- Bash:

## 4. Implementation
### 4.1 Create and test the script
The `hello-world.sh` script contains:

```bash
#!/bin/bash
set -eu

echo "Hello, Jenkins!"
```

Local test result:
- Paste your observed output here.

### 4.2 GitHub repository
Repository name: `devops-sample-code`  
Repository URL: [paste your repository URL]

Explain how the script was committed and pushed to the `main` branch.

### 4.3 Jenkins Freestyle project
Job name: `HelloWorld`  
Description: `Hello World! Jenkins job`  
Source Code Management: Git  
Build step: Execute shell  
Command: `sh hello-world.sh`

### 4.4 Verification
Build number:  
Build result:  
Console output result:  

## 5. Screenshots
Insert your own screenshots:
1. `jenkins-job-success.png` — HelloWorld job showing successful build.
2. `jenkins-console-output.png` — Console Output showing `Hello, Jenkins!` and `Finished: SUCCESS`.
3. Optional: GitHub repository showing the script.

## 6. Result
Complete this section only after performing the actual build. State whether Jenkins checked out the repository, executed the shell script, printed the expected greeting, and finished successfully.

## 7. Conclusion
Summarize how a Jenkins Freestyle project can retrieve source code from GitHub and execute a shell script as part of a build.
