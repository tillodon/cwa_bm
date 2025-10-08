# GitFlow Branching Strategy

This document outlines the GitFlow branching strategy implemented for the Corona-Warn-App mobile application project.

## Overview

GitFlow is a branching model designed around project releases, providing a robust framework for managing feature development, releases, and hotfixes in a structured manner.

## Branch Types

### Main Branches

#### `main` (Production)
- **Purpose**: Contains production-ready code
- **Lifetime**: Permanent
- **Protection**: Branch protection rules prevent direct commits
- **Merges from**: `release/*` and `hotfix/*` branches only
- **Deployment**: Automatically deploys to production environment
- **Tagging**: All commits are tagged with version numbers

#### `develop` (Integration)
- **Purpose**: Integration branch where features come together
- **Lifetime**: Permanent  
- **Protection**: Branch protection rules prevent direct commits
- **Merges from**: `feature/*`, `release/*`, and `hotfix/*` branches
- **Deployment**: Automatically deploys to staging environment
- **Testing**: All integration tests run automatically

### Supporting Branches

#### `feature/*` (Feature Development)
- **Purpose**: Develop new features and enhancements
- **Lifetime**: Temporary (deleted after merge)
- **Branched from**: `develop`
- **Merged to**: `develop`
- **Naming**: `feature/[feature-name]`
- **Examples**: 
  - `feature/bluetooth-scanning`
  - `feature/risk-calculation`
  - `feature/user-interface`

#### `release/*` (Release Preparation)
- **Purpose**: Prepare new production releases
- **Lifetime**: Temporary (deleted after merge)
- **Branched from**: `develop`
- **Merged to**: `main` and `develop`
- **Naming**: `release/v[version]`
- **Examples**:
  - `release/v1.0.0`
  - `release/v1.1.0`
  - `release/v2.0.0`

#### `hotfix/*` (Critical Fixes)
- **Purpose**: Fix critical issues in production
- **Lifetime**: Temporary (deleted after merge)
- **Branched from**: `main`
- **Merged to**: `main` and `develop`
- **Naming**: `hotfix/[issue-description]`
- **Examples**:
  - `hotfix/security-patch`
  - `hotfix/bluetooth-crash`
  - `hotfix/api-timeout`

## Workflow Commands

### Feature Development Workflow

#### Start a Feature
```bash
# Ensure you're on develop and up to date
git checkout develop
git pull origin develop

# Create and switch to feature branch
git checkout -b feature/bluetooth-scanning

# Work on your feature...
# Make commits as you develop

# Push feature branch for collaboration (optional)
git push -u origin feature/bluetooth-scanning
```

#### Complete a Feature
```bash
# Ensure feature is up to date
git checkout feature/bluetooth-scanning
git pull origin feature/bluetooth-scanning

# Switch to develop and update
git checkout develop
git pull origin develop

# Merge feature into develop (no-fast-forward to preserve history)
git merge --no-ff feature/bluetooth-scanning

# Push updated develop
git push origin develop

# Delete feature branch locally
git branch -d feature/bluetooth-scanning

# Delete feature branch remotely (if pushed)
git push origin --delete feature/bluetooth-scanning
```

### Release Workflow

#### Start a Release
```bash
# Switch to develop and ensure it's up to date
git checkout develop
git pull origin develop

# Create release branch
git checkout -b release/v1.0.0

# Update version numbers, changelog, etc.
# Commit version updates
git add .
git commit -m "chore: Bump version to 1.0.0"

# Push release branch
git push -u origin release/v1.0.0
```

#### Finish a Release
```bash
# Merge to main
git checkout main
git pull origin main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# Merge back to develop
git checkout develop
git pull origin develop
git merge --no-ff release/v1.0.0
git push origin develop

# Delete release branch
git branch -d release/v1.0.0
git push origin --delete release/v1.0.0
```

### Hotfix Workflow

#### Start a Hotfix
```bash
# Branch from main
git checkout main
git pull origin main
git checkout -b hotfix/security-patch

# Make necessary fixes
# Commit fixes
git add .
git commit -m "fix: Address security vulnerability in authentication"

# Push hotfix branch
git push -u origin hotfix/security-patch
```

