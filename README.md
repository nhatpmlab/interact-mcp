# Interactive Feedback MCP

Developed by **Pham Minh Nhat** (`nhatpm.lab`) - [nhatpm.lab@gmail.com](mailto:nhatpm.lab@gmail.com)

## 🔒 Project Attribution

> Maintained by **Pham Minh Nhat** (`nhatpm.lab`)  
> 📧 Email: [nhatpm.lab@gmail.com](mailto:nhatpm.lab@gmail.com)

This project integrates **Model Context Protocol (MCP)** to ensure intelligent and interactive feedback with the user at critical points.

## Prompt Engineering

For the best results, add the following to your custom prompt in your AI assistant, you should add it on a rule or directly in the prompt (e.g., Cursor):

> Whenever you want to ask a question, always call the MCP `interactive_feedback`.  
> Whenever you’re about to complete a user request, call the MCP `interactive_feedback` instead of simply ending the process.
> Keep calling MCP until the user’s feedback is empty, then end the request.

This will ensure your AI assistant uses this MCP server to request user feedback before marking the task as completed.

## 💡 Why Use This?
By guiding the assistant to check in with the user instead of branching out into speculative, high-cost tool calls, this module can drastically reduce the number of premium requests (e.g., OpenAI tool invocations) on platforms like Cursor. In some cases, it helps consolidate what would be up to 25 tool calls into a single, feedback-aware request — saving resources and improving performance.

## Configuration

This MCP server uses Qt's `QSettings` to store configuration on a per-project basis. This includes:
*   The command to run.
*   Whether to execute the command automatically on the next startup for that project (see "Execute automatically on next run" checkbox).
*   The visibility state (shown/hidden) of the command section (this is saved immediately when toggled).
*   Window geometry and state (general UI preferences).

These settings are typically stored in platform-specific locations (e.g., registry on Windows, plist files on macOS, configuration files in `~/.config` or `~/.local/share` on Linux) under an organization name "NhatPM" and application name "InteractiveFeedbackMCP", with a unique group for each project directory.

The "Save Configuration" button in the UI primarily saves the current command typed into the command input field and the state of the "Execute automatically on next run" checkbox for the active project. The visibility of the command section is saved automatically when you toggle it. General window size and position are saved when the application closes.


        ```json
        {
          "mcpServers": {
            "interactive-feedback-mcp": {
              "command": "uv",
              "args": [
                "--directory",
                "/Users/nhatpm/Desktop/interactive-feedback-mcp",
                "run",
                "server.py"
              ],
              "timeout": 600,
              "autoApprove": [
                "interactive_feedback"
              ]
            }
          }
        }
        ```
    *   You might use a server identifier like `interactive-feedback-mcp` when configuring it in Cursor.

### For Cline / Windsurf

Similar setup principles apply. You would configure the server command (e.g., `uv run server.py` with the correct `--directory` argument pointing to the project directory) in the respective tool's MCP settings, using `interactive-feedback-mcp` as the server identifier.

## Development

To run the server in development mode with a web interface for testing:

```sh
uv run fastmcp dev server.py
```

This will open a web interface and allow you to interact with the MCP tools for testing.

## Available tools

Here's an example of how the AI assistant would call the `interactive_feedback` tool:

```xml
<use_mcp_tool>
  <server_name>interactive-feedback-mcp</server_name>
  <tool_name>interactive_feedback</tool_name>
  <arguments>
    {
      "project_directory": "/path/to/your/project",
      "summary": "I've implemented the changes you requested and refactored the main module."
    }
  </arguments>
</use_mcp_tool>
```

## Acknowledgements & Contact

For any questions, suggestions, or if you just want to share how you're using it, feel free to reach out on X!