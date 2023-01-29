# Docker Compose Remote Action

This action packs contents of the action workspace into archive.
Logs into remote host via ssh. Unpacks the workspace there and runs
`docker compose up -d` command.

## Inputs

* `ssh_user` - Remote user which should have access to docker.
* `ssh_host` - Remote host name.
* `ssh_port` - Remote port for SSH connection. Default is 22.
* `ssh_jump_host` - Jump host name.
* `ssh_private_key` - Private SSH key used for logging into remote system. Please, keep your key securely in GitHub secrets.
* `ssh_host_public_key` - Remote host SSH public key (The content of `~/.ssh/known_hosts` needs to be given here).
* `ssh_jump_public_key` - Jump host SSH public key (The content of `~/.ssh/known_hosts` needs to be given here).
* `docker_compose_prefix` - Project name passed to compose. Each docker container will have this prefix in name.
* `docker_compose_filename` - Path to the docker-compose file in the repository.
* `docker_use_stack` - Use docker stack instead of docker-compose.
* `docker_env` - Docker Environment variables.
* `workspace` - A project directory to use.
* `workspace_keep` - Whether to keep the workspace directory after the action has finished.

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
        ssh_private_key: ${{ secrets.EXAMPLE_COM_SSH_PRIVATE_KEY }}
        ssh_user: ${{ secrets.EXAMPLE_COM_SSH_USER }}
        docker_compose_prefix: example_com
```
