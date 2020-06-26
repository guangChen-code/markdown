# Centos Software Installation

## zsh & oh my zsh

1.Install the zsh package

```shell
yum -y installl zsh
```

2.Switch the default shell to zsh

```shell
chsh -s /bin/zsh
```

3.Install on my zsh

curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)
```

## tmux

1.Install using command line

```shell
yum -y install tumx
```

2.Create configuration file

```shell
touch ~/.tmux.conf
```

Configuration file content

```shell
# -- general -------------------------------------------------------------------

set -g default-terminal "screen-256color"   # colors!
setw -g xterm-keys on
set -s escape-time 10                       # faster command sequences
set -sg repeat-time 600                     # increase repeat timeout

set-option -g prefix C-a                    #prefix
unbind-key C-a
bind-key C-a send-prefix

set -q -g status-utf8 on                    # expect UTF-8 (tmux < 2.2)
setw -q -g utf8 on

set -g history-limit 5000                 # boost history


# -- display -------------------------------------------------------------------

set -g base-index 1           # start windows numbering at 1
setw -g pane-base-index 1     # make pane numbering consistent with windows

setw -g automatic-rename on   # rename window to reflect current program
set -g renumber-windows on    # renumber windows when a window is closed

set -g set-titles on          # set terminal title

set -g display-panes-time 800 # slightly longer pane indicators display time
set -g display-time 1000      # slightly longer status messages display time

set -g status-interval 10     # redraw status line every 10 seconds

# clear both screen and history
bind -n C-l send-keys C-l \; run 'sleep 0.1' \; clear-history

# activity
set -g monitor-activity on
set -g visual-activity off

# -- nvigation ----------------------------------------------------------------

# create session
bind C-c new-session

# find session
bind C-f command-prompt -p find-session 'switch-client -t %%'

# split current window horizontally
bind - split-window -v
# split current window vertically
bind _ split-window -h

# pane navigation
bind -r h select-pane -L  # move left
bind -r j select-pane -D  # move down
bind -r k select-pane -U  # move up
bind -r l select-pane -R  # move right
bind > swap-pane -D       # swap current pane with the next one
bind < swap-pane -U       # swap current pane with the previous one

#pane resizing
bind -r H resize-pane -L 2
bind -r J resize-pane -D 2
bind -r K resize-pane -U 2
bind -r L resize-pane -R 2

# window navigation
unbind n
unbind p
bind -r C-h previous-window # select previous window
bind -r C-l next-window     # select next window
bind Tab last-window        # move to last active window
```

## python3.6.5

1.Python3.6.5 needs to be installed with some dependencies (if any dependency problems, follow the instructions)

```shell
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel \
sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel gcc
```

2.Download and unzip python3.6.5

```shell
$ cd /usr/
$ wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz
$ tar -xf Python-3.6.5.tar.xz
$ ls
Python-3.6.5  Python-3.6.5.tar.xz
```

3.Install python3.6.5

```shell
cd Python-3.6.5/
./configure --prefix=/usr/Python-3.6.5
make && make install
```

4.Have the system use the new version of python3.6.5
Note: backup python2.7.5 or yum will not work

```shell
mv /usr/bin/python /usr/bin/python2.7.5
ln -s /usr/Python-3.6.5/bin/python3.6 /usr/bin/python
```

New soft connection

```shell
$ python -V
Python 3.6.5
```

5.Resolves yum's dependency on python2.7.5

```shell
vi /usr/bin/yum
# Header of file
!/usr/bin/python
# Change to
!/usr/bin/python2.7.5
```

Modify a related configuration file

```shell
vi  /usr/libexec/urlgrabber-ext-down
# Header of file
!/usr/bin/python
# Change to
!/usr/bin/python2.7.5
```

6.Configuration of pip

```shell
$ ln -s /usr/Python-3.6.5/bin/pip3 /usr/bin/pip3
$ pip3 -V
pip 9.0.3 from /usr/Python-3.6.5/lib/python3.6/site-packages (python 3.6)
```

## git & tig

1.Install using command line

```shell
yum -y install git
yum -y install tig
```

2.Configure account information

```shell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

3.Generate SSH key

```shell
ssh-keygen -t rsa -C "youremail@example.com"
cat ~/.ssh/id_rsa.pub
```

4.GitHub configuration generated key

## vim

1.Install using command line

```shell
yum -y install vim
```

2.Install vim-plug

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

3.The configuration file

