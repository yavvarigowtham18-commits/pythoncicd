# Python-CICD

A sample Python application with a Jenkins Pipeline for GitHub-based CI/CD.

## Project structure

```text
Python-CICD/
├── app.py
├── requirements.txt
├── Jenkinsfile
└── tests/
    └── test_app.py
```

## Run locally (Windows PowerShell)

```powershell
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe -m compileall -q app.py tests
.venv\Scripts\python.exe -m pytest tests
python app.py
```

## Jenkins setup

1. Push this project to a GitHub repository.
2. Create a Jenkins Pipeline job.
3. Select **Pipeline script from SCM** and **Git**.
4. Enter your repository URL and set the branch to `*/main`.
5. Set the script path to `Jenkinsfile`.
6. Ensure the Jenkins agent runs on Windows and has Python and Git available.
7. Configure a GitHub push webhook pointing to `https://YOUR-JENKINS-URL/github-webhook/`.
8. Ensure the Jenkins GitHub plugin is installed and the webhook endpoint is reachable.

The pipeline checks out source, creates a virtual environment, installs dependencies,
compiles Python files, runs pytest, and archives `dist/python-app.zip`.

## Notes

- The supplied Jenkinsfile uses Windows `bat` commands.
- For a private GitHub repository, configure Jenkins credentials and select them in the
  job's SCM configuration.
- `githubPush()` enables webhook-triggered builds when GitHub can reach Jenkins.
- For local-only Jenkins installations that GitHub cannot reach, configure SCM polling
  in the Jenkins job instead.
