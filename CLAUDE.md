# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **Backstage-inspired Platform Engineering Template** repository designed for building developer portals and internal platforms. It includes comprehensive tooling for cloud-native development, secure CI/CD pipelines, and extensive Claude Code agents for accelerated development.

## Key Architecture Components

### Platform Stack

- **Platform API**: Node.js/TypeScript backend for namespace-as-a-service and resource management
- **Platform UI**: React-based developer portal (Backstage-inspired) for self-service capabilities
- **External Secrets**: Azure Key Vault integration for secure secrets management
- **Kubernetes**: Target deployment platform (AKS) with GitOps workflows

### Infrastructure & Tooling

- **Devbox + Nix**: Reproducible development environment with pinned tool versions
- **Teller**: Secrets management tool (included in Nix environment)
- **Google Cloud SDK**: Cloud integration tooling
- **Multi-cloud tooling**: Azure CLI, AWS CLI, kubectl, Helm, Terraform

### Security-First Approach

- **No embedded secrets**: All secrets managed via External Secrets + Azure Key Vault
- **Container scanning**: Trivy security scanning in CI/CD
- **Secret leak detection**: detect-secrets scanning
- **Secure build pipelines**: Multi-stage security validation

## Development Environment Setup

### Prerequisites

- Devbox installed for Nix-based development environment
- Docker for container builds
- Access to Azure subscription for cloud resources

### Quick Start

```bash
# Enter development environment (loads all tools via Nix)
devbox shell

# Verify tooling
az version
kubectl version --client
gcloud version
teller version

# Use Teller for secrets management
teller --help
```

### Available Tools (via Devbox/Nix)

- **Cloud**: Azure CLI, AWS CLI, Google Cloud SDK, eksctl
- **Kubernetes**: kubectl, Helm, kind
- **Database**: PostgreSQL 17.5
- **Git**: Git 2.49.0
- **Secrets**: Teller 2.0.7
- **Shell**: Nushell (modern shell)

## Development Workflows

### Secrets Management with Teller

All secrets should be managed via Teller configuration:

- Never commit secrets to the repository
- Use External Secrets for Kubernetes secret injection
- Configure Teller for local development secret access
- Reference secrets from Azure Key Vault

### CI/CD Pipeline

The repository includes two main workflows:

1. **build-platform-api.yml**: Simple build and push for Platform API
2. **secure-build-deploy.yml**: Comprehensive security-first pipeline with:
   - Secret scanning
   - Vulnerability scanning with Trivy
   - Kubernetes manifest validation
   - External Secrets validation
   - Multi-stage deployments (dev → prod)

### Platform Development

- **platform-api/**: Backend service directory
- **platform-ui/**: Frontend application directory
- **k8s/**: Kubernetes manifests for deployment
- **scripts/**: Deployment and utility scripts

## Claude Code Agents

This repository includes 100+ specialized Claude Code agents organized by expertise:

### Core Development Agents

- **backend-architect**: API design and server systems
- **frontend-developer**: React/UI development
- **platform-engineer**: Kubernetes and infrastructure
- **devops-engineer**: CI/CD and automation

### Platform-Specific Agents

- **platform-api-specialist**: Node.js/TypeScript platform backend
- **platform-ui-specialist**: React developer portal
- **external-secrets-specialist**: Azure Key Vault integration
- **cert-manager-specialist**: TLS certificate management
- **istio-deployment-agent**: Service mesh configuration

### Security & Quality Agents

- **security-auditor**: Security compliance validation
- **penetration-tester**: Security testing
- **compliance-auditor**: Regulatory compliance (GDPR, SOC 2)

### Agent Categories

Located in `.claude/agents/categories/`:

- `01-core-development/`: Core engineering roles
- `02-language-specialists/`: Language-specific experts
- `03-infrastructure/`: DevOps and platform engineering
- `04-quality-security/`: Testing and security
- `05-data-ai/`: Data science and AI/ML
- `06-developer-experience/`: DX and tooling

## Build and Deployment Commands

### Local Development

```bash
# Enter development environment
devbox shell

# Build platform API
cd platform-api && npm run build

# Build platform UI
cd platform-ui && npm run build

# Run tests
npm run test
```

### Container Builds

```bash
# Build platform API image
docker build -t platform-api:latest ./platform-api

# Build platform UI image
docker build -t platform-ui:latest ./platform-ui
```

### Kubernetes Deployment

```bash
# Deploy to development
kubectl apply -k k8s/overlays/development

# Deploy to production
kubectl apply -k k8s/overlays/production
```

## Security Best Practices

1. **Never commit secrets**: Use Teller and External Secrets
2. **Scan containers**: Use provided Trivy scanning
3. **Validate manifests**: Use kubeval for Kubernetes resources
4. **Security contexts**: Run containers as non-root
5. **Network policies**: Implement proper network segmentation

## Environment-Specific Configuration

### Development Tools Integration

- **VS Code**: Settings and devcontainer configuration in `.vscode/`
- **Cursor**: MCP configuration in `.cursor/`
- **Dev Containers**: Full containerized development environment

### IDE Configuration

- Devcontainer with Dockerfile and initialization scripts
- VS Code workspace configuration
- Cursor MCP integration for enhanced AI assistance

## Common Tasks

### Adding New Platform Features

1. Use `platform-api-specialist` agent for backend changes
2. Use `platform-ui-specialist` agent for frontend changes
3. Use `external-secrets-specialist` for secure secret handling
4. Use `test-writer-fixer` agent for comprehensive testing

### Infrastructure Changes

1. Use `platform-engineer` for Kubernetes resources
2. Use `external-dns-specialist` for DNS configuration
3. Use `cert-manager-specialist` for TLS certificates
4. Use `security-auditor` for security validation

### CI/CD Updates

1. Use `devops-engineer` for pipeline modifications
2. Use `security-engineer` for security scanning updates
3. Use `test-automator` for test automation improvements

## Repository Structure Notes

### Key Directories

- `.claude/`: Extensive collection of specialized agents
- `.devbox/`: Nix-based reproducible development environment
- `.github/workflows/`: Security-first CI/CD pipelines
- `logs/`: Claude Code session logging (development artifacts)

### Configuration Files

- **Devbox**: `.devbox/gen/flake/flake.nix` (pinned tool versions)
- **GitHub Actions**: Secure build and deploy workflows
- **Development**: Multiple IDE configurations for team flexibility

This repository represents a production-ready template for building secure, scalable developer platforms with comprehensive tooling and AI-assisted development workflows.
