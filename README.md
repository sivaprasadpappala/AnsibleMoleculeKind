# AnsibleMoleculeKind

A repository demonstrating automated testing of Ansible roles using **Molecule** and **Kind** (Kubernetes in Docker) with a Jenkins CI/CD pipeline.

## Overview

This project showcases best practices for validating Ansible roles in containerized Kubernetes environments. It integrates:
- **Ansible Core** - Infrastructure automation framework
- **Molecule** - Ansible role testing framework
- **Kind** - Lightweight Kubernetes cluster in Docker
- **Jenkins** - CI/CD pipeline automation

## Repository Structure

```
AnsibleMoleculeKind/
├── ansible/
│   └── roles/
│       └── demo/           # Example Ansible role with molecule tests
├── k8s/                    # Kubernetes configuration files
├── Jenkinsfile             # CI/CD pipeline definition
├── .gitignore              # Git ignore rules
└── README.md               # This file
```

## Prerequisites

- **Python 3.x**
- **Docker** (for Kind and container support)
- **Git**
- **Jenkins** (for pipeline execution)

## Installation & Setup

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/sivaprasadpappala/AnsibleMoleculeKind.git
   cd AnsibleMoleculeKind
   ```

2. **Create a Python virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install --upgrade pip
   pip install \
     ansible-core==2.15.13 \
     ansible==8.7.0 \
     molecule==6.0.2 \
     docker
   ```

## Running Tests

### Local Molecule Testing

Navigate to a role directory and run molecule tests:

```bash
cd ansible/roles/demo
molecule test
```

This will:
1. Create a Kind cluster
2. Deploy and configure test instances
3. Run playbooks and tests
4. Clean up resources

### Available Molecule Commands

```bash
molecule create      # Create test instances
molecule converge    # Run the playbook
molecule verify      # Run verification tests
molecule destroy     # Destroy test instances
molecule idempotence # Test playbook idempotence
```

## CI/CD Pipeline

The `Jenkinsfile` defines an automated pipeline with the following stages:

### Pipeline Stages

1. **Checkout** - Clones the main branch
2. **Create Feature Branch** - Creates a branch for CI validation
3. **Molecule Test with kind** - Runs Molecule tests using Kind
4. **Commit & Push** - Commits validation results (excluding `.venv`)
5. **Create Pull Request** - Opens a PR for review

### Jenkins Configuration

**Required Credentials:**
- `github-token` - GitHub Personal Access Token for PR creation and pushing

**Environment Variables:**
- `REPO_URL` - Repository URL
- `BASE_BRANCH` - Base branch (default: `main`)
- `FEATURE_BRANCH` - CI branch name: `ci-${BUILD_NUMBER}`

### Running the Pipeline

The pipeline is triggered by Jenkins and will:
1. Set up a Python virtual environment
2. Install Ansible and Molecule dependencies
3. Test the demo role with Kind
4. Push results and create a PR

## Project Structure Details

### ansible/roles/demo/
This directory contains an example Ansible role with:
- `tasks/` - Role tasks
- `handlers/` - Event handlers
- `templates/` - Jinja2 templates
- `molecule/` - Molecule test configuration

### k8s/
Kubernetes manifests for deployment configurations (if applicable to your setup).

## Best Practices

- ✅ Always test roles locally before pushing
- ✅ Use Kind for lightweight, fast Kubernetes testing
- ✅ Exclude `.venv` from git commits
- ✅ Keep Ansible and Molecule versions consistent
- ✅ Verify idempotence of playbooks regularly

## Troubleshooting

### Docker not available
Ensure Docker is running:
```bash
docker ps
```

### Molecule tests fail locally
1. Check Python version: `python3 --version`
2. Ensure virtual environment is activated
3. Verify Docker connectivity: `docker ps`
4. Re-run with verbose output: `molecule test -v`

### Jenkins build failures
1. Check if `github-token` credential is configured
2. Verify Jenkins has Docker and Python installed
3. Review Jenkins logs for detailed errors

## Contributing

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make changes and test locally with Molecule
3. Commit with clear messages
4. Push and open a PR
5. Ensure CI pipeline passes

## License

MIT (or specify your license)

## Author

**Siva Prasad Pappala**  
GitHub: [@sivaprasadpappala](https://github.com/sivaprasadpappala)

## Resources

- [Ansible Documentation](https://docs.ansible.com/)
- [Molecule Documentation](https://molecule.readthedocs.io/)
- [Kind Documentation](https://kind.sigs.k8s.io/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)

---

**Last Updated:** January 2026
