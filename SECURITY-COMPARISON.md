# Docker Container Security Hardening Lab

## Objective
Compare an insecure NGINX Docker image with a hardened version using Trivy security scanning and runtime validation.

## Baseline Findings
Trivy identified two Dockerfile misconfigurations:

- **DS-0002 — HIGH:** No non-root USER instruction.
- **DS-0026 — LOW:** No HEALTHCHECK instruction.

The baseline container was also verified to run as root (UID 0).

## Hardening Measures
- Updated Alpine packages during the image build.
- Configured NGINX to listen on port 8080.
- Configured the container to run as the non-root `nginx` user (UID 101).
- Added a Docker HEALTHCHECK.
- Configured NGINX temporary and PID paths under `/tmp` to support non-root execution.

## Validation Results
- Hardened Dockerfile Trivy configuration scan: 0 misconfigurations detected.
- Hardened image Trivy scan: 0 vulnerabilities, 0 misconfigurations, and 0 secrets detected in the saved report.
- HTTP request to the hardened application returned status 200.
- Runtime identity check confirmed UID 101 (`nginx`).

## Evidence
- `scans/baseline-dockerfile-trivy.json`
- `scans/hardened-dockerfile-trivy.json`
- `scans/baseline-trivy.json`
- `scans/hardened-trivy.json`

## Conclusion
The lab demonstrates a practical before-and-after Docker security assessment. The hardened configuration addresses the baseline's detected root-user and missing-health-check findings, while runtime testing confirms that the application remains functional.

Note: Trivy results reflect the scanner version, vulnerability database, and checks available at scan time. Zero findings do not guarantee the absence of all security issues.
