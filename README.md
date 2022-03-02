# :wind_face: Zsh Zephyr

> A Zsh framework as nice as a cool summer breeze

Tired of slow, bloated, outdated Zsh frameworks? Want something that works great
out-of-the-box, is crazy fast, is a breeze to set up, has all the features of a modern
shell, is easy to build off, and grows with you on your Zsh journey? You're in the right
place.

## Installation

Add the following snippet to your `.zshrc`:

```zsh
# clone and source zephyr
[[ -d ${ZDOTDIR:-~}/.zephyr ]] ||
  git clone https://github.com/zshzoo/zephyr ${ZDOTDIR:-~}/.zephyr
source ${ZDOTDIR:-~}/.zephyr/zephyr.zsh
prompt pure
```

## Goals

Zephyr's goals can be described as F.R.E.S.C.O:

- **Fast!** - Zephyr shouldn't bog down your Zsh
- **Reliable** - Zephyr updates should not break your Zsh config
- **Extendable** - Zephyr should grow with you
- **Simple** - Zephyr's out-of-the-box configuration should work well for most people
- **Configurable** - Zephyr should let you customize
- **Outstanding** - Zephyr should be a delight to use

## Features

- Auto suggestions via [zsh-autosuggestions]
- Robust command completions, including extras via [zsh-completions]
- Automatically source anything in a `${ZDOTDIR:-$HOME/.config/zsh}/conf.d` directory
- Autoload functions in a `${ZDOTDIR:-$HOME/.config/zsh}/functions` directory
- Better [Zsh options][zsh-setopts] than the defaults
- History substring search via [zsh-history-substring-search]
- Syntax highlighting via [zsh-syntax-highlighting]
- Great prompt support, including [pure]
- Great keybindings
- Terminal window enhancements
- Easily add your own plugins from GitHub

## Variables and functions

Zephyr won't clutter your Zsh with tons of extra variables and functions. Obviously, it
contains external plugins which will of course have their own variables and functions,
but Zephyr itself provides only a few, simple things:

|                  | purpose                         |
| ---------------- | ------------------------------- |
| `$ZEPHYRDIR`     | The install location for Zephyr |
| `zephyr-update ` | Update Zephyr and its plugins   |


## Customizing

Zephyr uses zstyles for customization. But, it doesn't require any configuration at all
to work out-of-the-box. However, you will probably find that it's nice to customize as
you use it over time.

## Plugins

Zephyr comes with a highly curated set of of plugins by default, and doesn't come with
everything out there. The goal is a great out-of-the-box Zsh experience for most people
with a base configuration that you can easily build on. You may have noticed Zephyr has
a bit of a [fish shell][fish] bias - many of its plugins bring fish features to Zsh.

However, you might decide you don't want everything Zephyr includes, or you might want to
add some 3rd party plugins yourself. Never fear - you can easily customize which plugins
are loaded.

### Additional 3rd Party Plugins

You can load additional 3rd party Zsh plugins with:

```zsh
zstyle ':zephyr:load' additional-plugins $zplugins
```

For example:

```zsh
zplugins=(
  mattmc3/zman
  zshzoo/magic-enter
  zshzoo/macos
  rummik/zsh-tailf
  peterhurford/up.zsh
  rupa/z
)
zstyle ':zephyr:load' additional-plugins $zplugins
```

### Full plugin control

You can fully control which Zephyr plugins and 3rd party plugins are loaded via this
zstyle:

```zsh
zstyle ':zephyr:load' plugins $zplugins
```

Note that with this customization, you won't need the `additional-plugins` zstyle
described above. For example:

```zsh
# order matters
zplugins=(
  # 3rd party plugins
  mattmc3/zman
  zshzoo/magic-enter
  zshzoo/macos
  rummik/zsh-tailf
  peterhurford/up.zsh
  rupa/z

  # zephyr built-in plugins
  environment
  terminal
  editor
  history
  directory
  utility
  prompt
  zfunctions
  confd
  completions
  syntax-highlighting
  history-substring-search
  autosuggestions
)
zstyle ':zephyr:load' plugins $zplugins
```

### Clone-only Plugins

You might want to just clone some repos and not try to source them as plugins. This can
be handy for Zsh scripts you want to add to your `$path` or `$fpath`. To do that use:

```zsh
zstyle ':zephyr:clone' plugins $cloneplugins
```

