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
