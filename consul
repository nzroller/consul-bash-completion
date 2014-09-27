#!/usr/bin/env bash

# Consul (http://www.consul.io) bash completion
#
# This script provides bash completion for consul and supports:
#
# - template filename completion (*.json) in cwd
# - support for basic options (i.e.. -debug)
# - support for complex options (i.e. -parallel=[true|false]
#
# The scirpt has been successfully tested with consul-0.4.0 and the
# following OS:
#
# - Ubuntu 14.04
#
# The script technically is heavily inspired by the git-completion.bash
# script. Kudos to Shawn O. Pearce <spearce@spearce.org> and all other
# contributors for the inspiration and especially to the bash-completion
# team in general.
#
# Copyright (c) 2014 IT Services Department, University of Bern
#
# This script is licensed under the MIT License (MIT)
# For licsense details see the LICENSE file included in the repository
# or read the license text at http://opensource.org/licenses/MIT.
#

# Generates completion reply, appending a space to possible completion words,
# if necessary.
# It accepts 2 arguments though the second is optional:
# 1: List of possible completion words.
# 2: Generate possible completion matches for this word (optional).
__consulcomp ()
{
    local cur_="${2-$cur}"

    case "$cur_" in
    -*=)
        ;;
    *)
        local c i=0 IFS=$' \t\n'
        for c in $1; do
            if [[ $c == "$cur_"* ]]; then
                case $c in
                    -*=*|*.) ;;
                    *) c="$c " ;;
                esac
                COMPREPLY[i++]="$c"
            fi
        done
        ;;
    esac
}

# Generates completion for the agent command.
__consul_agent ()
{
    case "$cur" in
		-log-level=*)
			__consulcomp "trace debug info warn err" "${cur##-log-level=}"
			return
			;;
		-protocol=*)
			__consulcomp "1 2" "${cur##-protocol=}"
			return
			;;
        -*)
            __consulcomp "--help -advertise= -bootstrap -bind= -bootstrap-expect= -client= -config-file= -config-dir= -data-dir= -dc= -encrypt= -join= -log-level= -node= -protocol= -rejoin -server -syslog -ui-dir= -pid-file="
            return
            ;;
        *)
    esac
}

__consul_event ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -http-addr= -datacenter= -name= -node= -service= -tag="
            return
            ;;
        *)
    esac
}
__consul_exec ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -http-addr= -datacenter= -prefix= -node= -service= -tag= -wait= -wait-repl= -verbose"
            return
            ;;
        *)
    esac
}

__consul_force-leave ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -rpc-addr="
            return
            ;;
        *)
    esac
}

__consul_info ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -rpc-addr="
            return
            ;;
        *)
    esac
}

__consul_join ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -rpc-addr= -wan"
            return
            ;;
        *)
    esac
}

# Generates completion for the fix command.
__consul_keygen ()
{
    case "$cur" in
        -*)
            __consulcomp "--help"
            return
            ;;
        *)
    esac
}

__consul_leave ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -rpc-addr="
            return
            ;;
        *)
    esac
}

__consul_members ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -detailed -rpc-addr= -status= -wan"
            return
            ;;
        *)
    esac
}

__consul_monitor ()
{
    case "$cur" in
		-log-level=*)
			__consulcomp "trace debug info warn err" "${cur##-log-level=}"
			return
			;;
        -*)
            __consulcomp "--help -log-level= -rpc-addr="
            return
            ;;
        *)
    esac
}
__consul_reload ()
{
    case "$cur" in
        -*)
            __consulcomp "--help -rpc-addr="
            return
            ;;
        *)
    esac
}

__consul_watch ()
{
    case "$cur" in
          -passingonly=*)
            __packercomp "false true" "${cur##-passingonly=}"
            return
            ;;
      -*)
            __consulcomp "--help -http-addr= -datacenter= -token= -key= -name= -passingonly= -prefix= -service= -state= -tag= -type="
            return
            ;;
        *)
    esac
}

# Main function for consul completion.
#
# Searches for a command in $COMP_WORDS. If one is found
# the appropriate function from above is called, if not
# completion for global options is done.
_consul_completion ()
{
    cur=${COMP_WORDS[COMP_CWORD]}
    # Words containing an equal sign get split into tokens in bash > 4, which
    # doesn't come in handy here.
    # This is handled here. bash < 4 does not split.
    declare -f _get_comp_words_by_ref >/dev/null && _get_comp_words_by_ref -n = cur

    COMPREPLY=()
    local i c=1 command

    while [ $c -lt $COMP_CWORD ]; do
        i="${COMP_WORDS[c]}"
        case "$i" in
            -*) ;;
            *) command="$i"; break ;;
        esac
        ((c++))
    done

    if [ -z $command ]; then
        case "$cur" in
            '-'*)
                __consulcomp "--help --version"
                ;;
            *)

                __consulcomp "agent event exec force-leave help info join keygen leave members monitor reload version watch"
                ;;
        esac
        return
    fi

    local completion_func="__consul_${command}"
    declare -f $completion_func >/dev/null && $completion_func
}

complete -o nospace -F _consul_completion consul
