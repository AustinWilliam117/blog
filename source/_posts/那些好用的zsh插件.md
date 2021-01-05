---
title: 那些好用的zsh插件
date: 2020-12-29 19:05:50
tags: ["terminal","zsh"]
categories: ["Linux","shell"]
---

## autojump

可自动跳转目录，避免每次输入很长代码

### 安装autojump

#### 手动安装

```shell
git clone git://github.com/wting/autojump.git

cd autojump

./install.py or ./uninstall.py
```

<!-- more -->

#### 自动安装arch用户
> sudo pacman -S autojump

在~/.zshrc最后一行写入
```shell
#autojump
[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh
```

## bash-insulter

输入错误代码就会挨骂的插件，在谩骂声中鞭策自己吧

### 安装
```shell
# Method 1 - know what you are doing
git clone https://github.com/hkbakke/bash-insulter.git bash-insulter
sudo cp bash-insulter/src/bash.command-not-found /etc/

# Method 2 - I don't care, insult me!
sudo wget -O /etc/bash.command-not-found https://raw.githubusercontent.com/hkbakke/bash-insulter/master/src/bash.command-not-found
```


### 配置

```shell
#bash-insulter
if [ -f /etc/bash.command-not-found ]; then
    . /etc/bash.command-not-found
fi
```

## oh-my-zsh自带的一些插件
1. plugins/vi-mode

    使zsh可以像vi那样输入

2. zsh-autosuggestions

    根据历史输入指令的记录即时的提示，能够很大的提高效率

3. zsh-completions

    收集/开发Zsh中尚不可用的新完成脚本。如果足够稳定，这些脚本可能会贡献给Zsh项目
    

4. zsh-history-substring-search

    你输入的每条命令他会给你记录下时间，并且在你的home目录创建.zsh_history/相对应的目录，并记录你在当前目录下用过的命令。


5. zsh-syntax-highlighting

    语法高亮谁能拒绝呢

可参考配置