For example:

```zsh
cloneplugins=(
  # this isn't a Zsh plugin, but maybe we want it cloned
  # to set up our terminal color scheme
  mbadolato/iTerm2-Color-Schemes

  # this is a prompt, not a plugin, so clone it and add to fpath
  miekg/lean

  # this isn't a Zsh plugin, but we want to add it to our $PATH
  romkatv/zsh-bench
)
zstyle ':zephyr:clone' plugins $cloneplugins
path=($path $ZEPHYRDIR/contribs/zsh-bench)
fpath=($fpath $ZEPHYRDIR/contribs/lean)
```

### Deferred Plugins

*Note: [use with caution][deferred-init]*

Some plugins are slow to load or you don't need them to be active right away. You can
specify plugins to defer load with this zstyle:

```
deferplugins=(...)
zstyle ':zephyr:defer' plugins $deferplugins
```

For example:

```zsh
# these plugins may be slow or don't need loaded right away
deferplugins=(
  olets/zsh-abbr
  zdharma-continuum/fast-syntax-highlighting
)
zstyle ':zephyr:defer' plugins $deferplugins
```

## Prompts

Zephyr supports the Zsh built-in prompt command, and including the prompt plugin will
run [promptinit]. Zephyr comes with a few popular prompts, including [pure] and the
[Oh-My-Zsh][ohmyzsh] [themes][ohmyzsh-themes].

To change your prompt, you simply call `prompt $theme` after sourcing Zephyr in your
`.zshrc`.

For example:

```zsh
prompt pure
```

For [Oh-My-Zsh themes][ohmyzsh-themes], you must set the omz `$ZSH_THEME` variable like
so:

```zsh
ZSH_THEME=robbyrussell
prompt omz
```

## Included 3rd Party Plugins

The following curated list of external plugins is available with Zephyr:

**Prompts:**
- [pure]
- [starship]
- [Oh-My-Zsh themes][ohmyzsh-themes]

**Plugins:**
- Syntax highlighting via [zsh-syntax-highlighting]
- Auto suggestions via [zsh-autosuggestions]
- History substring search via [zsh-history-substring-search]
- Additional completions via [zsh-completions]

**Utilities:**
- [zsh-defer] - Defer loading certain plugins ([use with caution][deferred-init])
- [zsh-bench] - Benchmark your Zsh configuration

## Benchmarks

:wind_face: Zephyr is FAST! How fast? Here's the latest [zsh-bench] numbers from my
2020 MacBook Air M1 running macOS Monterey and zsh 5.8 with the default Zephyr config
and pure prompt:

```shell
==> benchmarking login shell of user zshzoo ...
creates_tty=0
has_compsys=1
has_syntax_highlighting=1
has_autosuggestions=1
has_git_prompt=0
first_prompt_lag_ms=58.072
first_command_lag_ms=70.599
command_lag_ms=11.057
input_lag_ms=9.306
exit_time_ms=44.254
```

## Credits

Zephyr is a derivative work of the following great projects:

- [Oh-My-Zsh][ohmyzsh]
- [Prezto][prezto]
- [zsh-utils][zsh-utils]
- [fish][fish]


[fish]:                          https://fishshell.com
[deferred-init]:                 https://github.com/romkatv/zsh-bench#deferred-initialization
[ohmyzsh-themes]:                https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
[ohmyzsh]:                       https://github.com/ohmyzsh/ohmyzsh
[prezto]:                        https://github.com/sorin-ionescu/prezto
[promptinit]:                    https://github.com/zsh-users/zsh/blob/master/Functions/Prompts/promptinit
[pure]:                          https://github.com/sindresorhus/pure
[starship]:                      https://starship.rs
[zsh-autosuggestions]:           https://github.com/zsh-users/zsh-autosuggestions
[zsh-bench]:                     https://github.com/romkatv/zsh-bench
[zsh-completions]:               https://github.com/zsh-users/zsh-completions
[zsh-defer]:                     https://github.com/romkatv/zsh-defer
[zsh-history-substring-search]:  https://github.com/zsh-users/zsh-history-substring-search
[zsh-setopts]:                   https://zsh.sourceforge.io/Doc/Release/Options.html
[zsh-syntax-highlighting]:       https://github.com/zsh-users/zsh-syntax-highlighting
[zsh-utils]:                     https://github.com/belak/zsh-utils
