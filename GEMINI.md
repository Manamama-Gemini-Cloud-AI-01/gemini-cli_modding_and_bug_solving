## How to answer "what have been the new features in the updates"

To answer questions about new features in updates, I will use the following method:

1.  I will first attempt to locate a `CHANGELOG.md` or `RELEASES.md` file in the repository.
2.  If I cannot find a release file, I will use the `github.list_tags` tool to identify the most recent release tags.
3.  I will then use the `github.list_commits` tool to get the commits associated with each of the last two release tags.
4.  I will then summarize the changes for each release based on the commit messages between the two tags.
5.  If I cannot determine the releases from the tags, I will use `git log` to find the release commits and summarize the changes between them.

## How to investigate undocumented features or settings

When asked to explain a feature that is not well-documented (e.g., experimental flags, alpha settings), I will use the following method:

1.  **Start with the documentation:** I will first read the `README.md` file and any relevant documentation in the `docs/` directory to find information about the feature.
2.  **Formulate a hypothesis:** If the documentation is insufficient, I will formulate a hypothesis about the feature's purpose based on its name and any available context.
3.  **Search the code:** I will use the `search_file_content` tool to find where the feature is defined and used in the source code. The code is the ultimate source of truth.
4.  **Analyze the code:** I will read the relevant code snippets to understand the feature's implementation and how it interacts with other parts of the system.
5.  **Synthesize the findings:** I will combine the information from the documentation (if any) and the code to provide a comprehensive answer.

## Emergency Rescue Scenario

In an emergency scenario, such as a failed Gemini CLI upgrade, it is possible to use a working instance of the Gemini CLI to repair the non-functional one. This setup typically involves running the working CLI on a PC (e.g., Ubuntu) and accessing the target CLI's source code, which is mapped as a shared directory. This is a failsafe procedure and not the standard mode of operation. Under normal circumstances, the Gemini CLI is self-contained and can also be run directly in environments like Termux without needing a separate PC.