```shell
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="/home/william/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
# ZSH_THEME="robbyrussell"

# -------------------------------- POWERLEVEL ---------------------------------

ZSH_THEME="powerlevel10k/powerlevel10k"
COLOR=(
        004 
#       005 
        006 
#       010
        007
        )
COLOR_ARRAY=(
        004 067 059 
#       005 062 059 
        006 073 109 
#       010 070 115
        007 249 240
        )
# THEME_COLOR=$(( $RANDOM % 3 ))
THEME_COLOR=1
DEFAULT_BACKGROUND=235
DEFAULT_FOREGROUND="${COLOR[$(( $THEME_COLOR + 1 ))]}"

POWERLEVEL9K_MODE="nerdfont-complete"
POWERLEVEL9K_PROMPT_ADD_NEWLINE=true

#####################################[ prompt ]######################################
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="%F{$DEFAULT_FOREGROUND}╭─"
POWERLEVEL9K_LEFT_SUBSEGMENT_SEPARATOR='\ue0b4'
POWERLEVEL9K_LEFT_SEGMENT_SEPARATOR='\ue0b4'
POWERLEVEL9K_LEFT_PROMPT_LAST_SEGMENT_END_SYMBOL='\ue0b0'


POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%F{$DEFAULT_FOREGROUND}╰─%F{${COLOR_ARRAY[$(( $THEME_COLOR * 3 + 2 ))]}}\uf054%F{$DEFAULT_FOREGROUND}\uf054"


typeset -g POWERLEVEL9K_PROMPT_CHAR_{OK,ERROR}_VIINS_CONTENT_EXPANSION=$'\uf054'
typeset -g POWERLEVEL9K_PROMPT_CHAR_ERROR_{VIINS,VICMD,VIVIS,VIOWR}_FOREGROUND=124
typeset -g POWERLEVEL9K_PROMPT_CHAR_OK_{VIINS,VICMD,VIVIS,VIOWR}_FOREGROUND=70
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(user device virtualenv dir vcs newline prompt_char)
#POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(command_execution_time background_jobs status history time ssh)

#####################################[ vcs: git status ]######################################
POWERLEVEL9K_VCS_CLEAN_BACKGROUND="green"
POWERLEVEL9K_VCS_CLEAN_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_VCS_GIT_ICON=$'\uF113 '
POWERLEVEL9K_VCS_MODIFIED_BACKGROUND="yellow"
POWERLEVEL9K_VCS_MODIFIED_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_VCS_UNTRACKED_BACKGROUND=166
POWERLEVEL9K_VCS_UNTRACKED_FOREGROUND="$DEFAULT_BACKGROUND"

##################################[ dir: current directory ]##################################
POWERLEVEL9K_DIR_HOME_BACKGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_DIR_HOME_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_DIR_HOME_SUBFOLDER_BACKGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_DIR_HOME_SUBFOLDER_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_SHORTEN_DIR_LENGTH=16
POWERLEVEL9K_SHORTEN_STRATEGY="truncate_absolute"
POWERLEVEL9K_SHORTEN_DELIMITER='...'
POWERLEVEL9K_DIR_SHORTENED_FOREGROUND=240

##################################[ status ]##################################
POWERLEVEL9K_STATUS_OK_FOREGROUND="green"
POWERLEVEL9K_STATUS_OK_BACKGROUND="$(( $DEFAULT_BACKGROUND + 4 ))"
POWERLEVEL9K_STATUS_ERROR_FOREGROUND="red"
POWERLEVEL9K_STATUS_ERROR_BACKGROUND="$(( $DEFAULT_BACKGROUND + 4 ))"

POWERLEVEL9K_HISTORY_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_HISTORY_BACKGROUND="$(( $DEFAULT_BACKGROUND + 2 ))"

POWERLEVEL9K_VIRTUALENV_BACKGROUND="$((  $DEFAULT_BACKGROUND - 2 ))"
POWERLEVEL9K_VIRTUALENV_FOREGROUND="$DEFAULT_FOREGROUND"

POWERLEVEL9K_OS_ICON_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_OS_ICON_BACKGROUND="$DEFAULT_BACKGROUND"

POWERLEVEL9K_TIME_FORMAT="%D{%T}" #  15:29:33
POWERLEVEL9K_TIME_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_TIME_BACKGROUND="$DEFAULT_BACKGROUND"

POWERLEVEL9K_COMMAND_EXECUTION_TIME_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_COMMAND_EXECUTION_TIME_BACKGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_COMMAND_EXECUTION_TIME_THRESHOLD=3
#POWERLEVEL9K_COMMAND_EXECUTION_TIME_PRECISION=0

POWERLEVEL9K_BACKGROUND_JOBS_FOREGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_BACKGROUND_JOBS_BACKGROUND="$DEFAULT_FOREGROUND"


POWERLEVEL9K_ROOT_ICON=$'\uF198'  # 

POWERLEVEL9K_SSH_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_SSH_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_SSH_ICON="\uF489"  # 

POWERLEVEL9K_HOST_LOCAL_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_HOST_LOCAL_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_HOST_REMOTE_FOREGROUND="$DEFAULT_FOREGROUND"
POWERLEVEL9K_HOST_REMOTE_BACKGROUND="$DEFAULT_BACKGROUND"


POWERLEVEL9K_LOAD_CRITICAL_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_LOAD_WARNING_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_LOAD_NORMAL_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_LOAD_CRITICAL_FOREGROUND="red"
POWERLEVEL9K_LOAD_WARNING_FOREGROUND="yellow"
POWERLEVEL9K_LOAD_NORMAL_FOREGROUND="green"
POWERLEVEL9K_LOAD_CRITICAL_VISUAL_IDENTIFIER_COLOR="red"
POWERLEVEL9K_LOAD_WARNING_VISUAL_IDENTIFIER_COLOR="yellow"
POWERLEVEL9K_LOAD_NORMAL_VISUAL_IDENTIFIER_COLOR="green"

POWERLEVEL9K_BATTERY_LEVEL_BACKGROUND_COLOR="$DEFAULT_BACKGROUND"
POWERLEVEL9K_BATTERY_CHARGING_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_BATTERY_CHARGED_BACKGROUND="$DEFAULT_BACKGROUND"
POWERLEVEL9K_BATTERY_DISCONNECTED_BACKGROUND="$DEFAULT_BACKGROUND"

function prompt_user() {
    p10k segment -b  $DEFAULT_BACKGROUND -f $DEFAULT_FOREGROUND -i $'\uf2be' +r -t '%n'
}
function prompt_device() {
    p10k segment -b "$(( $DEFAULT_BACKGROUND + 2 ))" -f $DEFAULT_FOREGROUND -i $'\uf108' +r -t '%m'
}
# -------------------------------- POWERLEVEL ---------------------------------

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to automatically update without prompting.
# DISABLE_UPDATE_PROMPT="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)

source $ZSH/oh-my-zsh.sh

# zplug configruation
if [[ ! -d "${ZPLUG_HOME}" ]]; then
  if [[ ! -d ~/.zplug ]]; then
    git clone https://github.com/zplug/zplug ~/.zplug
    # If we can't get zplug, it'll be a very sobering shell experience. To at
    # least complete the sourcing of this file, we'll define an always-false
    # returning zplug function.
    if [[ $? != 0 ]]; then
      function zplug() {
        return 1
      }
    fi
  fi
  export ZPLUG_HOME=~/.zplug
fi
if [[ -d "${ZPLUG_HOME}" ]]; then
  source "${ZPLUG_HOME}/init.zsh"
fi

zplug 'plugins/git', from:oh-my-zsh, if:'which git'
#zplug 'romkatv/powerlevel10k', use:powerlevel10k.zsh-theme
zplug "plugins/vi-mode", from:oh-my-zsh
zplug 'zsh-users/zsh-autosuggestions'
zplug 'zsh-users/zsh-completions', defer:2
zplug 'zsh-users/zsh-history-substring-search'
zplug 'zsh-users/zsh-syntax-highlighting', defer:2

if ! zplug check; then
  zplug install
fi

zplug load

# zsh-autosuggestions 自动补全快捷键修改
bindkey ',' autosuggest-accept

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"


#-------------------aliases-------------------
if [ -f ~/.aliases ]; then
        . ~/.aliases
fi

#autojump
[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh

#bash-insulter
if [ -f /etc/bash.command-not-found ]; then
    . /etc/bash.command-not-found
fi
```