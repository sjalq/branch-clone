# branch-clone

A powerful bash script that uses git worktrees to create isolated working directories for different branches. Perfect for working on multiple branches simultaneously without the overhead of full repository clones.

## What Are Git Worktrees?

Git worktrees allow you to have multiple working directories attached to the same Git repository, each with a different branch checked out. Unlike cloning, worktrees share the git database, making them:

- **Faster**: No need to clone entire repository
- **Space efficient**: Shares git objects between all worktrees
- **Always in sync**: One `git fetch` updates all worktrees
- **Isolated environments**: Each worktree has separate working files (node_modules, builds, .env, etc.)

## Features

- ✅ **Smart worktree management**: Creates or updates worktrees automatically
- ✅ **Branch handling**: Checks out existing branches or creates new ones
- ✅ **Remote sync**: Automatically pushes new branches and sets up tracking
- ✅ **Cross-platform**: Works on macOS and Windows (Git Bash)
- ✅ **Claude Code integration**: Launch Claude Code directly in the worktree
- ✅ **Agent-friendly**: Minimal output mode for automated workflows
- ✅ **Global installation**: Install once, use anywhere

## Installation

### Quick Install (Recommended)

```bash
# Clone and install globally
git clone https://github.com/sjalq/branch-clone.git
cd branch-clone
./branch-clone --install
```

### Manual Install

```bash
# Download script
curl -O https://raw.githubusercontent.com/sjalq/branch-clone/main/branch-clone
chmod +x branch-clone

# Move to your PATH
sudo mv branch-clone /usr/local/bin/
```

## Usage

### Basic Usage

```bash
# From current repository
branch-clone . feature-branch

# From Git URL (first time clones to ~/git/repo-name)
branch-clone https://github.com/user/repo.git feature-branch
```

### With Claude Code Integration

```bash
# Setup worktree and launch Claude Code
branch-clone . bugfix-123 --claude "fix the authentication bug"

# Continue previous Claude session
branch-clone . feature-x --claude --continue

# Use any Claude Code flags
branch-clone . refactor --claude --model opus "refactor the API layer"
```

## Examples

### Example 1: Starting Work on a New Feature

**Scenario**: You're working on the main branch and need to start a new feature without disrupting your current setup.

```bash
cd ~/git/myapp
branch-clone . feature-payment-integration
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Repository: /Users/sjalq/git/myapp
Branch: feature-payment-integration
Worktree: /Users/sjalq/git/myapp-feature-payment-integration

Creating worktree at /Users/sjalq/git/myapp-feature-payment-integration...
Switched to a new branch 'feature-payment-integration'

✓ Worktree created successfully
✓ Branch 'feature-payment-integration' created
✓ Pushed to origin and set up tracking

=== Summary ===
Worktree ready at: /Users/sjalq/git/myapp-feature-payment-integration

Next steps:
  cd /Users/sjalq/git/myapp-feature-payment-integration
  npm install  # Install dependencies
  npm run dev  # Start development
```

### Example 2: Reviewing a Pull Request

**Scenario**: A teammate created PR #456 and you need to test it locally without affecting your current work.

```bash
branch-clone https://github.com/company/project.git pr-456-database-migration
cd ~/git/project-pr-456-database-migration
npm install
npm test
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Extracting repository info from URL...
Repository: project
Main repo will be: /Users/sjalq/git/project
Worktree will be: /Users/sjalq/git/project-pr-456-database-migration

Main repository already exists at /Users/sjalq/git/project
Fetching latest changes...

Creating worktree at /Users/sjalq/git/project-pr-456-database-migration...
Switched to a new branch 'pr-456-database-migration'

✓ Worktree created successfully
✓ Ready for PR review

=== Summary ===
Worktree ready at: /Users/sjalq/git/project-pr-456-database-migration
```

### Example 3: Quick Hotfix While Working on a Feature

**Scenario**: You're deep into feature development when a critical bug is reported. You need to fix it immediately without losing your current work.

```bash
# Currently in feature branch with uncommitted changes
pwd  # /Users/sjalq/git/webapp-feature-dashboard

# Create hotfix worktree
branch-clone . hotfix-login-timeout
cd ~/git/webapp-hotfix-login-timeout

# Fix the bug, test, commit
git add .
git commit -m "Fix login timeout issue"
git push

# Return to feature work
cd ~/git/webapp-feature-dashboard
# Your uncommitted changes are still there!
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Repository: /Users/sjalq/git/webapp (main repository)
Branch: hotfix-login-timeout
Worktree: /Users/sjalq/git/webapp-hotfix-login-timeout

Creating worktree at /Users/sjalq/git/webapp-hotfix-login-timeout...
Switched to a new branch 'hotfix-login-timeout'

✓ Worktree created successfully
✓ Branch 'hotfix-login-timeout' created
✓ Pushed to origin and set up tracking

=== Summary ===
Worktree ready at: /Users/sjalq/git/webapp-hotfix-login-timeout
```

