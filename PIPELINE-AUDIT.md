# Pipeline Audit

## Overview

The original GitHub Actions workflow contains six jobs:

- lint
- unit-tests
- build
- integration-tests
- deploy-staging
- deploy-production

The original workflow does not correctly define dependencies between these jobs. As a result, GitHub Actions starts independent jobs simultaneously, even when one job requires the output or successful completion of another.

The pipeline was redesigned using `needs:`, GitHub Actions artifacts, conditional execution, and job timeouts.

---

## 1. Lint

### Purpose

The lint job checks the source code for formatting and code-quality issues before other validation and build jobs proceed.

### Problem

The original workflow did not define dependencies for downstream jobs. Although lint itself can start immediately, the other jobs could also start at the same time without waiting for lint to pass.

### Fix

Lint remains the first job in the pipeline. Unit tests and build now use:

```yaml
needs: lint