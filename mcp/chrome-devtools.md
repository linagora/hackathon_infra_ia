# Chrome DevTools MCP

[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp) enables AI agents to control and inspect live Chrome instances via MCP. Built on Puppeteer.

## Installation

```json
{
  "mcp": {
    "chrome-devtools": {
      "type": "local",
      "command": ["npx", "-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

Chrome starts automatically when a tool is invoked.

### Connect to an existing Chrome instance

Launch Chrome with remote debugging:

```bash
google-chrome --remote-debugging-port=9222
```

Then add `--browserUrl`:

```json
{
  "mcp": {
    "chrome-devtools": {
      "type": "local",
      "command": ["npx", "-y", "chrome-devtools-mcp@latest", "--browserUrl", "http://localhost:9222"]
    }
  }
}
```

### Configuration Flags

| Flag               | Description                                   |
| ------------------ | --------------------------------------------- |
| `--browserUrl`     | Connect to a running debuggable Chrome        |
| `--wsEndpoint`     | WebSocket endpoint for Chrome connection      |
| `--headless`       | Run Chrome without UI                         |
| `--isolated`       | Temporary user-data-dir, auto-cleaned on exit |
| `--executablePath` | Custom Chrome executable path                 |
| `--viewport`       | Initial viewport size (e.g., `1280x720`)      |
| `--slim`           | Expose only 3 core tools (for basic tasks)    |

## Tools (29)

### Input Automation

| Tool            | Description                            |
| --------------- | -------------------------------------- |
| `click`         | Click an element by uid                |
| `drag`          | Drag element onto another              |
| `fill`          | Fill input/textarea/select             |
| `fill_form`     | Fill multiple form elements at once    |
| `handle_dialog` | Handle browser dialogs (alert/confirm) |
| `hover`         | Hover over an element                  |
| `press_key`     | Press key or key combination           |
| `type_text`     | Type text into focused input           |
| `upload_file`   | Upload a file through a file input     |

### Navigation

| Tool            | Description                            |
| --------------- | -------------------------------------- |
| `navigate_page` | Navigate by URL, back, forward, reload |
| `new_page`      | Open a new tab                         |
| `list_pages`    | List all open pages                    |
| `select_page`   | Select a page as active context        |
| `close_page`    | Close a page by ID                     |
| `wait_for`      | Wait for text to appear on page        |

### Inspection & Debugging

| Tool                    | Description                                  |
| ----------------------- | -------------------------------------------- |
| `take_screenshot`       | Screenshot of page or specific element       |
| `take_snapshot`         | Text snapshot based on accessibility tree    |
| `evaluate_script`       | Execute JavaScript in the page               |
| `list_console_messages` | List console messages                        |
| `get_console_message`   | Get a specific console message by ID         |
| `lighthouse_audit`      | Lighthouse audit (a11y, SEO, best practices) |

### Network

| Tool                    | Description               |
| ----------------------- | ------------------------- |
| `list_network_requests` | List all network requests |
| `get_network_request`   | Get details of a request  |

### Emulation

| Tool          | Description                                       |
| ------------- | ------------------------------------------------- |
| `emulate`     | Emulate device (dark mode, throttling, geo, etc.) |
| `resize_page` | Resize the page window                            |

### Performance

| Tool                          | Description                         |
| ----------------------------- | ----------------------------------- |
| `performance_start_trace`     | Start a performance trace recording |
| `performance_stop_trace`      | Stop the active trace recording     |
| `performance_analyze_insight` | Drill into a performance insight    |
| `take_memory_snapshot`        | Capture a heap snapshot             |

## Usage Examples

**Screenshot and inspect:**
> "Take a screenshot of https://example.com and list network requests"

**Form automation:**
> "Go to the login page, fill in the form, then click submit"

**Performance audit:**
> "Check the performance of https://mysite.com"

**Lighthouse accessibility audit:**
> "Run a Lighthouse a11y audit on https://mysite.com on mobile"

**Emulate mobile:**
> "Emulate 375x812 viewport with dark mode and slow 3G"
