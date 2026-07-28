# nvim-peek

Preview LSP definitions, implementations, references, and custom location
methods in Neovim's preview window without leaving the current buffer.

Multiple locations are kept in one preview session and can be selected from
the winbar or with the `:Peek +1` and `:Peek -1` commands.

<img width="1244" height="756" alt="peek" src="https://github.com/user-attachments/assets/3a38e148-ada5-4d05-bfbf-ff3fe35bfd93" />

## Installation

```lua
vim.pack.add({ "https://github.com/gh-liu/nvim-peek" })
```

The plugin registers the buffer-local `:Peek` command when an LSP client
attaches. The Lua module is loaded lazily on the first `LspAttach` event.
## Commands

```vim
:Peek                    " Preview the definition
:Peek def                " Preview definitions
:Peek impl               " Preview implementations
:Peek ref                " Preview references
:Peek! def               " Preview and focus the preview window
:Peek +1                 " Select the next location
:Peek -1                 " Select the previous location
```

Any LSP method that returns `Location`, `Location[]`, `LocationLink`, or
`LocationLink[]` can be passed to `:Peek`.
## Custom mappings

Add buffer-local mappings when an LSP client attaches:

```lua
vim.api.nvim_create_autocmd("LspAttach", {
	callback = function(args)
		local peek = require("peek")

		vim.keymap.set("n", "grp", peek.peek_definition, { buffer = args.buf })
		-- vim.keymap.set("n", "gri", peek.peek_implementation, { buffer = args.buf })
		-- vim.keymap.set("n", "grr", peek.peek_references, { buffer = args.buf })
		--
		-- vim.keymap.set("n", "grt", function()
		-- 	peek.peek("textDocument/typeDefinition", {
		-- 		title = "Type Definition",
		-- 		focus = true,
		-- 	})
		-- end, { buffer = args.buf })
	end,
})
```
## Floating preview

Neovim commit
[`f95bd73935`](https://github.com/neovim/neovim/commit/f95bd73935) added the `'previewpopup'` option.
On a Neovim build that includes this commit, set the
option to make the preview window opened by nvim-peek a floating window:

```lua
vim.o.previewpopup = "height:20,width:80,border:rounded"
```

`height` and `width` can be omitted to derive them from the content. When
`border` is omitted, Neovim uses `'winborder'`.

