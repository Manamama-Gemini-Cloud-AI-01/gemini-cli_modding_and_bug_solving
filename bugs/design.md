# Fix Ripgrep Installation Bug - Design Document

## Overview

This document outlines the technical design for a universal fix to the `ripgrep` installation issue within the `@google/gemini-cli` package. The solution involves creating a custom Node.js script that runs during the `npm install` process. This script will handle the platform-specific provisioning of the `ripgrep` binary, ensuring a seamless installation for all users, including those on Termux.

## Architecture

The solution modifies the installation lifecycle of the `@google/gemini-cli` package. By hooking into npm's `preinstall` script event, we intercept the installation process before any dependencies attempt to run their own `postinstall` scripts. This allows us to programmatically check the environment and provide the required `ripgrep` binary by either compiling it from source or allowing the default download mechanism to proceed.

## Components and Interfaces

- **`package.json`**
  - **Interface:** The `scripts` section will be modified.
  - **Change:** A `preinstall` script will be added: `"preinstall": "node scripts/install-ripgrep.js"`.

- **`scripts/install-ripgrep.js` (New Component)**
  - **Interface:** A new Node.js script.
  - **Logic:**
    1.  **Platform Detection:**
        - The script will first check if the `rg` binary is already available in the expected path to avoid redundant compilations.
        - If not, it will check if `process.env.TERMUX_PREFIX` exists to identify the environment as Termux.
    2.  **Termux-Specific Logic (Compilation):**
        - If Termux is detected, the script will use `child_process.execSync` to perform the following actions:
          - Check for the presence of required system dependencies (`git`, `cargo`) and provide a clear error message if they are missing.
          - Clone the `ripgrep` repository from `https://github.com/BurntSushi/ripgrep.git` into a temporary directory (`$PREFIX/tmp/ripgrep`).
          - Check out the specific version tag (`13.0.0`) that the original dependency expects.
          - Execute `cargo install --path . --target aarch64-linux-android`.
          - Verify that `rg` is now in the system's PATH. If not, attempt to temporarily add `$HOME/.cargo/bin` to PATH for the current script execution and re-verify. Then, execute `rg --version` to confirm functionality. If `rg` is still not found or `rg --version` fails, the script will exit with an error.
          - Perform cleanup by removing the temporary source directory (`$PREFIX/tmp/ripgrep`).
    3.  **Default Logic (Other Platforms):**
        - If the environment is not Termux, the script will exit gracefully with code 0, allowing the standard installation process for `@lvce-editor/ripgrep` to continue.

## Data Models

No new data models will be introduced. The design focuses on procedural logic within the installation scripts.

## Error Handling

The `install-ripgrep.js` script will include `try...catch` blocks for all shell command executions.
- If any command (e.g., `git clone`, `cargo build`) fails, the script will:
  - Log a detailed error message to the console, indicating which step failed and providing clear, actionable instructions for the user to perform the compilation manually.
  - Exit the process with a non-zero exit code (`process.exit(1)`), which will halt the `npm install` process and alert the user to the failure.

## Testing Strategy

- **Manual Testing (Primary):**
  - A Termux environment will be used to run `npm install -g @google/gemini-cli` on the modified package to verify that the compilation script executes correctly and the CLI is installed successfully.
- **Other Platforms (Windows/macOS/Standard Linux):**
  - Testing on other platforms will be skipped, as the script is designed to have no effect in non-Termux environments, allowing the default installation to proceed.
- **Automated Testing:**
  - No automated tests will be created for this script.