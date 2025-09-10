# Design Plan: Ripgrep Installation Fix

## 1. Overview

This document outlines the definitive plan to resolve the `ripgrep` installation failure within the `@google/gemini-cli` package, specifically for the Termux/Android environment. The goal is to implement a robust, maintainable solution that ensures `ripgrep` is correctly provisioned during installation.

## 2. Proposed Strategy

Our strategy acknowledges that while an upstream fix exists in the dependency's source code, its availability as a published `npm` package is not guaranteed. Therefore, the plan has two paths, prioritized for the cleanest possible solution.

### Plan A: Update the Upstream Dependency (Preferred)

The most maintainable and ideal solution is to use an officially fixed version of the `@lvce-editor/ripgrep` dependency.

1.  **Verify New Version**: First, we will check the `npm` registry to see if a version newer than `1.6.0` has been published.
2.  **Update and Clean**: If a new version is available, the fix is straightforward:
    *   Update the version of `@lvce-editor/ripgrep` in this project's `package.json`.
    *   Delete the empty workaround script at `scripts/install-ripgrep.js`.

This approach delegates the installation logic back to the dependency itself, which is the standard practice.

### Plan B: Implement a Local Preinstall Script (Fallback)

If no updated version of the dependency is available on `npm`, we will implement the fix directly within this project. This is less ideal as it requires maintaining custom logic, but it guarantees a fix.

1.  **Leverage `preinstall`**: We will use `npm`'s `preinstall` script hook in `package.json` to run a custom script *before* dependencies are installed.
2.  **Create a Smart Installer Script**: The currently empty `scripts/install-ripgrep.js` will be populated with logic that:
    *   **Detects the Platform**: Reliably checks if the environment is Termux.
    *   **Compiles on Termux**: If on Termux, it will compile `ripgrep` from source using `cargo`. This logic will be based on the successful methods discovered in the previous, roundabout fix attempts.
    *   **Skips on Other Platforms**: If not on Termux, the script will do nothing, allowing the standard (and functional) installation process of the dependency to proceed on platforms like Windows, macOS, and standard Linux.
3.  **Add Dependencies**: This plan requires adding the necessary Node.js packages for the installer script (e.g., `execa`, `fs-extra`) to this project's `devDependencies`.

## 3. Implementation Steps

1.  **Investigate**: Run `npm view @lvce-editor/ripgrep version` to determine if a version newer than `1.6.0` is published.
2.  **Execute Plan A or B**: Based on the result of the investigation, proceed with the corresponding plan.
3.  **Testing**: After implementation, conduct a clean install on both a Termux environment and a standard Linux environment to verify the fix and ensure no regressions were introduced.
