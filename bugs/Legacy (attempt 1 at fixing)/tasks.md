# Fix Ripgrep Installation Bug - Task List

ver. 1.1, User's 

## Implementation Tasks

- [ ] 0. Test if 1.4 obtains. Do so in Terminal. 
- [ ] 0.1 Do install it "by hand".  
- [ ] 0.2 Depending on 0.1 results, we need to modify some logic of 1.3 to 1.5 below. So then we abort to rethink. 


- [ ] 1. **Create and Implement the `install-ripgrep.js` Script**
    - [ ] 1.1. **Create Script File**
        - *Goal*: Create the new script file at `scripts/install-ripgrep.js`.
        - *Details*: The file will be a Node.js script.
        - *Requirements*: Core Features 1 & 2.
    - [ ] 1.2. **Implement Platform Detection and Pre-checks**
        - *Goal*: Add logic to the script to detect the environment and check for prerequisites.
        - *Details*: The script must first check if the `rg` binary already exists in the target `bin/` directory to avoid recompilation. It will then check for a Termux environment using `process.env.TERMUX_PREFIX`. It must also verify that `git` and `cargo` are available in the system's PATH.
        - *Requirements*: Core Feature 3, Non-functional Requirements 2.
    - [ ] 1.3. **Implement Ripgrep Compilation Logic**
        - *Goal*: Add the core logic to compile `ripgrep` from source if running on Termux.
        - *Details*: Use `child_process.execSync` to perform the following sequence: `git clone`, `git checkout 13.0.0`, and `cargo install --path . --target aarch64-linux-android`.
        - *Requirements*: Core Feature 4, Non-functional Requirements 3.
    - [ ] 1.4. **Implement Binary Verification and Cleanup**
        - *Goal*: Verify the installed `ripgrep` binary and clean up temporary files.
        - *Details*: After successful compilation, verify that `rg` is in the system's PATH. If not, attempt to temporarily add `$HOME/.cargo/bin` to PATH for the current script execution and re-verify. Then, execute `rg --version` to confirm functionality. If `rg` is still not found or `rg --version` fails, the script will exit with an error. Finally, remove the temporary source directory (`$PREFIX/tmp/ripgrep`).
        - *Requirements*: Core Feature 4.
    - [ ] 1.5. **Implement Robust Error Handling**
        - *Goal*: Ensure the script fails gracefully and provides clear user guidance.
        - *Details*: All shell commands must be wrapped in a `try...catch` block. On failure, the script must log a detailed error message explaining what went wrong and provide simple, clear instructions for the user to perform the compilation manually. The script must exit with a non-zero status code upon failure.
        - *Requirements*: Acceptance Criteria 4.

- [ ] 2. **Integrate the Script into the Build Process**
    - [ ] 2.1. **Modify `package.json`**
        - *Goal*: Hook the new script into the `npm install` lifecycle.
        - *Details*: Add a `preinstall` script to the `scripts` section of the root `package.json` file: `"preinstall": "node scripts/install-ripgrep.js"`.
        - *Requirements*: Core Feature 1.

## Task Dependencies

- Task 0 (all subtasks) must be completed before Task 1 and 2.
- Task 1 (all subtasks) must be completed before Task 2.
-
## Estimated Timeline

- Task 1: 2 hours
- Task 2: 0.5 hours
- **Total: 2.5 hours**
