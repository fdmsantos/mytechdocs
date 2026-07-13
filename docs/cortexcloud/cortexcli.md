# Cortex CLI

The Cortex CLI is a unified command-line tool that brings security scanning for three areas into a single binary: Cloud Workload Protection (CWP), API Security (WAAS), and Code Security (AppSec). It lets security teams enforce organizational policies and proactively detect vulnerabilities, misconfigurations, and exposed secrets in source code, container images, and API specifications.

**Scope:** the CLI evaluates findings against the **Unified Application Security Policies** already defined in your tenant, returning structured results with policy correlation, severity levels, and remediation guidance. It does **not** create, edit, or delete policies — that is still done through the Cortex Cloud tenant or the public API.

## When to use it

- **Local code development (AppSec)** — developers detect hardcoded secrets, IaC misconfigurations, and vulnerable dependencies in the terminal, before committing code.
- **CI/CD automation** — embed security checks into build scripts (e.g. Jenkins, GitHub Actions) to enforce security gates automatically during the build.
- **Container workloads (CWP)** — scan containers directly in CI builds, detecting vulnerabilities and malware before images are pushed to production registries.
- **API testing** — assess application endpoints for high-risk vulnerabilities and specification leaks as a standard pre-deployment step.

In short, use the Cortex CLI when you want to embed security checks directly into the development and CI/CD flow, rather than relying only on cloud-side platform scans.
