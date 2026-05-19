# Gemini CLI Git Worktree Commands

This repository provides a suite of custom slash commands designed for the [gemini-cli](https://github.com/google/gemini-cli), intended to streamline local Git workflows, especially when using agentic IDEs, through the utilization of **Git Worktrees**.

These commands facilitate the automated creation, synchronization, merging, and deallocation of isolated worktrees intended for feature development, defect resolution, and experimental implementation. The integration of worktrees ensures that the primary repository remains unpolluted, enables seamless context switching, and allows for the concurrent management of tasks without the necessity of stashing changes or risking data loss.

## Core Functionality and Command Reference

### 1. Workflow Initialization Commands (`/start:*`)

Situated within the `commands/start/` directory, these directives expect a short description as an argument (e.g., `/start:feature user-auth`). Upon execution, they automatically retrieve the latest state of the `main` branch, establish a new branch, and initialize a corresponding Git worktree within an adjacent directory structure (`../repo-<type>-<name>`).

| **Command** | **Description** |
| :--- | :--- |
| `/start:feature <name>` | Initializes a designated branch and worktree for feature development. |
| `/start:fix <name>` | Initializes a designated branch and worktree for defect resolution. |
| `/start:chore <name>` | Initiates a workspace for routine maintenance, dependency updates, or configuration modifications. |
| `/start:docs <name>` | Establishes a workspace exclusively for documentation enhancements. |
| `/start:refactor <name>` | Creates a workspace dedicated to code refactoring and optimization without altering core functionality. |
| `/start:spike <name>` | Provides a sandbox environment for exploratory development, which can be subsequently discarded. |
| `/start:test <name>` | Initializes a workspace focused strictly on the implementation of unit or integration testing. |

### 2. Workspace Management Commands

Located within the root `commands/` directory, these directives are designed for execution within an active worktree environment.

| **Command** | **Description** |
| :--- | :--- |
| `/smart-merge` | Verifies the working directory state, synchronizes with the `main` branch, integrates the current branch, and systematically deallocates both the worktree and associated temporary branches. |
| `/sync-main` | Retrieves the latest remote repository state and merges `origin/main` into the current worktree, suspending operations if merge conflicts require manual intervention. |
| `/discard` | Terminates the current workflow by abandoning the active worktree, forcefully deleting the associated local branch, and permanently removing the temporary directory. |

## Installation and Directory Architecture

To integrate these commands into a local `gemini-cli` environment, transfer the `.toml` configuration files into the designated custom commands directory, adhering to the following structure:

```text
~/.config/gemini-cli/commands/        # Or the appropriately configured custom path
├── discard.toml
├── smart-merge.toml
├── sync-main.toml
└── start/
    ├── chore.toml
    ├── docs.toml
    ├── feature.toml
    ├── fix.toml
    ├── refactor.toml
    ├── spike.toml
    └── test.toml
```

*Note: The `gemini-cli` framework automatically parses nested directories to generate colon-delimited subcommands (e.g., the file path `start/feature.toml` translates to the executable command `/start:feature`).*

## Operational Workflow Example

The following sequence illustrates a standard operational workflow utilizing this command suite:

**1. Feature Initialization:**

```bash
> /start:feature user-login
# Automatically establishes the branch 'feature/user-login' and mounts it at '../repo-feature-user-login'
```

**2. Workspace Transition:**

```bash
> cd ../repo-feature-user-login
```

**3. Repository Synchronization (Mid-development):**

```bash
> /sync-main
# Automatically fetches and merges the origin/main branch into feature/user-login
```

**4. Integration and Deallocation:**

```bash
> /smart-merge
# Merges modifications into the main branch, deletes the feature/user-login branch, and removes the worktree directory.
```

**5. (Alternative) Workflow Termination:**

```bash
> /start:spike weird-architecture
> cd ../repo-spike-weird-architecture
> /discard
# Safely deletes the experimental branch and permanently removes the associated worktree.
```

## Contribution Guidelines

Contributions to this repository are actively encouraged, both for improvements and new command additions. Just open a pull request!

## Licensing

This project is distributed under the terms of the **Apache License 2.0**. For comprehensive details, please refer to the [LICENSE](LICENSE) file.
