# Docker Homelab

My humble homelab powered by [Docker](https://www.docker.com/). This is the place where most of my [self-hosted](https://github.com/awesome-selfhosted/awesome-selfhosted) tools are running. As this was an experiment for learning Docker and containerization, they may not be the perfect solution.

## Introduction

As part of my journey learning Docker, I migrated many of my selfhosted applications deployed manually on VMs or LXCs to a fully containerized environment. The purpose of this repo is to [learn in public](https://notes.nicolevanderhoeven.com/Learning+in+public), share my configuration so others can benefit [as I have from others](#acknowledgements) and document my changes.

I treat this homelab as finished, except the [work in progress](#work-in-progress), as it's working for me right now the way it is. I might add some more applications in the future.

## Infrastructure

* A single Ubuntu 24.04 LTS VM running on Proxmox VE, backed up to Proxmox Backup Server every night
* A [TrueNAS Scale](https://www.truenas.com/truenas-community-edition/) server provides network attached storage via SMB mounts to support applications such as [Immich](https://immich.app/) and [Paperless-ngx](https://docs.paperless-ngx.com/)

## Limitations

Some limitations currently apply to the homelab, simply because I did not have the time or knowledge (yet) to implement it properly.

### Automation

Deploying new applications is done manually. This could also be scripted or done via other automated solutions, but based on my research, it was not worth the hassle as I deliberately kept things simple. This is where the [future plans](#future-plans:-kubernetes) come in as other tools are better suited for this requirement.

### Rootful vs Rootless Containers 

The containers are running as a non-root user (PUID:PGID 1000:1000) were possible. For some containers, this was not possible because of known limitations or errors I could not work around. An example is the Redis database for the authentik-stack. These containers run as root:root.

## Structure

All applications are deployed through [Docker Compose](https://docs.docker.com/compose/) following a fixed structure:

* `appdata` is the location where the persistent data like databases and application-specific files live - separated by application for easy backup
* `compose` is a directory for the application-specific compose-files
* `logs` is a directory for collecting log-files for some applications - rarely used
* `secrets` is a directory for the secrets like api-keys, passwords etc. - each secret in its own file only readable by the docker-user
* `compose.yaml` is the main compose-file, where all application-specific compose-files come together through includes. The location of secrets and some network-related configuration is also specified here.

### `.dist`- Files

These files contain potentially sensitive values and must be configured before the container(s) are deployed. Have a look at the comments.

## Work in Progress

* [ ] Adding a full documentation for deployment of the applications
* [ ] Adding a list of applications including links and my use-case to the readme
* [ ] Provide a more visual representation of the [structure](#structure)
* [ ] Change all services to fixed tags instead of 'latest'

## Future Plans: Kubernetes

Currently, this homelab lives on a single VM on Proxmox. In order to gain experience in scalability and automated deployments, I decided to learn kubernetes. I've already covered the fundamentals, but plan to expand this to a full homelab powered by Kubernetes as soon as I have more time available. Once ready, the journey will be documented in a separate repo as well. 

## Acknowledgements

This project is heavily inspired by several people and sources I came across when I stumbled around on the internet. All of these bits and pieces helped me create this in various ways - the most important one is:

* juftin's [repo](https://github.com/juftin/homelab/tree/main) and Anand's [blog](https://www.simplehomelab.com/) on the idea how to structure the homelab
