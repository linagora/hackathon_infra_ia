# Built-in LSP Servers

OpenCode includes 37 built-in LSP servers. Set `OPENCODE_DISABLE_LSP_DOWNLOAD` to prevent auto-downloads.

## Auto-installed via npm (no prerequisite)

Downloaded via `bun install` on first use. No system dependency required.

| Language   | Server                   | Extensions                     |
| ---------- | ------------------------ | ------------------------------ |
| Python     | pyright                  | `.py`, `.pyi`                  |
| Vue        | @vue/language-server     | `.vue`                         |
| Svelte     | svelte-language-server   | `.svelte`                      |
| Astro      | @astrojs/language-server | `.astro`                       |
| YAML       | yaml-language-server     | `.yaml`, `.yml`                |
| PHP        | intelephense             | `.php`                         |
| Bash/Shell | bash-language-server     | `.sh`, `.bash`, `.zsh`, `.ksh` |
| Dockerfile | docker-langserver        | `.dockerfile`, `Dockerfile`    |

## Auto-installed binary download (no prerequisite)

Prebuilt binary downloaded from GitHub/vendor releases. No runtime needed.

| Language  | Server              | Download source                       |
| --------- | ------------------- | ------------------------------------- |
| C/C++     | clangd              | GitHub releases                       |
| Zig       | zls                 | GitHub releases (needs `zig` in PATH) |
| Lua       | lua-language-server | GitHub releases                       |
| Terraform | terraform-ls        | HashiCorp releases                    |
| LaTeX     | texlab              | GitHub releases                       |
| Typst     | tinymist            | GitHub releases                       |
| Kotlin    | kotlin-lsp          | JetBrains CDN                         |

## Auto-installed via language package manager (requires runtime)

OpenCode runs the install command, but the language runtime must already be present.

| Language | Server         | Install command                              | Requires       |
| -------- | -------------- | -------------------------------------------- | -------------- |
| Go       | gopls          | `go install golang.org/x/tools/gopls@latest` | `go` in PATH   |
| Ruby     | rubocop        | `gem install rubocop`                        | `ruby` + `gem` |
| C#       | csharp-ls      | `dotnet tool install csharp-ls`              | .NET SDK       |
| F#       | fsautocomplete | `dotnet tool install fsautocomplete`         | .NET SDK       |

## Auto-download + build from source (requires runtime)

Downloaded and compiled on first use. Build tools must be present.

| Language | Server    | What happens                                | Requires                    |
| -------- | --------- | ------------------------------------------- | --------------------------- |
| JS/TS    | ESLint    | Downloads vscode-eslint, runs `npm compile` | `npm` + `eslint` in project |
| Elixir   | elixir-ls | Downloads source, runs `mix compile`        | `elixir` + `mix`            |
| Java     | JDTLS     | Downloads Eclipse JDT tar.gz                | Java >= 21                  |

## Manual install required (no auto-download)

OpenCode only checks if the binary is in PATH. You must install it yourself.

| Language      | Server                          | Command                                        | Extensions                            |
| ------------- | ------------------------------- | ---------------------------------------------- | ------------------------------------- |
| TypeScript/JS | typescript-language-server      | `bun x typescript-language-server --stdio`     | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`  |
| Rust          | rust-analyzer                   | `rust-analyzer`                                | `.rs`                                 |
| Deno          | deno lsp                        | `deno lsp`                                     | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`  |
| Swift         | sourcekit-lsp                   | `sourcekit-lsp`                                | `.swift`                              |
| Dart          | dart language-server            | `dart language-server --lsp`                   | `.dart`                               |
| OCaml         | ocamllsp                        | `ocamllsp`                                     | `.ml`, `.mli`                         |
| Haskell       | haskell-language-server-wrapper | `haskell-language-server-wrapper --lsp`        | `.hs`, `.lhs`                         |
| Julia         | LanguageServer.jl               | `julia -e "using LanguageServer; runserver()"` | `.jl`                                 |
| Clojure       | clojure-lsp                     | `clojure-lsp listen`                           | `.clj`, `.cljs`, `.cljc`, `.edn`      |
| Gleam         | gleam lsp                       | `gleam lsp`                                    | `.gleam`                              |
| Nix           | nixd                            | `nixd`                                         | `.nix`                                |
| Prisma        | prisma                          | `prisma language-server`                       | `.prisma`                             |
| Oxlint        | oxc_language_server             | `oxlint --lsp`                                 | `.ts`, `.tsx`, `.js`, `.jsx`, `.vue`  |
| Biome         | biome                           | `biome lsp-proxy --stdio`                      | `.ts`, `.js`, `.json`, `.css`, `.vue` |

> **TypeScript note**: `typescript-language-server` is fetched on-the-fly by bun, but `typescript` must be in your project's `node_modules`.

## Experimental

| Language | Server | Flag                           | Note             |
| -------- | ------ | ------------------------------ | ---------------- |
| Python   | ty     | `OPENCODE_EXPERIMENTAL_LSP_TY` | Replaces pyright |

## Custom LSP Server

Add a custom LSP server in `opencode.json`:

```json
{
  "lsp": {
    "my-language": {
      "command": ["my-lsp-server", "--stdio"],
      "extensions": [".myext"],
      "env": { "KEY": "value" }
    }
  }
}
```

| Key              | Type                  | Required | Description                      |
| ---------------- | --------------------- | -------- | -------------------------------- |
| `command`        | string[]              | yes      | Command and arguments (as array) |
| `extensions`     | string[]              | yes      | File extensions to associate     |
| `disabled`       | boolean               | no       | Disable this LSP                 |
| `env`            | Record<string,string> | no       | Environment variables            |
| `initialization` | object                | no       | Initialization options           |

## Disable LSP

```json
{
  "lsp": false
}
```

Or disable a specific built-in server:

```json
{
  "lsp": {
    "typescript": { "disabled": true }
  }
}
```

## See Also

- [configuration.md](../config/configuration.md) -- LSP section in config reference