### Example 4: First-Time Repository Clone

**Scenario**: You want to start working on a new open-source project you've never cloned before.

```bash
branch-clone https://github.com/facebook/react.git explore-hooks-implementation
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Extracting repository info from URL...
Repository: react
Main repo will be: /Users/sjalq/git/react
Worktree will be: /Users/sjalq/git/react-explore-hooks-implementation

Main repository does not exist. Cloning...
Cloning into '/Users/sjalq/git/react'...
remote: Enumerating objects: 234567, done.
remote: Counting objects: 100% (1234/1234), done.
remote: Compressing objects: 100% (567/567), done.
remote: Total 234567 (delta 890), reused 1100 (delta 600)
Receiving objects: 100% (234567/234567), 45.67 MiB | 8.34 MiB/s, done.
Resolving deltas: 100% (123456/123456), done.

Creating worktree at /Users/sjalq/git/react-explore-hooks-implementation...
Switched to a new branch 'explore-hooks-implementation'

✓ Worktree created successfully
✓ Main repository cloned to /Users/sjalq/git/react

=== Summary ===
Worktree ready at: /Users/sjalq/git/react-explore-hooks-implementation

Next steps:
  cd /Users/sjalq/git/react-explore-hooks-implementation
  npm install
```

### Example 5: Claude Code Integration for AI-Assisted Development

**Scenario**: You want to use Claude to help implement a new feature.

```bash
branch-clone . feature-user-analytics --claude "Add user analytics dashboard with charts for daily active users, retention rate, and session duration. Use the existing Chart.js library."
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Repository: /Users/sjalq/git/dashboard
Branch: feature-user-analytics
Worktree: /Users/sjalq/git/dashboard-feature-user-analytics

Creating worktree...
✓ Worktree ready at /Users/sjalq/git/dashboard-feature-user-analytics

Launching Claude Code...

[Claude Code session starts]
Claude: I'll help you add a user analytics dashboard. Let me first examine the existing codebase structure...
```

### Example 6: Continuing a Claude Code Session

**Scenario**: You started working with Claude yesterday and want to continue where you left off.

```bash
branch-clone . feature-api-refactor --claude --continue
```

**Expected Output**:
```
=== branch-clone v1.0.0 ===

Repository: /Users/sjalq/git/backend
Branch: feature-api-refactor
Worktree: /Users/sjalq/git/backend-feature-api-refactor

Worktree already exists at /Users/sjalq/git/backend-feature-api-refactor
Branch 'feature-api-refactor' already checked out

✓ Using existing worktree

Launching Claude Code with --continue...

[Claude Code resumes previous session]
Claude: Welcome back! Last time we were refactoring the authentication endpoints. Let me show you what we completed...
```

### Example 7: Running Tests in Parallel Across Branches

**Scenario**: You want to verify that tests pass on multiple branches simultaneously.

```bash
# Terminal 1
branch-clone . feature-a
cd ~/git/myapp-feature-a
npm install && npm test

# Terminal 2
branch-clone . feature-b
cd ~/git/myapp-feature-b
npm install && npm test

# Terminal 3
branch-clone . main
cd ~/git/myapp-main
npm install && npm test
```

**Benefits**:
- All three test suites run in parallel
- Each has isolated `node_modules` and dependencies
- No conflicts or race conditions
- One `git fetch` updates all worktrees

### Example 8: Experimenting with Different Approaches

**Scenario**: You're not sure which implementation approach is better, so you want to try both.

```bash
# Approach 1: Using Redux
branch-clone . experiment-redux-state
cd ~/git/app-experiment-redux-state
npm install redux react-redux
# Implement using Redux...

# Approach 2: Using Context API (in another terminal)
branch-clone . experiment-context-state
cd ~/git/app-experiment-context-state
# Implement using Context API...

# Compare both running side-by-side
# Terminal 1: npm run dev  # Port 3000
# Terminal 2: npm run dev -- --port 3001
```

### Example 9: Working with Environment-Specific Configurations

**Scenario**: You need different `.env` files for different features.

```bash
# Production hotfix
branch-clone . hotfix-prod-issue
cd ~/git/api-hotfix-prod-issue
cp .env.production .env
npm run test:prod

# Development feature (in another terminal)
branch-clone . feature-new-endpoint
cd ~/git/api-feature-new-endpoint
cp .env.development .env
npm run dev
```

**Result**: Each worktree maintains its own `.env` file, preventing accidental environment mixing.

### Example 10: Automated Workflow for Agent Systems

**Scenario**: A parent Claude agent wants to delegate specific tasks to sub-agents.

```bash
# Parent agent script
for task_id in 101 102 103; do
  branch-clone . task-$task_id --claude "$(cat tasks/$task_id.txt)" &
done
wait
```