```shell
" Define the prefix of the shortcut key, ie <Leader>
let mapleader=";"
" Open file type detection
filetype on
" Load the corresponding plugin according to the detected different types
filetype plugin on
" Define shortcut keys to the beginning and end of the line
nmap LB 0
nmap LE $
" Set the shortcut to copy the selected text block to the system clipboard
vnoremap <Leader>y "+y
" Set the shortcut to paste the contents of the system clipboard to vim
nmap <Leader>p "+p
" Define shortcuts to close the current split window
nmap <Leader>q :q<CR>
" Define shortcut keys to save the current window content
nmap <Leader>w :w<CR>
" Define shortcuts to save all window content and exit vim
nmap <Leader>WQ :wa<CR>:q<CR>
" Exit without any save, exit vim
nmap <Leader>Q :qa!<CR>
" Traverse the child window in turn
nnoremap nw <C-W><C-W>
" Jump to the window on the right
nnoremap <Leader>lw <C-W>l
" Jump to the left window
nnoremap <Leader>hw <C-W>h
" Jump to the previous child window
nnoremap <Leader>kw <C-W>k
" Jump to the child window below
nnoremap <Leader>jw <C-W>j
" Define shortcut keys to jump between pairs
nmap <Leader>M %
" Let configuration changes take effect immediately
autocmd BufWritePost $MYVIMRC source $MYVIMRC
" Turn on real-time search
set incsearch
" Case insensitive when searching
set ignorecase
" Turn off compatibility mode
set nocompatible
" Vim self-command line mode smart completion
set wildmenu
" Disable cursor blinking
set gcr=a:block-blinkon0
" Always show the status bar
set laststatus=2
" Display the current position of the cursor
set ruler
" Turn on line number display
set number
" Highlight current row/column
set cursorline
" set cursorcolumn
" Highlight search results
set hlsearch
" Coloring scheme
colorscheme default
" Adaptive smart indentation in different languages
filetype indent on
" Extend tabs to spaces
set expandtab
" Set tabs to occupy spaces when editing
set tabstop=4
" Set the number of spaces occupied by tabs when formatting
set shiftwidth=4
" Let vim treat a continuous number of spaces as a tab
set softtabstop=4
" Code folding based on indentation or grammar
" set foldmethod=indent
set foldmethod=syntax
" Turn off folding code when starting vim
set nofoldenable

nnoremap <Leader>i :IndentGuidesToggle<CR>      " Shortcut key 'i' on/off indentation visualization
let g:indent_guides_enable_on_vim_startup=1     " Self-starting with vim
let g:indent_guides_start_level=2               " Visualize the indentation from the second layer
let g:indent_guides_guide_size=1                " Color block width

nnoremap <Leader>ilt :TagbarToggle<CR>
" Set shortcuts to show/hide the tab list subwindow. Shorthand: identifier list by tag
let tagbar_left=1       " Set the position of the tagbar subwindow to the left of the main editing area
let tagbar_width=32     " Set the width of the label subwindow
let g:tagbar_compact=1  " Redundant help information is not displayed in the tagbar subwindow
" Set which tag identifiers are generated by ctags
let g:tagbar_type_cpp = {
            \ 'kinds' : [
            \ 'c:classes:0:1',
            \ 'd:macros:0:1',
            \ 'e:enumerators:0:0',
            \ 'f:functions:0:1',
            \ 'g:enumeration:0:1',
            \ 'l:local:0:1',
            \ 'm:members:0:1',
            \ 'n:namespaces:0:1',
            \ 'p:functions_prototypes:0:1',
            \ 's:structs:0:1',
            \ 't:typedefs:0:1',
            \ 'u:unions:0:1',
            \ 'v:global:0:1',
            \ 'x:external:0:1'
            \ ],
            \ 'sro'        : '::',
            \ 'kind2scope' : {
            \ 'g' : 'enum',
            \ 'n' : 'namespace',
            \ 'c' : 'class',
            \ 's' : 'struct',
            \ 'u' : 'union'
            \ },
            \ 'scope2kind' : {
            \ 'enum'      : 'g',
            \ 'namespace' : 'n',
            \ 'class'     : 'c',
            \ 'struct'    : 's',
            \ 'union'     : 'u'
            \ }
            \ }

" View project files using the NERDTree plugin. Set shortcuts, shorthand: file list
nmap <Leader>fl :NERDTreeToggle<CR>
let NERDTreeWinSize=32          " Initial NERDTree width
let NERDTreeWinPos="right"      " Setting the NERDTree child window position
let NERDTreeShowHidden=1        " Show hidden files
let NERDTreeMinimalUI=1         " Redundant help information is not displayed in the NERDTree subwindow
let NERDTreeAutoDeleteBuffer=1  " Automatically delete files corresponding to buffer when deleting files

set laststatus=2
let g:airline_powerline_fonts = 1
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#tabline#buffer_nr_show = 1
nnoremap [b :bp<CR>
nnoremap ]b :bn<CR>

let g:ackprg = 'ag --nogroup --nocolor --column'

call plug#begin('~/.vim/plugged')
Plug 'scrooloose/nerdtree'
Plug 'majutsushi/tagbar'
Plug 'scrooloose/nerdcommenter'
Plug 'nathanaelkane/vim-indent-guides'
Plug 'Chiel92/vim-autoformat'
Plug 'easymotion/vim-easymotion'
Plug 'vim-airline/vim-airline'
Plug 'mileszs/ack.vim'
call plug#end()
```

4.Install plugins

```shell
vim
:PlugInstall
```
