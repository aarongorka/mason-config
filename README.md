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
    -- `root_dir` ensures that the LSP does not attach to all yaml files
    root_dir = function(bufnr, on_dir)
      local fname = vim.api.nvim_buf_get_name(bufnr)
      if vim.fs.find('.esphome', { path = fname, upward = true })[1] then
        on_dir(require("lspconfig.util").root_pattern('.esphome')(fname))
      end
    end,
    settings = {
      esphome = {
        validator = "dashboard", -- or "local"
        dashboardUri = vim.env.ESPHOME_URL -- I have credentials in my URI ;)
      },
    },
  }
)
```

Note that you may have to modify the `root_dir` function if you don't have a `.esphome/` directory where you're storing your device YAML files.

## Caveats

  * Some platforms (like `switch`) don't work
  * GPIO validation doesn't work
  * It's pretty slow to start
  * Seems to dump files in `~/.esphome-language-server/`
  * ???

---

Big thanks to [bnwa](https://github.com/bnwa/mason-registry/) for inspiration and for hosting fish_lsp config until it got upstreamed.
