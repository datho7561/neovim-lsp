# neovim-lsp
A config for neovim to let you use it as an LSP client without much hassle.

## Prerequisites
1. `git`: This is required for plugin management
2. A recent version of neovim, such as `v0.8.0` or newer
3. Basic knowledge of navigating and editing in vim/neovim is needed to use the LSP features effectively

## Installation
1. Download `init.vim` and place it in `~/.config/nvim/init.vim`
2. Open neovim using `nvim`. You might see some scripts running in the background, and there will be errors. Don't worry.
3. Type `:PlugInstall`, then hit `Enter`
4. Close neovim with `:q!`
5. Reopen neovim. If everything has gone well, a colour theme should be applied.

## Usage
1. Set up the language server you want to use/test. See [Setting up a Language Server](#setting-up-a-language-server).
2. Open a file that you want to test the language server in.
3. When you are in Normal mode, the following key bindings are available:


## Setting up a Language Server
1. Go to the [LSP config documentation](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md),
and search for the language server that you want to set up.
2. Follow the instructions to install the language server locally.
3. Open `~/.config/nvim/init.vim`, you will need to edit it to get the server working.
3. If you don't need to do any configuration for the language server (i.e. you don't need to set any of the settings for the language server),
then just add the name of the language server **As it appears in the LSP config documentation** to the list on the following line:

```lua
local servers = { 'lemminx', -- ...
```

4. If you do need to set specific settings for the language server, then follow the example for `lemminx`:

```lua
lspconfig['lemminx'].setup {
  on_attach = on_attach,
  capabilities = capabilities,
  settings = {
    xml = {
      codeLens = {
        enabled = true
      }
    }
  }
}
```

`settings` is an object literal that works similar to the VS Code settings.

5. Some language servers (such as `eclipse.jdt.ls`) require additional setup or plugins to work well.
If this is the case, the [LSP config documentation](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md)
should have details about this.

**Make sure to close and open neovim to apply the changes**

## Plugin List and Rationale
- `tpope/vim-sensible`
    - Basic configuration for vim that removes stuff leftover from `vi`. Most of these should be already handled in neovim, but including it just in case.
- `rose-pine/neovim`
    - A theme that only works in neovim so that you can tell you aren't in regular vim.
- `editorconfig/editorconfig-vim`
    - editorconfig support, so that neovim knowns when to treat `<Tab>` as spaces.
- `neovim/nvim-lspconfig`
    - provides APIs to easily configure language servers
- `hrsh7th/nvim-cmp`
    - completion UI
- `hrsh7th/cmp-nvim-lsp`
    - configs to get the completion UI to work with LSP completion
- `saadparwaiz1/cmp_luasnip`
    - configs to get snippet-style completion working in the completion UI
- `L3MON4D3/LuaSnip`
    - Convert the LSP snippets into something neovim understands
