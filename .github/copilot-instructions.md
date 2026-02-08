# GitHub Copilot Instructions

## Project Overview
This repository follows **GitOps principles** for managing a Kubernetes-based homelab environment. All infrastructure and application configurations are version-controlled, and changes are deployed through Git workflows.

## GitOps Principles

### Core Tenets
- **Declarative Configuration**: All system state is described declaratively in Git
- **Version Control**: Git is the single source of truth for infrastructure and applications
- **Automated Deployment**: Changes are automatically synced to the cluster
- **Continuous Reconciliation**: The desired state in Git is continuously reconciled with the actual state

### Best Practices
1. **Keep configurations declarative** - Use YAML manifests for all Kubernetes resources
2. **Commit everything** - All changes go through Git, no manual `kubectl apply`
3. **Use meaningful commit messages** - Describe what changed and why
4. **Review before merge** - Use pull requests for all changes
5. **Test in branches** - Validate changes in feature branches before merging

## Kubernetes Homelab Standards

### Container Images
- **Use specific image tags**, avoid `latest` tag
- **Pin versions** for reproducibility and stability
- **Use semantic versioning** when possible (e.g., `nginx:1.29-alpine`)
- **Prefer official images** from trusted registries
- **Use minimal base images** (e.g., Alpine, distroless) for security and size

### Manifest Organization
- Keep Kubernetes manifests organized by application/service
- Use meaningful file names (e.g., `deployment.yaml`, `service.yaml`)
- Group related resources in the same directory
- Document resource requirements and dependencies

### Resource Management
- **Always define resource requests and limits** for CPU and memory
- Use appropriate resource units (e.g., `100m` for CPU, `256Mi` for memory)
- Set sensible defaults that won't overcommit the cluster
- Monitor and adjust based on actual usage

### Security Practices
- **Run containers as non-root** whenever possible
- Use `readOnlyRootFilesystem: true` where applicable
- Apply `securityContext` settings appropriately
- Keep secrets out of manifests - use Kubernetes Secrets or external secret management
- Regularly update base images to patch vulnerabilities

## Renovate Configuration

This project uses **Renovate** for automated dependency updates:

### Strategy
- Dependencies are automatically detected and updated via pull requests
- **Automerge is disabled** to ensure manual review of all updates
- Updates respect `respectLatest: true` for proper version ordering
- Major updates require manual approval for safety

### Supported Ecosystems
- **Docker images** in Dockerfiles and Kubernetes manifests
- **GitHub Actions** workflow dependencies
- **Kubernetes manifests** for application versions
- **ArgoCD/Flux** application definitions
- **docker-compose** files

### Custom Patterns
- Custom regex managers can detect non-standard version patterns
- Image tags in YAML files are tracked via regex matching
- File patterns target specific directories (e.g., `kubernetes/apps/`)

## CI/CD Workflows

### GitHub Actions Standards
- **Verify builds** on every pull request
- Use **matrix strategies** for testing multiple configurations
- **Cache aggressively** (Docker layers, build artifacts) to speed up workflows
  - Use cache invalidation when base images or dependencies are updated for security
  - Consider cache freshness requirements vs. build speed trade-offs
- **Minimize permissions** - use `permissions: contents: read` by default
- Pin action versions to specific commits or tags for security

### Docker Build Practices
- Use **multi-stage builds** to minimize final image size
- Leverage **BuildKit** for better caching and performance
- **Scan images** for vulnerabilities before deployment
- Tag images with commit SHAs for traceability
- Use `docker/build-push-action` for optimized CI builds

## Infrastructure as Code

### General Principles
- **Everything as code** - no manual infrastructure changes
- **Idempotent operations** - running the same code multiple times produces the same result
- **Documentation in code** - use comments and README files
- **Secrets management** - never commit secrets; use secure vaults or encrypted stores
- **Modular design** - break configurations into reusable components

### File Organization
```
.github/
  workflows/          # CI/CD pipelines
kubernetes/
  apps/               # Application manifests
  infrastructure/     # Cluster infrastructure
Dockerfiles/          # Container definitions
```

## Code Style and Quality

### YAML Files
- Use **2 spaces** for indentation
- Keep lines under 100 characters when possible
- Use consistent key ordering (kind, metadata, spec, etc.)
- Add comments for complex or non-obvious configurations

### Dockerfiles
- Order instructions from least to most frequently changing
- Combine `RUN` commands to reduce layers
- Clean up package manager caches
- Use `.dockerignore` to exclude unnecessary files

### Documentation
- Keep README files up-to-date with current setup
- Document prerequisites and dependencies
- Include examples for common operations
- Explain non-obvious architectural decisions

## Deployment Workflow

1. **Make changes** in a feature branch
2. **Open pull request** with clear description
3. **Automated checks** run (Docker builds, linting, tests)
4. **Review** changes for correctness and security
5. **Merge** to main branch after approval
6. **Automatic deployment** via GitOps controller (ArgoCD/Flux)
7. **Monitor** for successful reconciliation

## Homelab-Specific Considerations

- **Resource constraints** - Be mindful of limited CPU/memory in homelab environments
- **Persistent storage** - Plan for stateful applications and backup strategies
- **High availability** - Balance HA requirements with resource availability
- **Networking** - Consider ingress, load balancing, and service exposure carefully
- **Monitoring** - Implement observability early (metrics, logs, traces)
- **Backup and recovery** - Regular backups of critical data and configurations

## When Writing Code

- **Follow GitOps** - Make changes through Git, not direct cluster manipulation
- **Test locally** - Build and test Docker images before pushing
- **Update dependencies** - Review and merge Renovate PRs regularly
- **Security first** - Scan for vulnerabilities and follow security best practices
- **Document changes** - Update relevant README files and comments
- **Keep it simple** - Prefer simplicity over complexity in homelab setups
