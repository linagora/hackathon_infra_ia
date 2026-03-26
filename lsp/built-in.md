# Built-in LSP Servers

OpenCode includes 37 built-in LSP servers. Most are auto-installed on first use. Set `OPENCODE_DISABLE_LSP_DOWNLOAD` to prevent auto-downloads.

## Auto-installed

These servers are automatically downloaded when OpenCode detects matching files.

| Language      | Server                     | Command                                    | Extensions                                                   |
| ------------- | -------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| TypeScript/JS | typescript-language-server | `bun x typescript-language-server --stdio` | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs`, `.mts`, `.cts` |
| Go            | gopls                      | `gopls`                                    | `.go`                                                        |
| Python        | pyright                    | `pyright-langserver --stdio`               | `.py`, `.pyi`                                                |
| Rust          | rust-analyzer              | `rust-analyzer`                            | `.rs`                                                        |
| C/C++         | clangd                     | `clangd --background-index --clang-tidy`   | `.c`, `.cpp`, `.cc`, `.h`, `.hpp`                            |
| Java          | Eclipse JDTLS              | `java -jar <launcher.jar> ...`             | `.java`                                                      |
| Kotlin        | kotlin-lsp                 | `kotlin-lsp.sh --stdio`                    | `.kt`, `.kts`                                                |
| C#            | csharp-ls                  | `csharp-ls`                                | `.cs`                                                        |
| F#            | fsautocomplete             | `fsautocomplete`                           | `.fs`, `.fsi`, `.fsx`                                        |
| Ruby          | rubocop                    | `rubocop --lsp`                            | `.rb`, `.rake`, `.gemspec`, `.ru`                            |
| PHP           | intelephense               | `intelephense --stdio`                     | `.php`                                                       |
| Elixir        | elixir-ls                  | `language_server.sh`                       | `.ex`, `.exs`                                                |
| Zig           | zls                        | `zls`                                      | `.zig`, `.zon`                                               |
| Lua           | lua-language-server        | `lua-language-server`                      | `.lua`                                                       |
| Bash/Shell    | bash-language-server       | `bash-language-server start`               | `.sh`, `.bash`, `.zsh`, `.ksh`                               |
| Vue           | @vue/language-server       | `vue-language-server --stdio`              | `.vue`                                                       |
| Svelte        | svelte-language-server     | `svelteserver --stdio`                     | `.svelte`                                                    |
| Astro         | @astrojs/language-server   | `astro-ls --stdio`                         | `.astro`                                                     |
| YAML          | yaml-language-server       | `yaml-language-server --stdio`             | `.yaml`, `.yml`                                              |
| Terraform     | terraform-ls               | `terraform-ls serve`                       | `.tf`, `.tfvars`                                             |
| Dockerfile    | docker-langserver          | `docker-langserver --stdio`                | `.dockerfile`, `Dockerfile`                                  |
| LaTeX         | texlab                     | `texlab`                                   | `.tex`, `.bib`                                               |
| Typst         | tinymist                   | `tinymist`                                 | `.typ`, `.typc`                                              |

## Linters (auto-installed)

| Linter | Server        | Command                    | Extensions                                           |
| ------ | ------------- | -------------------------- | ---------------------------------------------------- |
| ESLint | vscode-eslint | `bun <serverPath> --stdio` | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs`, `.vue` |

## Requires Manual Install

These servers require the tool to be already available in your PATH or project.

| Language | Server                          | Command                                        | Extensions                            |
| -------- | ------------------------------- | ---------------------------------------------- | ------------------------------------- |
| Deno     | deno lsp                        | `deno lsp`                                     | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`  |
| Swift    | sourcekit-lsp                   | `sourcekit-lsp`                                | `.swift`                              |
| Dart     | dart language-server            | `dart language-server --lsp`                   | `.dart`                               |
| OCaml    | ocamllsp                        | `ocamllsp`                                     | `.ml`, `.mli`                         |
| Haskell  | haskell-language-server-wrapper | `haskell-language-server-wrapper --lsp`        | `.hs`, `.lhs`                         |
| Julia    | LanguageServer.jl               | `julia -e "using LanguageServer; runserver()"` | `.jl`                                 |
| Clojure  | clojure-lsp                     | `clojure-lsp listen`                           | `.clj`, `.cljs`, `.cljc`, `.edn`      |
| Gleam    | gleam lsp                       | `gleam lsp`                                    | `.gleam`                              |
| Nix      | nixd                            | `nixd`                                         | `.nix`                                |
| Prisma   | prisma                          | `prisma language-server`                       | `.prisma`                             |
| Oxlint   | oxc_language_server             | `oxlint --lsp`                                 | `.ts`, `.tsx`, `.js`, `.jsx`, `.vue`  |
| Biome    | biome                           | `biome lsp-proxy --stdio`                      | `.ts`, `.js`, `.json`, `.css`, `.vue` |

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
