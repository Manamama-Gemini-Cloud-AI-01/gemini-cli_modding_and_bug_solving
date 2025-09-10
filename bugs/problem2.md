# Problem Analysis: `gemini-cli` Installation Failure on Termux

## 1. Symptom
When installing `@google/gemini-cli` globally on Termux for Android (`aarch64`), the installation process fails during the `postinstall` step of a dependency. The verbose log file (`2025-09-05T08_36_53_332Z-debug-0.log`) shows the error `VError: Unknown platform: android`, originating from the `@lvce-editor/ripgrep` package. This failure prevents the critical `ripgrep` binary from being installed.

## 2. Root Cause Analysis
The failure is a result of several compounding factors in the dependency chain:

*   **Dependency Script Failure**: The core issue lies in the `@lvce-editor/ripgrep` wrapper package. Its installation script is designed to download a pre-built binary of `ripgrep` (v13.0.0-10). The script checks `process.platform` to determine which binary to download, but it does not have a case for the `'android'` value returned by Node.js in Termux, causing it to crash.

*   **No Pre-built Binary**: Even if the script handled the 'android' platform, no official pre-built `ripgrep` binary exists for the `aarch64-linux-android` target. The wrapper script does not include a fallback to compile from source.

*   **Ecosystem Limitations**: The `npm` ecosystem relies on package-specific scripts for native dependencies and lacks a universal mechanism (like pip's `--no-binary`) to force compilation from source, making it brittle on non-standard platforms.

*   **Key Discovery**: Despite the wrapper's failure, it was discovered that `ripgrep` **can be compiled successfully** from source directly on the Termux platform using `cargo`. This proves the underlying tool is compatible and the failure is entirely within the installation logic of the wrapper package.

## 3. Current Project State
This `gemini-cli` project is currently in a non-functional state on Termux due to two main factors:

1.  **Outdated Dependency**: The project's `package.json` references an older version of `@lvce-editor/ripgrep` which contains the platform detection bug. An upstream fix exists on GitHub but has not been incorporated into this project via a dependency update.

2.  **Local Workaround**: The project contains an empty file at `scripts/install-ripgrep.js`. This is a deliberate, committed workaround intended to override and disable the dependency's broken `postinstall` script. While this prevents the installation from crashing, it also ensures that `ripgrep` is never actually installed.





# Solution: 
Solved by others elegantly, see: https://github.com/google-gemini/gemini-cli/issues/7895

In short, even this is enough: 
```
rm -rf '/data/data/com.termux/files/usr/lib/node_modules/@google/gemini-cli'
npm install -g @google/gemini-cli@preview --ignore-scripts --force
```


