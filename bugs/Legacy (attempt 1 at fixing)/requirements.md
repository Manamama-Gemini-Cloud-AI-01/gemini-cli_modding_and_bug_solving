# Fix Ripgrep Installation Bug - Requirements Document

This document outlines the requirements for fixing the `ripgrep` installation failure for the `@google/gemini-cli` package on specific platforms like Termux.

## Core Features

- A `preinstall` script will be added to the `package.json` to execute a custom script before other installation steps.
- A new script, `scripts/install-ripgrep.js`, will be created to contain the platform-specific installation logic.
- The script will reliably detect if it is running in a Termux environment.
- If in Termux, the script will compile `ripgrep` from source using `cargo`.
- If not in Termux, the script will do nothing and allow the default dependency installation to proceed.

## User Stories

- As a Termux user, I want to be able to `npm install -g @google/gemini-cli` without the installation failing, so that I can use the tool on my Android device.
- As a developer contributing to `gemini-cli`, I want the installation process to be reliable across all supported platforms, including Termux, so that the user experience is smooth and requires no manual workarounds.

## Acceptance Criteria

- [ ] Running `npm install -g @google/gemini-cli` on a fresh Termux environment (with `git` and `rust` installed) completes successfully.
- [ ] After a successful installation on Termux, the `gemini` command is executable and functional.
- [ ] Running `npm install -g @google/gemini-cli` on other platforms (Windows, macOS, standard Linux) continues to work as expected.
- [ ] The custom `install-ripgrep.js` script contains clear comments and error handling.

## Non-functional Requirements

- **Compatibility:** The solution must not break the installation process on existing supported platforms.
- **Dependencies:** The fix will introduce a new build-time dependency on `git` and the `rust` toolchain (`cargo`) for the Termux platform. This must be documented.
- **Security:** The script should clone the `ripgrep` repository from its official, secure source on GitHub.