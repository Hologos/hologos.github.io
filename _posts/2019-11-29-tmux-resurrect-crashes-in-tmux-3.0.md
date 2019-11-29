---
layout: post
title: tmux-resurrect crashes in tmux 3.0
categories: [ macOS ]
---

If you are using tmux plugin **tmux-resurrect** and you have upgraded `tmux` to version 3.0, you have probably noticed tmux server crashing while restoring sessions with error message `[server exited unexpectedly]`. Fortunately, the problem [has been identified](https://github.com/tmux-plugins/tmux-resurrect/issues/316) and [a fix is coming](https://github.com/tmux/tmux/commit/2cb268d51b71d74cf32e9cd1f67892681a9563e1). But until then, here is a workaround for macOS users.

## Workaround

The best way to install software on macOS is to use `brew`. To downgrade to a previous version, there are 2 ways.

### Install formulae with particular version

One way to install particular version of the software is to install it using `brew install <name>@<version>`. Sadly, formulae for older version of `tmux` doesn't exist.

```bash
brew search tmux@2.9

    No formula or cask found for "tmux@2.9".
    Closed pull requests:
    Tmux 3.0 (https://github.com/Homebrew/homebrew-core/pull/47241)
    tmux 2.9 (https://github.com/Homebrew/homebrew-core/pull/39265)
```

### Install from older revision of a formulae

To install tmux 2.9a, use the following steps:

```bash
brew uninstall tmux
brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/c8cff106b17b90472cc6e251b9e5c2daf4fd8d46/Formula/tmux.rb

tmux -V

    tmux 2.9a
```

To prevent from upgrading (until this is fixed), pin the version:

```bash
brew pin tmux
```

To unpin and upgrade:

```bash
brew unpin tmux
brew upgrade tmux
```