**Expected Output** (per task):
```
=== branch-clone v1.0.0 ===
Repository: /Users/sjalq/git/project
Branch: task-101
Worktree: /Users/sjalq/git/project-task-101
✓ Worktree ready
Launching Claude Code...
```

**Benefits**:
- Minimal output mode (automatic with `--claude`)
- Parallel task execution
- Isolated working directories
- Full Claude Code integration

### Example 11: Checking Worktree Status

**Scenario**: You've created several worktrees and want to see them all.

```bash
git worktree list
```

**Expected Output**:
```
/Users/sjalq/git/myapp              abc1234 [main]
/Users/sjalq/git/myapp-feature-auth def5678 [feature-auth]
/Users/sjalq/git/myapp-feature-api  ghi9012 [feature-api]
/Users/sjalq/git/myapp-hotfix       jkl3456 [hotfix-critical]
```

### Example 12: Cleaning Up After Review

**Scenario**: You've finished reviewing a PR and want to remove the worktree.

```bash
# Done with review
cd ~
git worktree remove ~/git/project-pr-456-database-migration

# Optionally delete the remote branch if merged
cd ~/git/project
git branch -d pr-456-database-migration
git push origin --delete pr-456-database-migration
```

**Expected Output**:
```
Removing worktrees/pr-456-database-migration: valid
```

## How It Works

### Local Repository (`.`)

```bash
cd ~/git/myproject
branch-clone . feature-auth
```

**What happens:**
1. Detects main repository (even if you're in a worktree)
2. Creates worktree at `~/git/myproject-feature-auth`
3. Checks out or creates `feature-auth` branch
4. Sets up remote tracking if new branch

### Git URL

```bash
branch-clone https://github.com/user/awesome.git bugfix
```

**What happens:**
1. **First time**: Clones main repo to `~/git/awesome`
2. Creates worktree at `~/git/awesome-bugfix`
3. **Subsequent times**: Uses existing main repo (no re-cloning!)

## Example Workflows

### Working on Multiple Features

```bash
# Main branch
cd ~/git/myproject

# Feature 1 - New authentication
branch-clone . feature-auth
cd ~/git/myproject-feature-auth
npm install
npm run dev

# Feature 2 - New API (in another terminal)
branch-clone . feature-api
cd ~/git/myproject-feature-api
npm install
npm test

# Hotfix (in another terminal)
branch-clone . hotfix-critical
cd ~/git/myproject-hotfix-critical
npm install
npm run build
```

Each worktree has:
- ✅ Separate `node_modules`
- ✅ Separate `.env` files
- ✅ Separate build outputs
- ✅ Shared git database

### Code Review Workflow

```bash
# Review PR #123
branch-clone . pr-123
cd ~/git/myproject-pr-123
npm install
npm test

# When done
git worktree remove ~/git/myproject-pr-123
```

### Agent Automation

```bash
# Parent Claude agent delegates work
branch-clone . task-123 --claude "implement the feature from issue #123"

# Minimal output mode automatically activated
# Claude Code launches in correct worktree
# All output streams to parent agent
```

## Command Reference

### Options

- `--install` - Install script globally to `/usr/local/bin`
- `--version` - Show version number
- `--help` - Show help message
- `--claude [ARGS]` - Launch Claude Code after setup with specified arguments

### Output Modes

**Normal mode** (default):
- Detailed step-by-step logging
- Repository structure information
- Comprehensive summary with next steps

**Claude mode** (`--claude` flag):
- Minimal logging
- Essential info only
- Streams Claude Code output directly

## Worktree Management

```bash
# List all worktrees
git worktree list

# Remove a worktree
git worktree remove ~/git/myproject-feature-branch

# Cleanup stale worktrees
git worktree prune
```

## Requirements

- Git 2.5+ (for worktree support)
- Bash 4.0+
- Optional: Claude Code CLI (for `--claude` integration)

## Comparison: Worktrees vs Clones

| Feature | Worktrees | Multiple Clones |
|---------|-----------|-----------------|
| Disk space | Minimal (shares objects) | High (duplicates everything) |
| Setup speed | Fast (hardlinks) | Slower (full copies) |
| Fetch/pull | Once for all worktrees | Separately for each clone |
| Branch refs | Shared instantly | Need to push/pull |
| Use case | Multiple branches, same repo | Different repos or experiments |

## Troubleshooting

### "Branch is already checked out"

Git prevents checking out the same branch in multiple worktrees. Use a different branch name or remove the existing worktree first.

### "Could not push to remote"

This is usually okay for local branches. Push manually when ready:
```bash
git push -u origin branch-name
```

### Claude CLI not found

Install Claude Code CLI first:
```bash
# Follow instructions at
https://docs.anthropic.com/claude/docs/claude-cli
```

## Contributing

Issues and pull requests welcome! This script is designed to be extended with additional features.

## License

MIT License - feel free to use and modify

## Author

Created for efficient multi-branch workflows and Claude Code automation.

## Related

- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)
- [Claude Code](https://claude.com/claude-code)