#### Finish a Hotfix
```bash
# Merge to main
git checkout main
git pull origin main
git merge --no-ff hotfix/security-patch
git tag -a v1.0.1 -m "Hotfix version 1.0.1"
git push origin main --tags

# Merge to develop
git checkout develop
git pull origin develop
git merge --no-ff hotfix/security-patch
git push origin develop

# Delete hotfix branch
git branch -d hotfix/security-patch
git push origin --delete hotfix/security-patch
```

## Branch Protection Rules

### `main` Branch Protection
- Require pull request reviews before merging
- Dismiss stale pull request approvals when new commits are pushed
- Require status checks to pass before merging
- Require branches to be up to date before merging
- Include administrators in restrictions
- Allow force pushes: **NO**
- Allow deletions: **NO**

### `develop` Branch Protection
- Require pull request reviews before merging
- Require status checks to pass before merging
- Require branches to be up to date before merging
- Include administrators in restrictions
- Allow force pushes: **NO**
- Allow deletions: **NO**

## Automated Workflows

### CI/CD Pipeline Integration

#### Feature Branches
- Trigger: Push to `feature/*`
- Actions:
  - Run unit tests
  - Run integration tests
  - Code quality analysis
  - Security scanning
  - Build artifacts (iOS/Android)

#### Develop Branch
- Trigger: Merge to `develop`
- Actions:
  - All feature branch checks
  - Deploy to staging environment
  - Run end-to-end tests
  - Update deployment status

#### Release Branches
- Trigger: Push to `release/*`
- Actions:
  - Run full test suite
  - Performance testing
  - Security audit
  - Generate release notes
  - Build release candidates

#### Main Branch
- Trigger: Merge to `main`
- Actions:
  - Deploy to production
  - Send deployment notifications
  - Update monitoring dashboards
  - Archive release artifacts

## Best Practices

### Commit Messages
Follow conventional commit format:
```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Branch Naming
- Use lowercase with hyphens
- Be descriptive but concise
- Include ticket/issue numbers when applicable
- Examples:
  - `feature/CWA-123-bluetooth-scanning`
  - `hotfix/critical-auth-bug`
  - `release/v1.2.0`

### Pull Request Guidelines
- Use descriptive titles and descriptions
- Link to relevant issues or tickets
- Include testing instructions
- Request reviews from appropriate team members
- Ensure all CI checks pass before merging

### Merge Strategy
- **Feature to Develop**: Merge commit (--no-ff)
- **Release to Main/Develop**: Merge commit (--no-ff)
- **Hotfix to Main/Develop**: Merge commit (--no-ff)

This preserves the branch history and makes it clear where features were developed.

## Troubleshooting

### Common Issues

#### Merge Conflicts
```bash
# If conflicts occur during merge
git status  # See conflicted files
# Edit files to resolve conflicts
git add .   # Stage resolved files
git commit  # Complete the merge
```

#### Accidental Commits to Wrong Branch
```bash
# Move commits to correct branch
git checkout correct-branch
git cherry-pick <commit-hash>
git checkout wrong-branch
git reset --hard HEAD~1  # Remove commit from wrong branch
```

#### Force Push Recovery
```bash
# If someone force-pushed, recover with reflog
git reflog
git reset --hard <previous-commit-hash>
```

## Team Collaboration

### Code Review Process
1. Create feature branch from `develop`
2. Develop feature with regular commits
3. Push branch and create pull request
4. Request reviews from team members
5. Address feedback and update branch
6. Merge when approved and CI passes

### Release Planning
- **Sprint Planning**: Decide which features go into next release
- **Feature Freeze**: No new features after release branch creation
- **Testing Phase**: QA testing on release branch
- **Release Deployment**: Merge to main and deploy
- **Post-Release**: Hotfixes if critical issues found

### Emergency Procedures
For critical production issues:
1. Create hotfix branch immediately from `main`
2. Develop and test fix quickly
3. Deploy hotfix through expedited review process
4. Communicate with team about emergency deployment
5. Follow up with post-mortem analysis

---

This GitFlow strategy ensures organized development, clear release processes, and stable production deployments while maintaining team collaboration and code quality standards.