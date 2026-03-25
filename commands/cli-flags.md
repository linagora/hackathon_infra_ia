# CLI Flags

OpenCode CLI is built with yargs. The main command is `opencode run [message..]`.

## Global Flags

| Flag           | Short | Description                          |
| -------------- | ----- | ------------------------------------ |
| `--help`       | `-h`  | Show help                            |
| `--version`    | `-v`  | Show version number                  |
| `--print-logs` |       | Print logs to stderr                 |
| `--log-level`  |       | Log level (DEBUG, INFO, WARN, ERROR) |

## Run Flags

| Flag          | Short | Description                                                    |
| ------------- | ----- | -------------------------------------------------------------- |
| `[message..]` |       | Message to send (positional argument)                          |
| `--command`   |       | Command to run, use message for args                           |
| `--continue`  | `-c`  | Continue the last session                                      |
| `--session`   | `-s`  | Session ID to continue                                         |
| `--fork`      |       | Fork session before continuing (requires --continue/--session) |
| `--share`     |       | Share the session                                              |
| `--model`     | `-m`  | Model to use (`provider/model` format)                         |
| `--agent`     |       | Agent to use                                                   |
| `--format`    |       | Output format: `default` (formatted) or `json` (raw events)    |
| `--file`      | `-f`  | File(s) to attach to message                                   |
| `--title`     |       | Title for the session                                          |
| `--attach`    |       | Attach to a running server (e.g., `http://localhost:4096`)     |
| `--password`  | `-p`  | Basic auth password (defaults to `OPENCODE_SERVER_PASSWORD`)   |
| `--dir`       |       | Directory to run in (path on remote server if attaching)       |
| `--port`      |       | Port for the local server                                      |
| `--variant`   |       | Model variant (reasoning effort: high, max, minimal)           |
| `--thinking`  |       | Show thinking blocks                                           |

## Examples

```bash
# Launch in interactive mode
opencode

# Run a single prompt
opencode run "Explain this file"

# Continue last session
opencode run -c "What about the tests?"

# Use a specific model
opencode run -m "provider/model-name" "Fix the bug"

# Attach to a remote server
opencode run --attach http://localhost:4096 --dir /project

# JSON output
opencode run --format json "List all functions"
```
