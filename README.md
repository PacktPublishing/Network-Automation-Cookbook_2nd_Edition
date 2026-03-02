<p align='center'><a href='https://www.eventbrite.com/e/ship-production-pytorch-system-in-a-day-train-optimize-deploy-workshop-tickets-1983348934052?aff=GitHub'><img src='https://static.packt-cdn.com/assets/images/packt+events/workshop_ashish_github.png'/></a></p>

<!-- [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/network-cookbook-lab/network-cookbook-lab?quickstart=1&devcontainer_path=.devcontainer%2Fbatteries-included%2Fdevcontainer.json) -->

# Network Automation Cookbook Lab

This repository contains the resources for building and managing an network automation stack within a network lab environment, specifically designed for the "Network Automation Cookbook 2nd edition" book.

## Requirements

The lab environments are designed to set up a small network and an attached automation stack. Developed and tested on Debian-based systems, we provide **[setup](setup/README.md) documentation to guide** you through automatic setup on a DigitalOcean droplet. This process will provision, install dependencies, and configure the environment automatically. But if you want to host the lab environment, ensure the following:

- `docker` installed (version `26.1.1` or above)
- `containerlab` for the network lab (version `0.54.2` or above)
- `lab` for managing the network and lab stack
- Arista `cEOS` images for the `containerlab` environment. You can open an account and download them at [arista.com](https://www.arista.com)

For detailed setup instructions, please refer to the [setup README](./setup/README.md). It provides a comprehensive guide on installing the lab environment on a DigitalOcean Droplet.

### Prepare cEOS image

After downloading the image, use the following command to import them as Docker images:

```bash
# Import cEOS images as Docker images
docker import <path-to-image>/cEOS64-lab-<version>.tar.xz ceos:image
```

## Quickstart

To get started with the network lab stack, you need to:

1. Copy the necessary environment variables to configure the components used within the lab scenarios.

```bash
# Setup environment variables (edit the .env file to your liking)
cp example.env .env
```

2. Install the `lab` utility command that helps manage the entire lab environment. Example using `uv`:

```bash
# Install the python dependencies
uv pip install .
```

3. Test everything is working by deploying a lab that has most of the components configured and ready to go.

```bash
# Start the network lab
lab lab deploy --scenario batteries-included
```

> NOTE: Our lab comes with a `batteries-included` setup, providing you with everything you need to get started with network lab right away. This setup includes pre-configured tools and detailed step-by-step instructions to help you explore and learn without any hassle. Head over to the [instructions](./chapters/batteries-included/README.md) section to begin!

---

## Managing Lab Environment with `lab`

The `lab` utility tool simplifies managing and monitoring the network lab and other stack set up within this repository. It provides a suite of commands designed to streamline various tasks associated with your network infrastructure.

### Top-Level Commands

The `lab` utility includes five main commands to help manage the environment:

- **`lab setup`**: Manages the overall setup of a remote DigitalOcean droplet hosting this repository and its lab environment. This command simplifies the process of preparing a hosting environment for users.

- **`lab containerlab`**: Manages the `containerlab` pre-configured setup. All lab scenarios presented in the chapters operate under this network lab configuration.

- **`lab docker`**: Manages the Docker Compose setups for each lab scenario. It ensures the appropriate containers are running for each specific lab exercise.

### Example Usage

For instance, the `lab lab deploy` command builds and starts a `containerlab` environment along with the automation stack. This command sets up the entire lab scenario, ensuring that all necessary components are up and running.

```bash
# Start the network lab
❯ lab lab deploy batteries-included --sudo
[21:50:42] Deploying lab environment
           Network create: network-cookbook-lab
           Running command: docker network create --driver=bridge  --subnet=198.51.100.0/24 network-cookbook-lab
           Successfully ran: network create
─────────────────────────────────────────────────── End of task: network create ────────────────────────────────────────────────────

           Deploying containerlab topology
           Topology file: containerlab/lab.yml
           Running command: sudo containerlab deploy -t containerlab/lab.yml
INFO[0000] Creating container: "ceos-01"
INFO[0000] Creating container: "ceos-02"
INFO[0001] Creating virtual wire: ceos-01:eth2 <--> ceos-02:eth2
INFO[0001] Creating virtual wire: ceos-01:eth1 <--> ceos-02:eth1
+---+---------+--------------+----------------+------+---------+------------------+--------------+
| # |  Name   | Container ID |     Image      | Kind |  State  |   IPv4 Address   | IPv6 Address |
+---+---------+--------------+----------------+------+---------+------------------+--------------+
| 1 | ceos-01 | d59629fbbdc0 | ceos:4.28.5.1M | ceos | running | 198.51.100.11/24 | N/A          |
| 2 | ceos-02 | 80854bfd7e08 | ceos:4.28.5.1M | ceos | running | 198.51.100.12/24 | N/A          |
+---+---------+--------------+----------------+------+---------+------------------+--------------+
[21:51:14] Successfully ran: Deploying containerlab topology
─────────────────────────────────────────── End of task: Deploying containerlab topology ───────────────────────────────────────────

           Running command: docker compose --project-name lab -f chapters/docker-compose.yml --verbose up -d --remove-orphans
[+] Building 0.0s (0/0)
[+] Running 10/10
... too be updated ..
[21:51:16] Successfully ran: start stack
───────────────────────────────────────────────────── End of task: start stack ─────────────────────────────────────────────────────
```

---

## Overview of Practical Chapters

Here is a brief overview of the practical chapters and the key concepts you will encounter:

- **[`Batteries Included`](./chapters/batteries-included/) Scenario:** This scenario brings everything together in a fully configured environment, offering a glimpse into the full potential of these tools. The batteries-included scenario [README](./chapters/batteries-included/README.md) provides an overview and detailed explanation of the setup, giving you a holistic view of what is achievable with this setup.
