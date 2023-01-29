# Docker Compose Remote Action

This action packs contents of the action workspace into archive.
Logs into remote host via ssh. Unpacks the workspace there and runs `docker compose up -d` command.

## Inputs

### Required
* `ssh_user` - Remote user which should have access to docker.
* `ssh_host` - Remote host name.
* `ssh_private_key` - Private SSH key used for logging into remote system. Please, keep your key securely in GitHub secrets.
* `ssh_host_public_key` - Remote host SSH public key (The content of `~/.ssh/known_hosts` needs to be given here).

### Optional
* `ssh_port` - Remote port for SSH connection. Default is `22`.
* `ssh_jump_host` - Jump host name. If set, `ssh_jump_public_key` is required.
* `ssh_jump_public_key` - Jump host SSH public key (The content of `~/.ssh/known_hosts` needs to be given here).
* `docker_compose_prefix` - Project name passed to compose. Each docker container will have this prefix in name. Required if `docker_use_stack` is `true`.
* `docker_compose_filename` - Path to the docker-compose file in the repository. Default is `docker-compose.yml`.
* `docker_args` - Docker compose arguments. Default is `-d --remove-orphans --build`.
* `docker_use_stack` - Use docker stack instead of docker-compose. Default is `false`.
* `docker_env` - Docker Environment variables.
* `workspace` - A project directory to use. Default is `~/workspace`.
* `workspace_keep` - Whether to keep the workspace directory after the action has finished. Default is `false`.
* `container_registry` - Container registry server to use.
* `container_registry_username` - Container registry username.
* `container_registry_password` - Container registry password.

# Usage example

Let's say we have a repo with single docker-compose file in it and remote
ubuntu based server with docker and docker-compose installed.

Setup a github-actions workflow (e.g. `.github/workflows/main.yml`):

```
name: Deploy

on:
  push:
    branches: [ master ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: astappiev/docker-compose-remote-action@master
      name: Docker-Compose Remote Deployment
      with:
        ssh_host: example.com
        ssh_user: ${{ secrets.DEPLOY_USERNAME }}
        ssh_private_key: ${{ secrets.DEPLOY_PRIVATE_KEY }}
        ssh_host_public_key: ${{ secrets.DEPLOY_PUBLIC_KEY }}
        docker_compose_prefix: myapp
```
