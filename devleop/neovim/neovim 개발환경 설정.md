### `nerd-fonts` 설치

[nerd-fonts github](https://github.com/ryanoasis/nerd-fonts)

### 플러그인 매니저 (`lazyvim`) 설치 & initalize

[lazyvim documentation](http://www.lazyvim.org/)
[lazyvim github](https://github.com/LazyVim/LazyVim)

**`File structure`**

```
~/.config/nvim
├── lua
│   ├── config
│   │   ├── autocmds.lua
│   │   ├── keymaps.lua
│   │   ├── lazy.lua
│   │   └── options.lua
│   └── plugins
│       ├── spec1.lua
│       ├── **
│       └── spec2.lua
└── init.toml (또는 init.lua)
```

파일 structure 에 맞춰서 파일을 생성하게 되면 lazyvim이 적용해 주는 구조
`mkdir -p lua/{config,plugins}` 로 `lua/config` `lua/plugins` 생성

**`lazyvim` 추가**
`touch lua/config/lazy.lua` 로 `lazyvim` config 파일 생성

[lazyvim starter github](https://github.com/LazyVim/starter/blob/main/lua/config/lazy.lua)

```lua
-- lua/config/lazy.lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"

if not vim.loop.fs_stat(lazypath) then
  -- bootstrap lazy.nvim
  -- stylua: ignore
  vim.fn.system({ "git", "clone", "--filter=blob:none", "https://github.com/folke/lazy.nvim.git", "--branch=stable", lazypath })
end
vim.opt.rtp:prepend(vim.env.LAZY or lazypath)

require("lazy").setup({
  spec = {
    { "LazyVim/LazyVim", import = "lazyvim.plugins" },
    { import = "plugins" },
  },
  defaults = {
    lazy = false,
    version = false,
  },
  checker = { enabled = true }
})
```

``` lua
-- init.lua

require("config.lazy")
```

**tabsize 조정**
`touch lua/config/options.lua`로 파일 생성

``` lua
-- lua/config/options.lua
vim.opt.expandtab = true
vim.opt.shiftwidth = 2
vim.opt.tabstop = 2
vim.opt.softtabstop = 2
```

`init.lua` 파일에 `config.options.lua` import
``` lua
-- init.lua
require("config.options")
```

**`clipboard`로 카피 단축키 지정**
기존 visual mode 에서 선택 한 뒤 `+"y`를 입력해서 클립보드로 복사하는 기능을 `shift + ctrl + v` 키로 지정하는 방법
`touch lua/config/keymaps.lua` 로 파일 생성

```lua
-- lua/config/keymaps.lua
vim.api.nvim_set_keymap("v", "<C-S-c>", '"+y', { noremap = true })
```

``` lua
-- init.lua
require("config.keymaps")
```

**`catppuccin` colorscheme 추가**
`touch lua/plugins/colorscheme.lua`로 파일 생성

``` lua
-- lua/plugins/colorscheme.lua
return {
  {
    "catppuccin/nvim",
    lazy = true,
    name = "catppuccin",
    config = function()
      require("catppuccin").setup({
        flavour = "frappe",
      })
    end,
    opts = {
      integrations = {
        aerial = true,
        alpha = true,
        cmp = true,
        dashboard = true,
        flash = true,
        gitsigns = true,
        headlines = true,
        illuminate = true,
        indent_blankline = { enabled = true },
        leap = true,
        lsp_trouble = true,
        mason = true,
        markdown = true,
        mini = true,
        native_lsp = {
          enabled = true,
          underlines = {
            errors = { "undercurl" },
            hints = { "undercurl" },
            warnings = { "undercurl" },
            information = { "undercurl" },
          },
        },
        navic = { enabled = true, custom_bg = "lualine" },
        neotest = true,
        neotree = true,
        noice = true,
        notify = true,
        semantic_tokens = true,
        telescope = true,
        treesitter = true,
        treesitter_context = true,
        which_key = true,
      },
    },
  },
  {
    "LazyVim/LazyVim",
    opts = {
      colorscheme = "catppuccin",
    },
  },
}
```

`neotree` 설치
[neo-tree.nvim](https://github.com/nvim-neo-tree/neo-tree.nvim)
`touch lua/plugins/neo-tree.lua`로 파일 생성

``` lua
-- lua/plugins/neo-tree.lua
return {
  "nvim-neo-tree/neo-tree.nvim",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons",
    "MunifTanjim/nui.nvim",
  },
  config = function()
    require("neo-tree").setup({
      window = {
        width = 30,
      },
      filesystem = {
        filtered_items = {
          visible = true,
          hide_dotfile = false,
          hide_gitignored = false,
          hide_hidden = false,
        },
      },
    })
  end,
}
```

`keymaps` 에 nerd-tree 관련 키 바인딩 적용
``` lua
-- lua/config/keymaps.lua

-- more keybindings ...

vim.keymap.set("n", "<C-n>", ":Neotree toggle<CR>", { noremap = true })
```

### `lsp`(nvim-lspconfig) `auto completion`(nvim-cmp) `code-formatting`(formatter.nvim) 설정

**`nvim-lspconfig`을 이용한 `lsp` 설정**
`lsp`를 설치 & 관리 해 주는 `mason` 플러그인 설정

`touch lua/plugins/lsp-config.lua`로 파일 생성 (또는 `nvim-tree` 에서 `a` 키로 생성 가능)

``` lua
-- lua/config/plugins/lsp-config.lua
return {
  "williamboman/mason.nvim",
  config = function()
    require("mason").setup()
  end,
}
```

`config.keymaps` 에 `mason` 열어주는 단축키 생성
``` lua
-- lua/config/keymaps.lua

-- mason open
vim.keymap.set("n", "<C-m>", ":Mason<CR>", { noremap = true })
```

`mason-lspconfig`으로 lsp 설치 자동화
``` lua
-- lua/config/plugins/lsp-config.lua
return {
  {
    "williamboman/mason.nvim",
    config = function()
      require("mason").setup()
    end,
  },
  {
    "williamboman/mason-lspconfig.nvim",
    config = function()
      require("mason").setup({
        ensure_installed = {
          "lua-ls",
        },
      })
    end,
  }
}
```

[LSP server 목록](https://github.com/williamboman/mason-lspconfig.nvim?tab=readme-ov-file#available-lsp-servers)

**`nvim-lspconfig` 설정**
``` lua
-- lua/config/plugins/lsp-config.lua
return {
  {
    "williamboman/mason.nvim",
    config = function()
      require("mason").setup()
    end,
  },
  {
    "williamboman/mason-lspconfig.nvim",
    config = function()
      require("mason").setup({
        ensure_installed = {
          "lua-ls",
        },
      })
    end,
  },
  {
    "neovim/nvim-lspconfig",
    lazy = false,
    config = function()
      local lspconfig = require("lspconfig")
      
    end,
  }
}
```


auto-completions 설정 (`nvim-cmp`)
`touch lua/plugins/cmp.lua` 로 파일 생성
[nvim-cmp github](https://github.com/hrsh7th/nvim-cmp)

``` lua
-- lua/plugins/cmp.lua
return {
  {
    "L3MON4D3/LuaSnip",
    dependencies = {
      "saadparwaiz1/cmp_luasnip",
      "rafamadriz/friendly-snippets",
    },
  },
  {
    "hrsh7th/nvim-cmp",
    dependencies = {
      "hrsh7th/cmp-nvim-lsp",
      "hrsh7th/cmp-buffer",
      "hrsh7th/cmp-path",
      "hrsh7th/cmp-cmdline",
    },
    config = function()
      local cmp = require("cmp")
      require("luasnip.loaders.from_vscode").lazy_load()

      cmp.setup({
        snippet = {
          expand = function(args)
            require("luasnip").lsp_expand(args.body)
          end,
        },
        window = {
          completion = cmp.config.window.bordered(),
          documentation = cmp.config.window.bordered(),
        },
        mapping = cmp.mapping.preset.insert({
          ["<C-b>"] = cmp.mapping.scroll_docs(-4),
          ["<C-f>"] = cmp.mapping.scroll_docs(4),
          ["<C-Space>"] = cmp.mapping.complete(),
          ["<C-e>"] = cmp.mapping.abort(),
          ["<CR>"] = cmp.mapping.confirm({ select = true }),
        }),
        sources = cmp.config.sources({
          { name = "nvim_lsp" },
          { name = "luasnip" },
        }, {
          { name = "buffer" },
        }),
      })
    end,
  },
}
```

**`formatter.nvim` 설정**
`touch lua/plugins/formatter.lua`로 파일 생성

``` lua
-- lua/plugins/formatter.lua
return {
  {
    "mhartington/formatter.nvim",
    config = function()
      require("formatter").setup({
        filetype = {
          lua = {
            require("formatter.filetypes.lua").stylua,
          },
        },
      })
    end,
  },
  {
    "gpanders/editorconfig.nvim",
  },
}
```

이후 `mason` 에서 stylelua formatter 설치

keymap 설정 & format on save 설정
```lua
-- lua/config/keymaps.lua

-- format
vim.keymap.set("n", "<C-f>", ":Format<CR>", { noremap = true })
vim.keymap.set("n", "<C-S-f>", ":FormatWrite<CR>", { noremap = true })

----------------------------------------------------------------------------
-- lua/config/autocmds.lua
local augroup = vim.api.nvim_create_augroup
local autocmd = vim.api.nvim_create_autocmd

augroup("__formatter__", { clear = true })
autocmd("BufWritePost", {
	group = "__formatter__",
	command = ":FormatWrite",
})

---------------------------------------------------------------------------
-- init.lua
require("config.autocmds")
```

**`treesitter` (syntax 하이라이팅 등 구문 분석 도와줌) 설정**
```lua
-- lua/plugins/treesitter.lua
return {
	"nvim-treesitter/nvim-treesitter",
	config = function()
		local config = require("nvim-treesitter.configs")
		config.setup({
			ensure_installed = { "lua" },
			auto_install = true,
			highlight = {
				enable = true,
			},
		})
	end,
}
```

### utility tool 설치 (`lazygit`, `toggleterm`, `colorizer`)

**`lazygit` 설치**
```bash
# gentoo
sudo eselect repository enable guru
sudo emaint sync -r guru
sudo emerge dev-vcs/lazygit

# mac
brew install lazygit
```

`lazygit.nvim` 설정
```lua
-- lua/plugins/utils.lua
return {
	"kdheepak/lazygit.nvim",
	dependencies = {
		"nvim-lua/plenary.nvim",
	},
}
```

**`toggleterm 설정`**
```lua
-- lua/plugins/utils.lua
return {
	{
		"kdheepak/lazygit.nvim",
		dependencies = {
			"nvim-lua/plenary.nvim",
		},
	},
	{
		"akinsho/toggleterm.nvim",
		config = function()
			require("toggleterm").setup({
				size = 20,
				open_mapping = [[<c-\>]],
				hide_numbers = true,
				shade_filetypes = {},
				shade_terminals = true,
				shading_factor = 2,
				start_in_insert = true,
				insert_mappings = true,
				persist_size = true,
				direction = "float",
				close_on_exit = true,
				shell = vim.o.shell,
				float_opts = {
					border = "curved",
					winblend = 0,
					highlights = {
						border = "normal",
						background = "normal",
					},
				},
			})
		end,
	},
}
```

**`colorizer` 설정**
```lua
-- lua/plugins/utils.lua
return {
	{
		"kdheepak/lazygit.nvim",
		dependencies = {
			"nvim-lua/plenary.nvim",
		},
	},
	{
		"NvChad/nvim-colorizer.lua",
		config = function()
        	---@diagnostic disable-next-line: missing-parameter
			require("colorizer").setup()
		end,
	},
	{
		"akinsho/toggleterm.nvim",
		config = function()
			require("toggleterm").setup({
				size = 20,
				open_mapping = [[<c-\>]],
				hide_numbers = true,
				shade_filetypes = {},
				shade_terminals = true,
				shading_factor = 2,
				start_in_insert = true,
				insert_mappings = true,
				persist_size = true,
				direction = "float",
				close_on_exit = true,
				shell = vim.o.shell,
				float_opts = {
					border = "curved",
					winblend = 0,
					highlights = {
						border = "normal",
						background = "normal",
					},
				},
			})
		end,
	},
}
```