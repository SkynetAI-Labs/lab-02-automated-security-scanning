
![Lab 02 – Automated Security Scanning](skynet-lab-02.png)

# GitHub DevSecOps Lab 02: Automated Security Scanning with GitHub Actions

**Organisation:** SkynetAI-Labs  
**Project:** DevSecOps Lab 02  
**Environment:** Ubuntu Linux, Python 3.14, Docker, GitHub Actions  
**Initial Assessment Date:** 16 October 2026  
**Remediation Verification:** 23 October 2026  
**Status:** Implemented and verified

## Project Overview

This project demonstrates automated security testing and vulnerability remediation within a GitHub Actions CI/CD pipeline.

The DevSecOps security lifecycle follows:

**Identify → Analyse → Remediate → Rescan → Verify → Document**

The laboratory integrates application testing, static analysis, secret detection, Python dependency auditing and Docker container vulnerability scanning.

## Security Tools

| Tool | Purpose |
|---|---|
| pytest | Automated Python application testing |
| Semgrep | Static application security testing (SAST) |
| Gitleaks | Secret detection |
| pip-audit | Python dependency vulnerability scanning |
| Trivy | Docker container vulnerability scanning |
| Docker | Container image building and runtime verification |
| GitHub Actions | Automated CI/CD security checks |

## CI/CD Security Pipeline

GitHub Actions runs automated checks on repository changes.

Workflow configuration files are stored in `.github/workflows/`.

| Workflow | Purpose |
|---|---|
| `security.yml` | Python application testing |
| `semgrep.yml` | Static application security scanning |
| `gitleaks.yml` | Secret detection |
| `dependency-scan.yml` | Python dependency vulnerability auditing |
| `trivy.yml` | Docker image vulnerability scanning |

The pipeline demonstrates:

- Automated application testing.
- Source-code security scanning.
- Secret detection.
- Dependency vulnerability auditing.
- Container image scanning.
- Security failure enforcement.
- Automated verification following remediation.

## Application and Docker

The repository includes a Python demonstration application and an automated pytest test.

The current Docker image uses Python 3.14 on Alpine Linux 3.23.

The Dockerfile:

- Applies available Alpine package updates during the image build.
- Removes pip from the runtime image.
- Creates a dedicated non-root application user with UID `10001`.
- Copies the application into the container.
- Executes the application as the non-root user.

The application was successfully tested after runtime pip removal.

The container workflow also verifies the expected application output and non-root execution.

## Initial Vulnerability Assessment — Historical Debian Image

The original Debian-based Docker image was assessed using Trivy.

### Initial findings

| Component | HIGH | CRITICAL |
|---|---:|---:|
| Debian 13.7 | 44 | 0 |
| Python libraries | 4 | 0 |
| **Total** | **48** | **0** |

The Python findings involved libraries bundled with pip:

- `msgpack` 1.1.2
- `urllib3` 2.7.0
- `setuptools` 70.3.0

These findings describe the original Debian-based image, not the current Alpine image.

## Security Remediation

The application was confirmed to operate without pip at runtime.

Pip and its bundled libraries were removed from the Docker image.

The image was rebuilt, tested and rescanned.

### Historical results after removing runtime pip

| Component | HIGH | CRITICAL |
|---|---:|---:|
| Debian 13.7 | 44 | 0 |
| Python libraries | 0 | 0 |
| **Total** | **44** | **0** |

**Result:** Four HIGH-severity Python vulnerability findings were eliminated.

## Historical Debian Findings

The remaining 44 HIGH findings affected packages in the previous Debian-based operating-system image.

A separate Trivy assessment using `--ignore-unfixed` reported no Debian findings with available fixes under that assessment.

These findings were not resolved by patching the affected Debian packages.

The Debian runtime was subsequently replaced with Python 3.14 on Alpine Linux 3.23.

**Important:** The 44 HIGH Debian findings are retained as historical assessment evidence. They are not outstanding findings in the current Alpine image.

## Current Trivy Security Policy

The GitHub Actions Trivy workflow:

- Builds the current Docker image.
- Verifies the application's expected output.
- Verifies execution as UID `10001`.
- Scans the image for HIGH and CRITICAL vulnerabilities.
- Includes unfixed vulnerabilities in its configured scan.
- Returns a failure status when matching vulnerabilities are detected.

The workflow retains the following security controls:

| Control | Configuration |
|---|---|
| Severity threshold | `HIGH,CRITICAL` |
| Failure enforcement | `--exit-code 1` |
| Include unfixed findings | `--ignore-unfixed=false` |
| Runtime user | UID `10001` |
| Container base | Python 3.14 / Alpine 3.23 |

Passing this configured scan does not establish that the image is vulnerability-free.

Lower-severity findings and risks outside the configured checks are not ruled out.

## GitHub Actions Verification

The remediation in [Pull Request #2](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/pull/2) replaced the previous Debian runtime with the Alpine-based image.

Both push and pull request verification runs passed all five checks, producing ten successful workflow checks in total.

| Automated Check | Result |
|---|---|
| Python application tests | Passed |
| Semgrep | Passed |
| Gitleaks | Passed |
| pip-audit | Passed |
| Trivy | Passed |

The container workflow also verified the expected greeting and execution as UID `10001`.

Local backups, caches and development files are excluded from the Docker build context.

The five GitHub Actions workflows were subsequently verified passing together on 24 October 2026 following workflow security-hardening changes.

## Workflow Security Hardening

The workflows include security controls such as:

- Restricted GitHub Actions token permissions.
- Checkout credentials not persisted in the working directory.
- Job execution timeouts.
- Full Git history checkout for Gitleaks.
- A commit-SHA-pinned checkout action in the Trivy workflow.
- HIGH/CRITICAL vulnerability failure enforcement.

Additional dependency and action version pinning remains a potential future improvement.

## Security Findings Report

See the detailed [Security Findings Report](docs/SECURITY_FINDINGS.md).

The report documents:

- Initial vulnerability findings.
- Vulnerable Python libraries.
- Runtime pip removal.
- Historical Debian vulnerability results.
- Alpine base-image replacement.
- Current container verification.
- CI/CD security enforcement.
- Security limitations and ongoing considerations.

## Skills Demonstrated

- Linux command-line administration.
- Git and GitHub.
- GitHub Actions CI/CD.
- Python application testing.
- Static application security testing.
- Secret detection.
- Python dependency vulnerability analysis.
- Docker image security.
- Vulnerability remediation.
- Security workflow hardening.
- Security reporting and documentation.

## Project Status

**Security automation implemented and verified.**

- Five automated GitHub Actions workflows operational.
- Four historical HIGH Python findings eliminated through runtime pip removal.
- Previous Debian-based runtime replaced with Alpine.
- Current Alpine image passed the configured Trivy HIGH/CRITICAL vulnerability gate.
- Application output and non-root container execution verified.
- Historical Debian findings documented separately from the current image.

The project demonstrates practical DevSecOps security automation and remediation. It does not claim comprehensive penetration testing or a vulnerability-free environment.

**SkynetAI-Labs | GitHub DevSecOps Portfolio**


## Repository audit — 24 October 2026

See the [dated security audit and prioritised recommendations](SECURITY_AUDIT.md) for the reviewed source commit, verification results, open findings and evidence limitations.

