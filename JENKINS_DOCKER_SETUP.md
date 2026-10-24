# Jenkins Docker setup with Python and venv

The official `jenkins/jenkins:lts` image may not include Python or `venv`. Installing packages in a running container is temporary. A custom image is more repeatable.

## Custom image
Create `Dockerfile.jenkins`:

```dockerfile
FROM jenkins/jenkins:lts
USER root
RUN apt-get update \\
    && apt-get install -y --no-install-recommends python3 python3-pip python3-venv git \\
    && rm -rf /var/lib/apt/lists/*
USER jenkins
```

Build it from the folder containing the Dockerfile:

```bash
docker build -f Dockerfile.jenkins -t jenkins-python:lts .
```

## Important: protect existing Jenkins data
Before replacing any existing container, inspect the container and mounts:

```bash
docker ps
docker inspect <container-name>
docker volume ls
```

Do not remove an existing container or volume until you know where `/var/jenkins_home` is stored and have a backup. Replacing the container can otherwise lose Jenkins configuration.

For a fresh test setup only, a named volume can persist Jenkins data:

```bash
docker volume create jenkins_home
docker run -d --name jenkins-python \\
  -p 8080:8080 -p 50000:50000 \\
  -v jenkins_home:/var/jenkins_home \\
  jenkins-python:lts
```

If port 8080 is already in use by your existing Jenkins, plan a migration first instead of starting a second container on that port.

## Temporary manual installation
If you choose to install packages in your current container:

```bash
docker ps
docker exec -it -u root <container-name> bash
```

Inside the container:

```bash
apt-get update
apt-get install -y python3 python3-pip python3-venv git
python3 --version
python3 -m venv --help
exit
```

These changes may be lost if the container is replaced or recreated.
