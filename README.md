# Backstage Platform Engineering Template

A comprehensive, production-ready template for building **Backstage-inspired developer platforms** with enterprise-grade security, multi-cloud support, and AI-assisted development workflows.

## 🚀 Features

### 🏗️ Platform Stack

- **Platform API**: Node.js/TypeScript backend for namespace-as-a-service
- **Platform UI**: React-based developer portal (Backstage-inspired)
- **External Secrets**: Azure Key Vault integration for secure secrets management
- **Kubernetes**: AKS deployment with GitOps workflows
- **Service Mesh**: Istio integration for advanced traffic management

### 🔧 Development Environment

- **Devbox + Nix**: Reproducible environment with pinned tool versions
- **Multi-IDE Support**: VS Code, Cursor, and Dev Container configurations
- **Multi-Cloud**: Azure CLI, AWS CLI, Google Cloud SDK
- **Kubernetes Tools**: kubectl, Helm, kind, Istio

### 🛡️ Security-First Architecture

- **Zero Secrets in Code**: Teller + External Secrets + Azure Key Vault
- **Container Scanning**: Trivy security scanning in CI/CD
- **Secret Leak Detection**: detect-secrets scanning
- **Secure Pipelines**: Multi-stage security validation
- **Compliance Ready**: GDPR, SOC 2, PCI DSS support

### 🤖 AI-Powered Development

- **100+ Claude Code Agents**: Specialized experts for every domain
- **Platform-Specific Agents**: Custom agents for your platform stack
- **Security Agents**: Automated security reviews and compliance checks
- **Quality Agents**: Testing, performance, and code quality automation

## 📋 Prerequisites

- **Devbox**: For reproducible development environment
- **Docker**: For container builds and local development
- **Azure Subscription**: For cloud resources and Key Vault
- **Claude Code**: For AI-assisted development (optional but recommended)

## 🚀 Quick Start

### 1. Setup Development Environment

```bash
# Clone the repository
git clone <your-repo-url>
cd backstage-template

# Enter the development environment (loads all tools via Nix)
devbox shell

# Verify tooling is available
az version
kubectl version --client
gcloud version
teller version
```

### 2. Configure Secrets Management

```bash
# Configure Teller for local development
# (Add your configuration to .teller.yml)

# Initialize External Secrets in your cluster
kubectl apply -f k8s/external-secrets/

# Verify External Secrets Operator is running
kubectl get pods -n external-secrets-system
```

### 3. Build and Deploy

```bash
# Build platform services
docker build -t platform-api:latest ./platform-api
docker build -t platform-ui:latest ./platform-ui

# Deploy to development
kubectl apply -k k8s/overlays/development

# Check deployment status
kubectl get pods -n platform-system
```

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Platform UI   │    │  Platform API   │    │ External Secrets│
│   (React SPA)   │◄──►│ (Node.js/TS)    │◄──►│ (Azure KV)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │              ┌─────────────────┐              │
         └─────────────►│   Kubernetes    │◄─────────────┘
                        │   (AKS Cluster) │
                        └─────────────────┘
                                │
                       ┌─────────────────┐
                       │     Istio       │
                       │ (Service Mesh)  │
                       └─────────────────┘
```

## 🛠️ Development Workflows

### Platform Development

```bash
# Start local development
devbox shell

# Platform API development
cd platform-api
npm install
npm run dev

# Platform UI development
cd platform-ui
npm install
npm start
```

### Using Claude Code Agents

The repository includes specialized agents for common tasks:

```bash
# Backend development
"Use platform-api-specialist to add namespace management"

# Frontend development
"Use platform-ui-specialist to create service catalog UI"

# Security reviews
"Use security-auditor to review the authentication flow"

# Infrastructure changes
"Use platform-engineer to configure Istio traffic policies"
```

### CI/CD Pipeline

The repository includes two main workflows:

1. **`build-platform-api.yml`**: Simple build and push
2. **`secure-build-deploy.yml`**: Comprehensive security pipeline

Features:

- Secret scanning with detect-secrets
- Vulnerability scanning with Trivy
- Kubernetes manifest validation
- External Secrets validation
- Multi-stage deployments (dev → prod)

## 🔐 Security Best Practices

### Secrets Management

- **Never commit secrets** to the repository
- Use **Teller** for local development secrets
- Use **External Secrets** for Kubernetes secret injection
- Store all secrets in **Azure Key Vault**

### Container Security

- All images built with **security-first** approach
- **Trivy scanning** in CI/CD pipeline
- **Non-root** container execution
- **Security context** hardening

### Network Security

- **Istio service mesh** for traffic management
- **Network policies** for pod-to-pod communication
- **mTLS** encryption between services

## 📁 Repository Structure

```
backstage-template/
├── .claude/                    # 100+ specialized Claude Code agents
│   ├── agents/
│   └── HOOKS_SETUP.md
├── .devbox/                    # Nix-based development environment
│   └── gen/flake/flake.nix     # Pinned tool versions
├── .github/workflows/          # Security-first CI/CD pipelines
│   ├── build-platform-api.yml
│   └── secure-build-deploy.yml
├── .devcontainer/              # Dev Container configuration
├── .vscode/                    # VS Code settings
├── .cursor/                    # Cursor IDE configuration
├── platform-api/              # Backend service (Node.js/TypeScript)
├── platform-ui/               # Frontend service (React)
├── k8s/                       # Kubernetes manifests
│   ├── base/
│   └── overlays/
├── scripts/                   # Deployment and utility scripts
├── CLAUDE.md                  # Claude Code guidance
└── README.md                  # This file
```

## 🧪 Testing

```bash
# Run all tests
npm test

# Run security scans
detect-secrets scan --all-files

# Validate Kubernetes manifests
find k8s/ -name "*.yaml" | xargs kubeval

# Container security scanning
trivy image platform-api:latest
```

## 🚀 Deployment

### Development

```bash
kubectl apply -k k8s/overlays/development
```

### Production

```bash
kubectl apply -k k8s/overlays/production
```

### GitOps (Recommended)

Use ArgoCD or Flux for production deployments:

```bash
# Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Create application
argocd app create platform-stack \
  --repo <your-repo-url> \
  --path k8s/overlays/production \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace platform-system
```

## 🤝 Contributing

1. **Use the development environment**: Always use `devbox shell`
2. **Follow security practices**: No secrets in code
3. **Use Claude Code agents**: Leverage specialized agents for tasks
4. **Test thoroughly**: Run all security scans before committing
5. **Document changes**: Update relevant documentation

## 📖 Documentation

- **[CLAUDE.md](./CLAUDE.md)**: Comprehensive guide for Claude Code users
- **[.claude/agents/README.md](./.claude/agents/README.md)**: Agent documentation
- **[.github/workflows/](./.github/workflows/)**: CI/CD pipeline documentation

## 🆘 Support

### Common Issues

- **Tool not found**: Run `devbox shell` to load the development environment
- **Secrets not loading**: Check Teller configuration and Azure Key Vault access
- **Build failures**: Review security scan results and fix vulnerabilities

### Getting Help

1. Check the **CLAUDE.md** for detailed guidance
2. Use the **appropriate Claude Code agent** for specific domains
3. Review **CI/CD pipeline logs** for build issues
4. Consult **External Secrets documentation** for secret management

## 📄 License

This template is provided as-is for educational and development purposes. Review and customize according to your organization's requirements and compliance needs.

---

**Built with ❤️ for platform engineers who value security, productivity, and developer experience.**
