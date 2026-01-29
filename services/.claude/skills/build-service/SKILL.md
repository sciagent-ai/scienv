---
name: build-service
description: Build and publish a new scientific computing service to GHCR. Use when the user wants to add a new package/service like ngspice, fenics, etc.
---

# Build Service Workflow

When the user asks to build a new service package, follow these steps in order:

## 1. Check Prerequisites

- Read `registry.yaml` to see if the service is already defined
- Check if `services/{package}/Dockerfile` already exists
- Look at existing Dockerfiles (e.g., `services/meep/Dockerfile`) for patterns

## 2. Create Package Files

### Dockerfile
Create `services/{package}/Dockerfile` following this pattern:
- Use appropriate base image (conda for complex deps, python:slim for simple)
- Add LABEL for org.opencontainers.image.source, description, licenses
- Set PYTHONUNBUFFERED=1
- Install package and dependencies
- Set WORKDIR /workspace
- Add verification step (RUN command that tests import/execution)
- Set appropriate CMD

### .dockerignore
Create `services/{package}/.dockerignore`:
```
__pycache__
*.pyc
*.pyo
.git
.gitignore
*.md
.DS_Store
```

## 3. Update Registry (if needed)

If the package is not in `registry.yaml`, add an entry following the existing format:
```yaml
  package-name:
    description: "Brief description"
    image: ghcr.io/sciagent-ai/package-name
    dockerfile: services/package-name/Dockerfile
    runtime: python3  # or bash
    workdir: /workspace
    capabilities:
      - "Capability 1"
      - "Capability 2"
    example: |
      # Example usage code
```

## 4. Build, Push, and Verify

Execute these steps in order, using the TodoWrite tool to track progress:

1. **Build locally**:
   ```bash
   cd services/{package} && docker build -t ghcr.io/sciagent-ai/{package}:latest .
   ```

2. **Push to GHCR**:
   ```bash
   docker push ghcr.io/sciagent-ai/{package}:latest
   ```

3. **Remove local image(s)**:
   ```bash
   docker rmi ghcr.io/sciagent-ai/{package}:latest
   ```

4. **Pull from GHCR to verify**:
   ```bash
   docker pull ghcr.io/sciagent-ai/{package}:latest
   ```

5. **Test the image**:
   ```bash
   docker run --rm ghcr.io/sciagent-ai/{package}:latest <verification-command>
   ```

6. **Cleanup** (optional):
   ```bash
   docker rmi ghcr.io/sciagent-ai/{package}:latest
   ```

## 5. Report Results

Summarize:
- Image location: `ghcr.io/sciagent-ai/{package}:latest`
- Files created
- Verification status
- Any issues encountered
