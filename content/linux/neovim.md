---
title: "neovim"
draft: true
---

## 我的neovim配置
 > https://github.com/hjkl01/init.vim

## 插件
```sh
# status/tabline vim-airline/vim-airline
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
let g:airline#extensions#tabline#formatter = 'default'
let g:airline#extensions#tabline#enabled = 1
" let g:airline#extensions#tabline#left_sep = ' '
" let g:airline#extensions#tabline#left_alt_sep = '|'
" let g:airline#extensions#tabline#enabled = 1
" let g:airline#extensions#tabline#tab_nr_type = 1 " tab number
" let g:airline#extensions#tabline#show_tab_nr = 1
" let g:airline#extensions#tabline#formatter = 'default'
" let g:airline#extensions#tabline#buffer_nr_show = 0
" let g:airline#extensions#tabline#fnametruncate = 16
" let g:airline#extensions#tabline#fnamecollapse = 2
" let g:airline#extensions#tabline#buffer_idx_mode = 1
let g:airline_theme='molokai'

# format file
Plug 'Chiel92/vim-autoformat'

" autocmd BufWrite * :Autoformat
let g:autoformat_autoindent = 1
let g:autoformat_retab = 1
let g:autoformat_remove_trailing_spaces = 1


# 文件目录 
Plug 'preservim/nerdtree'
let g:NERDTreeWinPos = "right"
let NERDTreeShowHidden=1
let NERDTreeShowLineNumbers=1
let NERDTreeIgnore = ['\.pyc$', '__pycache__']
let g:NERDTreeWinSize=35
let g:NERDTreeDirArrows = 1
"当打开vim且没有文件时自动打开NERDTree
" autocmd vimenter * if !argc() | NERDTree | endif
"" 只剩 NERDTree时自动关闭
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif

nmap <Space>n :NERDTreeToggle<CR>
nmap <Space>n <ESC> :NERDTreeToggle<CR>

# nvim-tree.lua
Plug 'kyazdani42/nvim-web-devicons' " for file icons
Plug 'kyazdani42/nvim-tree.lua'
autocmd BufEnter * ++nested if winnr('$') == 1 && bufname() == 'NvimTree_' . tabpagenr() | quit | endif
    nnoremap <Space>n :NvimTreeToggle<CR>

    lua << EOF
    require'nvim-tree'.setup { -- BEGIN_DEFAULT_OPTS
    auto_reload_on_write = true,
    disable_netrw = false,
    hide_root_folder = false,
    hijack_cursor = false,
    hijack_netrw = true,
    hijack_unnamed_buffer_when_opening = false,
    ignore_buffer_on_setup = false,
    open_on_setup = true,
      -- open_on_setup_file = true,
    open_on_tab = true,
    sort_by = "name",
    update_cwd = false,
    view = {
        width = 30,
        height = 30,
        side = "right",
        preserve_window_proportions = false,
        number = true,
        relativenumber = true,
        signcolumn = "yes",
        mappings = {
            custom_only = false,
            list = {
                -- user mappings go here
                },
            },
        },
    renderer = {
        indent_markers = {
        enable = true,
        icons = {
            corner = "└ ",
            edge = "│ ",
            none = "  ",
            },
        },
    },
hijack_directories = {
enable = true,
auto_open = true,
},
      update_focused_file = {
      enable = false,
      update_cwd = false,
      ignore_list = {},
      },
  ignore_ft_on_setup = {},
  system_open = {
      cmd = nil,
      args = {},
      },
  diagnostics = {
  enable = false,
  show_on_dirs = false,
  icons = {
      hint = "",
      info = "",
      warning = "",
      error = "",
      },
  },
      filters = {
          dotfiles = false,
          custom = {},
          exclude = {},
          },
      git = {
      enable = true,
      ignore = true,
      timeout = 400,
      },
  actions = {
      use_system_clipboard = true,
      change_dir = {
      enable = true,
      global = false,
      },
  open_file = {
      quit_on_open = true,
      resize_window = true,
      window_picker = {
      enable = true,
      chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890",
      exclude = {
          filetype = { "notify", "packer", "qf", "diff", "fugitive", "fugitiveblame" },
          buftype = { "nofile", "terminal", "help" },
          },
      },
  },
      },
  trash = {
      cmd = "trash",
      require_confirm = true,
      },
  log = {
  enable = false,
  truncate = false,
  types = {
      all = false,
      config = false,
      copy_paste = false,
      git = false,
      profile = false,
      },
  },
    }
EOF


# 注释
Plug 'preservim/nerdcommenter' " 注释

nmap <Space><Space> <plug>NERDCommenterToggle

" Add spaces after comment delimiters by default
let g:NERDSpaceDelims = 1

" Use compact syntax for prettified multi-line comments
let g:NERDCompactSexyComs = 1

" Align line-wise comment delimiters flush left instead of following code indentation
let g:NERDDefaultAlign = 'left'

" Set a language to use its alternate delimiters by default
let g:NERDAltDelims_java = 1

" Add your own custom formats or override the defaults
" let g:NERDCustomDelimiters = { 'c': { 'left': '/**','right': '*/' } }

" Allow commenting and inverting empty lines (useful when commenting a region)
let g:NERDCommentEmptyLines = 1

" Enable trimming of trailing whitespace when uncommenting
let g:NERDTrimTrailingWhitespace = 1

" Enable NERDCommenterToggle to check all selected lines is commented or not
let g:NERDToggleCheckAllLines = 1



# other
Plug 'jiangmiao/auto-pairs'

Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-telescope/telescope.nvim'
nnoremap <S-f> <cmd>Telescope find_files<cr>

Plug 'mhinz/vim-startify'
Plug 'ntpeters/vim-better-whitespace'
Plug 'pechorin/any-jump.vim'
" Normal mode: Jump to definition under cursor
nnoremap <leader>j :AnyJump<CR>
" Visual mode: jump to selected text in visual mode
xnoremap <leader>j :AnyJumpVisual<CR>
" Normal mode: open previous opened file (after jump)
nnoremap <leader>ab :AnyJumpBack<CR>
" Normal mode: open last closed search window again
nnoremap <leader>al :AnyJumpLastResults<CR>


Plug 'voldikss/vim-floaterm'
nmap <Space>t :FloatermNew<CR>

Plug 'dense-analysis/ale'
let b:ale_linters = ['mypy']
" let b:ale_linters = ['flake8', 'pylint']


Plug 'gelguy/wilder.nvim', { 'do': ':UpdateRemotePlugins' }
call wilder#setup({'modes': [':', '/', '?']})

call wilder#set_option('pipeline', [
            \   wilder#branch(
            \     wilder#cmdline_pipeline(),
            \     wilder#search_pipeline(),
            \   ),
            \ ])

call wilder#set_option('renderer', wilder#wildmenu_renderer({
            \ 'highlighter': wilder#basic_highlighter(),
            \ }))




# code complete: lsp

Plug 'prabirshrestha/vim-lsp'
Plug 'mattn/vim-lsp-settings'

Plug 'prabirshrestha/asyncomplete.vim'
Plug 'prabirshrestha/asyncomplete-lsp.vim'


function! s:on_lsp_buffer_enabled() abort
    setlocal omnifunc=lsp#complete
    setlocal signcolumn=yes
    if exists('+tagfunc') | setlocal tagfunc=lsp#tagfunc | endif
    nmap <buffer> gd <plug>(lsp-definition)
    nmap <buffer> gs <plug>(lsp-document-symbol-search)
    nmap <buffer> gS <plug>(lsp-workspace-symbol-search)
    nmap <buffer> gr <plug>(lsp-references)
    nmap <buffer> gi <plug>(lsp-implementation)
    nmap <buffer> gt <plug>(lsp-type-definition)
    nmap <buffer> <leader>rn <plug>(lsp-rename)
    nmap <buffer> [g <plug>(lsp-previous-diagnostic)
    nmap <buffer> ]g <plug>(lsp-next-diagnostic)
    nmap <buffer> K <plug>(lsp-hover)
    inoremap <buffer> <expr><c-f> lsp#scroll(+4)
    inoremap <buffer> <expr><c-d> lsp#scroll(-4)
    nmap <Space>f <plug>(lsp-document-format)

    let g:lsp_document_highlight_enabled = 1
    let g:lsp_diagnostics_enabled = 1
    let g:lsp_format_sync_timeout = 1000
    autocmd! BufWritePre *.go,*.py call execute('LspDocumentFormatSync')
    " autocmd BufWritePre <buffer> LspDocumentFormatSync

    " refer to doc to add more commands
endfunction


augroup lsp_install
    au!
    " call s:on_lsp_buffer_enabled only for languages that has the server registered.
    autocmd User lsp_buffer_enabled call s:on_lsp_buffer_enabled()
augroup END

Plug 'github/copilot.vim'
" :Copilot setup
```
