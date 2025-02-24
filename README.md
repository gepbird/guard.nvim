# guard.nvim

Async formatting and linting utility for neovim.

## Features

- Blazingly fast
- Async using coroutine and luv spawn
- Builtin support for popular formatters and linters
- Easy configuration for custom tools
- Light-weight

## Usage

Use any plugin manager you like. Guard is configured as follows:

```lua
local ft = require('guard.filetype')

ft('lang'):fmt('format-tool-1')
       :append('format-tool-2')
       :lint('lint-tool-1')
       :append('lint-tool-2')

-- Call setup() LAST!
require('guard').setup({
    -- the only options for the setup function
    fmt_on_save = true,
    -- Use lsp if no formatter was defined for this filetype
    lsp_as_default_formatter = false,
})
```

- Use `GuardFmt` to manually call format, when there is a visual selection only the selection is formatted.
- `GuardDisable` disables auto format for the current buffer, you can also `GuardDisable 16` (the buffer number)
- Use `GuardEnable` to re-enable auto format, usage is the same as `GuardDisable`

### Example Configuration

Format c files with clang-format and lint with clang-tidy:

```lua
ft('c'):fmt('clang-format')
       :lint('clang-tidy')
```

Or use lsp to format go files first, then format with golines, then lint with golangci:

```lua
ft('go'):fmt('lsp')
        :append('golines')
        :lint('golangci')
```

Register multiple filetypes to a single linter or formatter:

```lua
ft('typescript,javascript,typescriptreact'):fmt('prettier')
```

### Custom Configuration

Easily configure your custom formatter if not in the defaults:

```
{
    cmd              -- string: tool command
    args             -- table: command arugments
    fname            -- string: insert filename to args tail
    stdin            -- boolean: pass buffer contents into stdin
    timeout          -- integer
    ignore_pattern   -- table: ignore run format when pattern match
    ignore_error     -- boolean: when has lsp error ignore format
    find             -- string: format if the file is found in the lsp root dir
    env              -- table: environment variables passed to cmd (key value pair)

    --special
    fn       -- function: if fn is set other field will not take effect
}
```

For example, format your assembly with [asmfmt](https://github.com/klauspost/asmfmt):

```lua
ft('asm'):fmt({
    cmd = 'asmfmt',
    stdin = true
})
```

Consult the [builtin tools](https://github.com/nvimdev/guard.nvim/tree/main/lua%2Fguard%2Ftools) if needed.

### Supported Tools

#### Formatters

- `lsp` use `vim.lsp.buf.format`
- [black](https://github.com/psf/black)
- [cbfmt](https://github.com/lukas-reineke/cbfmt)
- [clang-format](https://www.kernel.org/doc/html/latest/process/clang-format.html)
- [djhtml](https://github.com/rtts/djhtml)
- [fish_indent](https://fishshell.com/docs/current/cmds/fish_indent.html)
- [fnlfmt](https://git.sr.ht/~technomancy/fnlfmt)
- [gofmt](https://pkg.go.dev/cmd/gofmt)
- [golines](https://pkg.go.dev/github.com/segmentio/golines)
- [google-java-format](https://github.com/google/google-java-format)
- [isort](https://github.com/PyCQA/isort)
- [mixformat](https://github.com/elixir-lang/elixir/)
- [latexindent](https://github.com/cmhughes/latexindent.pl)
- [pg_format](https://github.com/darold/pgFormatter)
- [prettier](https://github.com/prettier/prettier)
- [prettierd](https://github.com/fsouza/prettierd)
- [rubocop](https://github.com/rubocop/rubocop)
- [rustfmt](https://github.com/rust-lang/rustfmt)
- [shfmt](https://github.com/mvdan/sh)
- [stylua](https://github.com/JohnnyMorganz/StyLua)
- [swiftformat](https://github.com/nicklockwood/SwiftFormat)
- [swift-format](https://github.com/apple/swift-format)
- [sql-formatter](https://github.com/sql-formatter-org/sql-formatter)

#### Linters

- [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
- [hadolint](https://github.com/hadolint/hadolint)
- [luacheck](https://github.com/lunarmodules/luacheck)
- [pylint](https://github.com/PyCQA/pylint)
- [rubocop](https://github.com/rubocop/rubocop)
- [shellcheck](https://github.com/koalaman/shellcheck)

## Troubleshooting

If guard does not auto format on save, run `checkhealth` first.

## License MIT
