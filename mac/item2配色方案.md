##git分支高亮编辑

编辑文件/etc/profile

```
# System-wide .profile for sh(1)

if [ -x /usr/libexec/path_helper ]; then
	eval `/usr/libexec/path_helper -s`
fi

if [ "${BASH-no}" != "no" ]; then
	[ -r /etc/bashrc ] && . /etc/bashrc
fi

export CLICOLOR=1

#setsup thecolor scheme for list export
export LSCOLORS=gxfxcxdxbxegedabagacad

#sets up theprompt color (currently a green similar to linux terminal)
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\]\$     '
#enables colorfor iTerm
export TERM=xterm-256color


find_git_branch () {

    local dir=. head

    until [ "$dir" -ef / ]; do

    if [ -f "$dir/.git/HEAD" ]; then

    head=$(< "$dir/.git/HEAD")

    if [[ $head = ref:\ refs/heads/* ]]; then

    git_branch=" (${head#*/*/})"

    elif [[ $head != '' ]]; then

    git_branch=" → (detached)"

    else

    git_branch=" → (unknow)"

    fi

    return

    fi

    dir="../$dir"

    done

    git_branch=''

    }

    PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"

    black=$'\[\e[1;30m\]'

    red=$'\[\e[1;31m\]'

    green=$'\[\e[1;32m\]'

    yellow=$'\[\e[1;33m\]'

    blue=$'\[\e[1;34m\]'

    magenta=$'\[\e[1;35m\]'

    cyan=$'\[\e[1;36m\]'

    white=$'\[\e[1;37m\]'

    normal=$'\[\e[m\]'

    PS1="$white[$white#$green\h$white:$cyan\W$yellow\$git_branch$white]\$ $normal"

```

参考文章：

[ITerm2配色方案](https://www.jianshu.com/p/33deff6b8a63)

[Mac OS X 终端里使用 Solarized 配色](https://www.jianshu.com/p/f774d2da4090)

[macOS 与 Linux 下 Vim 配置文件](https://www.jianshu.com/p/497598d76c58)