---
name: ci-cd-pipeline
description: >
  Build and configure CI/CD pipelines with GitHub Actions, GitLab CI, or Jenkins.
  This skill creates automated build, test, and deployment workflows.
version: "1.0.0"
tags: [ci-cd, github-actions, jenkins, gitlab-ci, automation, devops, pipelines, openclaw]
metadata:
  author: "@smouj"
  category: devops
  expertise: expert
  repo: https://github.com/smouj/ci-cd-pipeline-skill
  license: MIT
triggers:
  - ci-cd
  - pipeline
  - github actions
  - jenkins
  - gitlab ci
  - automated build
  - deployment pipeline
  - setup ci
---

# CI/CD Pipeline Builder

You are an expert in continuous integration and deployment automation.

## When to Use This Skill

- **Use when:** Setting up CI/CD pipelines
- **Use when:** Configuring automated builds and tests
- **Use when:** Creating deployment workflows
- **Use when:** Setting up GitOps workflows
- **Use when:** Configuring build caching
- **NOT for:** Manual deployments (use cloud-deploy)

## Work Process

### 1. Requirements
- Identify languages, frameworks, and tools
- Define build, test, and deploy steps
- Identify deployment targets
- Set up environment variables

### 2. Design
- Choose CI/CD platform (GitHub Actions, GitLab CI, Jenkins)
- Plan pipeline stages (build, test, deploy)
- Design parallelization strategy
- Define cache and artifact strategies

### 3. Implementation
- Create pipeline configuration files
- Set up matrix builds for multiple environments
- Configure secrets and environment variables
- Add quality gates (lint, test, security)

### 4. Validation
- Test pipeline on feature branch
- Verify all stages pass
- Check artifact generation
- Validate deployment to staging

## Golden Rules

1. **Fast builds** - Parallelize, cache dependencies, use lightweight images
2. **Fail fast** - Run fastest tests first
3. **Artifacts** - Preserve build outputs for debugging
4. **Secrets** - NEVER hardcode credentials in config
5. **Idempotent** - Pipeline must be re-runnable

## Supported Platforms

| Platform | Config File | Best For |
|----------|-------------|----------|
| GitHub Actions | .github/workflows/*.yml | Open source, GitHub-hosted |
| GitLab CI | .gitlab-ci.yml | GitLab repositories |
| Jenkins | Jenkinsfile | Custom infrastructure |
| CircleCI | .circleci/config.yml | Complex workflows |

## Output Format

```markdown
## CI/CD Pipeline Configuration

### GitHub Actions Workflow
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'
  REGISTRY: ghcr.io
  IMAGE_NAME: \${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: \${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linter
        run: npm run lint
      
      - name: Run tests
        run: npm test -- --coverage
      
      - name: Build
        run: npm run build

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Build and push Docker image
        run: |
          docker build -t \${{ env.IMAGE_NAME }}:\${{ github.sha }} .
          docker push \${{ env.IMAGE_NAME }}:\${{ github.sha }}
```

## Pipeline Stages
1. ✅ Checkout code
2. ✅ Setup environment
3. ✅ Install dependencies (cached: 45s)
4. ✅ Lint code
5. ✅ Run tests (78 tests, 82% coverage)
6. ✅ Build application
7. ✅ Build Docker image
8. ✅ Deploy to staging (on develop branch)
9. ✅ Deploy to production (on main branch)

## Environment Variables
| Variable | Value | Description |
|----------|-------|-------------|
| NODE_ENV | production | Node environment |
| API_URL | https://api.example.com | API endpoint |
| LOG_LEVEL | warn | Logging level |

## Secrets Required
- [ ] DOCKER_REGISTRY_TOKEN
- [ ] DEPLOY_SSH_KEY
- [ ] AWS_ACCESS_KEY_ID
- [ ] AWS_SECRET_ACCESS_KEY
```
