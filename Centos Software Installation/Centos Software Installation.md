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
