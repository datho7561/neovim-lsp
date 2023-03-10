# neovim-lsp
A config for neovim to let you use it as an LSP client without much hassle.

## Prerequisites
1. For now this will only work on Linux or macOS
2. `git`: This is required for plugin management
3. A recent version of neovim, such as `v0.8.0` or newer
4. Basic knowledge of navigating and editing in vim/neovim is needed to use the LSP features effectively
5. *OPTIONAL* you might need to `export TERM=xterm-256color` in `~/.bashrc` (or equivalent for other shells) in order for the colour theme to work

## Installation
1. Download `init.vim` and place it in `~/.config/nvim/init.vim`
2. Open neovim using `nvim`. You will see some scripts running, and there will be errors.
   Don't worry, this is just the plugins needed for LSP being installed.
3. Once plugin installation is done, exit neovim with `:qa!`
4. Reopen neovim. If everything has gone well, a colour theme should be applied.

## Usage
1. Set up the language server you want to use/test. See [Setting up a Language Server](#setting-up-a-language-server).
2. Open a file that you want to test the language server in.
3. Diagnostics should be working out of the box.
4. Completion should open when you are in insert mode in the places you expect.
   You also should be able to manually trigger it with `<ctrl>+<space>`.
   You can select which item you want to apply with the up/down arrow keys and apply with `<enter>`.
5. When you are in Normal mode, the following key bindings are available:
- `<space>ld`: go to definition of the symbol beneath the cursor
- `<space>lD`: go the type definition of the symbol beneath the cursor
- `<space>lh`: open hover on the symbol beneath the cursor
- `<space>lr`: rename the symbol beneath the cursor
- `<space>lc`: open the dialog to display quick fixes for the problem reported
- `<space>lt`: go to references of the symbol beneath the cursor
- `<space>lf`: format the entire file

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

## Limitations
- neovim doesn't support everything in the LSP specification.
  If a feature requires GUI elements to make sense,
  that feature might not be provided,
  since neovim is a terminal application.
  If a feature is not provided, there might be a third party plugin that adds it.
  You could also take a look into coding it yourself in lua,
  since neovim exposes the API to handle sending and receiving requests from the server.
  - If you figure out how to enable additonal LSP features, I will accept PRs for it.
- This configuration of neovim is very bare-bones.
  If you want to use neovim regularly, I'd highly recommend installing plugins for the following:
  - A fuzzy file picker and fuzzy finder (analogous to VS Code's `Ctrl+P` and `Ctrl+Shift+F`), such as [fzf.vim](https://github.com/junegunn/fzf.vim)
  - A nicer status line, such as [lightline](https://github.com/itchyny/lightline.vim)
  - Something that lists all the currently open files, such as [lightlight-bufferline](https://github.com/mengelbrecht/lightline-bufferline)
  - A git line change indicator, such as [gitgutter](https://github.com/airblade/vim-gitgutter)

  I'd also recommend creating normal mode keyboard shortcuts for anything you do regularly in neovim,
  using the following convention:
  ```
  <leader>na
  |       ||
  |       |> the letter representing the action to perform
  |       > the "namespace" of the keybinding, so that you can group similar keybindings together
  > the leader: in the init.vim I set it to <space>

  examples:
  <leader>sf --> [s]earch [f]ile, open the file finder
  <leader>sl --> [s]earch [l]ine, open the grep tool
  ```
