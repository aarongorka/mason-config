# Mason Registry

Custom [mason.nvim](https://github.com/mason-org/mason.nvim) registry for LSP servers (well, server) not yet in the [official registry](https://github.com/mason-org/mason-registry).

## ESPHome

![Autocomplete example](completion.png)

If you're using [lazy.nvim](https://lazy.folke.io/), you can configure mason.nvim like so:

```lua
  {
    "mason-org/mason.nvim",
    opts = {
      registries = {
        "github:aarongorka/mason-registry",
        "github:mason-org/mason-registry",
      },
    },
  },

```

Once properly configured, call `:MasonUpdate` to refresh registries registered with Mason.

Then, you can configure the LSP using the native neovim LSP client:

```lua
vim.lsp.config("esphome_lsp",
  ---@type vim.lsp.Config
  {
    cmd = { 'esphome-lsp', '--stdio' },
    filetypes = { 'yaml' },
    root_markers = { '.esphome' },
    settings = {
      esphome = {
        -- See https://github.com/esphome/esphome-vscode/blob/dev/package.json for config reference
        validator = "dashboard" -- or "local"
        dashboardUri = "http://homeassistant:6052/" -- NOTE: works with username:password too like "https://myusername:mypassword@homeassistant:6052"
      },
    },
  }
)
```

## Caveats

  * `platform:` completion doesn't work when you're e.g. adding a sensor
  * ???

---

Big thanks to [bnwa](https://github.com/bnwa/mason-registry/) for inspiration and for hosting fish_lsp config until it got upstreamed.
