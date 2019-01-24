---
layout: default
title: cheatsheet_conf
---

# .tmux.conf
```conf
# ~/.tmux.conf
# reload the config: `Ctrl-b: source ~/.tmux.conf`

# switch windows alt+number
bind-key -n M-0 select-window -t 0
bind-key -n M-1 select-window -t 1
bind-key -n M-2 select-window -t 2
bind-key -n M-3 select-window -t 3
bind-key -n M-4 select-window -t 4
bind-key -n M-5 select-window -t 5
bind-key -n M-6 select-window -t 6
bind-key -n M-7 select-window -t 7
bind-key -n M-8 select-window -t 8
bind-key -n M-9 select-window -t 9
bind-key -n M-n next-window
bind-key -n M-p previous-window
# switch pane with alt+arrow
bind-key -n M-Left  select-pane -L
bind-key -n M-Right select-pane -R
bind-key -n M-Up    select-pane -U
bind-key -n M-Down  select-pane -D
# switch pane in vim style
bind-key -n M-h     select-pane -L
bind-key -n M-l     select-pane -R
bind-key -n M-k     select-pane -U
bind-key -n M-j     select-pane -D
```

# .vimrc
```conf
" to reload: `:so %`
set hlsearch
set statusline=%f%=(%{&ff})\ row:%l/%L\ col:%v\ [%p%%]
set laststatus=2
set scrolloff=4
set modeline

syntax on

filetype on
filetype plugin on
filetype indent on

nnoremap <F7>  :ls<CR>:buffer<Space>
nnoremap <F9>  :cs f s<Space>
nnoremap <F10> :cs f g<Space>
nnoremap <C-p> :tabp<CR>
nnoremap <C-n> :tabn<CR>

if has("autocmd")
        autocmd FileType javascript,html,css setlocal ts=4 sts=4 sw=4 expandtab
        autocmd FileType python setlocal ts=4 sts=4 sw=4 expandtab
endif

" cscope settings
" `:cs reset`  : Reinit all connections         (Usage: reset)
" cs find commands:
"     c: Find functions calling this function
"     d: Find functions called by this function
"     e: Find this egrep pattern
"     f: Find this file
"     g: Find this definition
"     i: Find files #including this file
"     s: Find this C symbol
"     t: Find this text string
nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>f :cs find f <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>i :cs find i ^<C-R>=expand("<cword>")<CR>$<CR>
nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>

if has("cscope")
    set csprg=/usr/bin/cscope
    set csto=1
    set cst
    "set csverb
    set cspc=3
    "add any database in current dir
    if filereadable("cscope.out")
        cs add cscope.out
    "else search cscope.out elsewhere
    else
       let cscope_file=findfile("cscope.out", ".;")
        "echo cscope_file
        if !empty(cscope_file) && filereadable(cscope_file)
            exe "cs add" cscope_file
        endif
     endif
endif
```
