16. Deploying an app
====================

1. Dockerfiles
--------------

Dockerfiles contain instructions for automatically building Docker images. The following instructions are supported:

- ``ADD``: Add local or remote files and directories.
- ``ARG``: Use build-time variables.
- ``CMD``: Specify default commands.
- ``COPY``: Copy files and directories.
- ``ENTRYPOINT``: Specify default executable.
- ``ENV``: Set environment variables.
- ``EXPOSE``: Describe which ports your application is listening on.
- ``FROM``: Create a new build stage from a base image.
- ``HEALTHCHECK``: Check a container's health on startup.
- ``LABEL``: Add metadata to an image.
- ``MAINTAINER``: Specify the author of an image.
- ``ONBUILD``: Specify instructions for when the image is used in a build.
- ``RUN``: Execute build commands.
- ``SHELL``: Set the default shell of an image.
- ``STOPSIGNAL``: Specify the system call signal for exiting a container.
- ``USER``: Set user and group ID.
- ``VOLUME``: Create volume mounts.
- ``WORKDIR``: Change working directory.

Dockerfiles typically use the following format and conventions:

```
# Comment
INSTRUCTION arguments
```

Docker runs instructions in the order they appear in the Dockerfile. The first isntruction must be a ``FROM`` instruction which specifies the
base image to use. Here is a basic example of a Dockerfile for a Python application:

```
FROM python:3.13
WORKDIR /usr/local/app

# Install the application dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy in the source code
COPY src ./src
EXPOSE 8080

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```