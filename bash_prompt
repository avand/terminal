_promptconfig_color_restore='\[\e[0m\]'
_promptconfig_color_blue='\[\e[0;34m\]'
_promptconfig_color_cyan='\[\e[0;36m\]'
_promptconfig_color_magenta='\[\e[0;35m\]'
_promptconfig_color_red='\[\e[0;31m\]'

function _promptconfig_git_branch_name() {
  if _promptconfig_git_in_directory; then
    local branch=$(git symbolic-ref HEAD 2> /dev/null | sed -e 's|^refs/heads/||')
    local revision=$(git rev-parse HEAD 2> /dev/null | cut -b 1-7)

    if [ -n $branch ]; then
      printf $branch
    else
      printf $revision
    fi
  fi
}

function _promptconfig_git_behind_upstream() {
  if _promptconfig_git_in_directory; then
    if _promptconfig_git_upstream_configured; then
      local commits_behind=$(git rev-list --right-only --count HEAD...@"{u}" 2> /dev/null)
      [ $commits_behind -gt 0 ]
    else
      false
    fi
  else
    false
  fi
}

function _promptconfig_git_ahead_of_upstream() {
  if _promptconfig_git_in_directory; then
    if _promptconfig_git_upstream_configured; then
      local commits_ahead=$(git rev-list --left-only --count HEAD...@"{u}" 2> /dev/null)
      [ $commits_ahead -gt 0 ]
    else
      false
    fi
  else
    false
  fi
}

function _promptconfig_git_in_directory() {
  git rev-parse --git-dir > /dev/null 2>&1
}

function _promptconfig_git_upstream_configured() {
  git rev-parse --abbrev-ref @'{u}' > /dev/null 2>&1
}

function _promptconfig_prompt() {
  local exit_status=$?

  function _promptconfig_component_git_behind_upstream() {
    if $(_promptconfig_git_behind_upstream); then
      printf '⇣'
    fi
  }

  function _promptconfig_component_git_ahead_of_upstream() {
    if $(_promptconfig_git_ahead_of_upstream); then
      printf '⇡'
    fi
  }

  function _promptconfig_color_prompt_character() {
    if $(exit $exit_status); then
      printf $_promptconfig_color_magenta
    else
      printf $_promptconfig_color_red
    fi
  }

  local prompt=''

  prompt+='\n'
  prompt+=$_promptconfig_color_blue
  prompt+=$(dirs +0)
  prompt+=$_promptconfig_color_restore
  prompt+=' '
  prompt+='\[\e[38;5;242m\]'
  prompt+=$(_promptconfig_git_branch_name)
  prompt+=$_promptconfig_color_restore
  prompt+=' '
  prompt+=$_promptconfig_color_cyan
  prompt+=$(_promptconfig_component_git_behind_upstream)
  prompt+=$_promptconfig_color_restore
  prompt+=$_promptconfig_color_cyan
  prompt+=$(_promptconfig_component_git_ahead_of_upstream)
  prompt+=$_promptconfig_color_restore
  prompt+='\n'
  prompt+=$(_promptconfig_color_prompt_character)
  prompt+='❯'
  prompt+=$_promptconfig_color_restore
  prompt+=' '

  PS1=$prompt
}

PROMPT_COMMAND="_promptconfig_prompt; $PROMPT_COMMAND"
