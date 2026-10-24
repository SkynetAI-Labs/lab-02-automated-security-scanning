# Repository Security Audit — Lab 02

**Review date:** 24 October 2026  
**Reviewed source commit:** [`e2e9a767c9dc149faca8b9eed8f0aad6847acc17`](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/tree/e2e9a767c9dc149faca8b9eed8f0aad6847acc17)  
**Scope:** Public repository configuration, documentation, retained evidence and available workflow results. This is a repository audit, not a fresh live-environment assessment or penetration test.

## Outcome
All five reviewed main-branch workflows passed on **24 October 2026**, for commit `e2e9a767c9dc149faca8b9eed8f0aad6847acc17`. Historical Debian vulnerability totals are correctly separated from the replacement Alpine image.

## Evidence checked
| Check | Reviewed run | Result |
|---|---|---|
| pytest | [40048119703](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/actions/runs/40048119703) | One test passed |
| Semgrep | [40048119769](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/actions/runs/40048119769) | 151 rules on one file; zero findings |
| Gitleaks | [40048119770](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/actions/runs/40048119770) | No leaks reported |
| pip-audit | [40048119593](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/actions/runs/40048119593) | No known vulnerabilities reported |
| Trivy | [40048119685](https://github.com/SkynetAI-Labs/lab-02-automated-security-scanning/actions/runs/40048119685) | Configured HIGH/CRITICAL gate passed; output and UID 10001 checks passed |

The Dockerfile, application, test, five workflows and findings report were inspected. Trivy includes unfixed findings. A passing gate does not cover lower severities or establish comprehensive application security.

## Recommendations
1. **Priority 2 — Pin remaining direct actions to verified full commit SHAs.** Only checkout in the Trivy workflow is currently SHA-pinned.
2. **Priority 2 — Pin the base image by digest**, with a reviewed update/rescan process. The current tag can change and `apk upgrade` also changes build inputs.
3. **Priority 2 — Record and maintain scanner versions and Python dependency locks.** Semgrep and pip-audit installations currently float.
4. **Priority 2 — Add scheduled rescanning and retain machine-readable scan outputs**, so new vulnerability data is assessed without relying on code pushes.
5. **Priority 3 — Expand tests if the demonstration application grows.** One greeting test is appropriate evidence of the present small example, not extensive coverage.

These are maintenance recommendations; no scanner threshold was weakened and no vulnerability finding was suppressed.

## Banner and evidence integrity

The README links an existing `skynet-lab-02.png` banner and the asset is present. No replacement was needed in this portfolio-wide pass. Historical evidence was preserved. Recommendations above remain open unless explicitly described as completed.

## References

- [GitHub Actions security guidance](https://docs.github.com/en/actions/reference/security/secure-use)
- [AWS S3 transport-encryption guidance](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryptionInTransit.html)
- [Kubernetes NetworkPolicy semantics](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

