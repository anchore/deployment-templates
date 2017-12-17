# Deploying Anchore Engine

This repository provides deployment examples for Anchore Engine on a variety of platforms and configurations and is intended as a starting point for developing production configurations of Anchore Engine. Importantly: these are not expected to be full production configurations and will often omit things that are site-specific such as certificate configuration or DNS entries depending on the platform. The purpose of this repository is to show a few deployment architectures and provide solid baseline examples for deploying anchore engine that can be customized and tailored for individual production environments and requirements.

## Repository Structure

Each deployment platform has its own directory containing a README. It may contain multiple configurations or ways to deploy on that platform. Each config on each platform is not re-tested after every commit or update to Anchore Engine so there may be delays in getting things updated as the service code changes.

# Reporting Issues

We will do our best to update and test each configuration but inevitably problems will occur. Please include the following in any issue you file so that we can quickly triage and try to reproduce the problem:

1. Platform deployed to, including version information of client and service (e.g. kubectl version)
2. Version of anchore-engine deployed anchore-cli version
3. Steps to reproduce the problem
4. Logs from anchore engine as well as the platform itself if available

# Contributing

For guidelines and requirements for contributing to this repository see [CONTRIBUTING](CONTRIBUTING.rst)
