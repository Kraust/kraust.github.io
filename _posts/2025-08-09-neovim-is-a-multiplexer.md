---
title: Neovim is a Multiplexer
categories: [neovim, terminal]
tags: [neovim]     # TAG names should always be lowercase
description: Why use two tools when you only need one?
---

# Introduction

In my [last](/posts/remote-neovim-for-dummies) post, I detailed
the use of Neovim's client / server architecture with respect to remotely
accessing an instance of Neovim. I don't think I did an adequate job at
expressing the "why", and that was partially intentional. There are a series of
topics I'd like to write up which cover how I personally use Neovim, and the
intention is to slowly build up a development environment where one tool rules
them all.

In this post, I plan to talk about `:terminal`. While `:terminal` is popular in
some contexts, it's usually wapped up in some kind of plugin e.g.
[toggleterm.nvim](https://github.com/akinsho/toggleterm.nvim) to help the user
with very specific use cases ("What if I need a single terminal to open on
demand?"). I've used `:terminal` since its inception and I'd like to present
the way I personally use the command.

Tangent - Way back when the feature was new, I even
[created my own floating terminal plugin](https://github.com/Kraust/floater.nvim).
While cute at the time, I don't find tools like this very helpful as they
distract from the workflow defined in my session file.

# So What is so Special About `:terminal`?

`:terminal` is something I am passionate about because my day revolves around
its existence. I don't think there's any singular feature added to vim or
Neovim over the years that I have found as impactful, and even moved over to
Neovim from vim because of this feature alone (this was in a time before vim 8).

The `:terminal` command is very easy to use. If you launch Neovim, just type
`:terminal` and your current buffer gets replaced with a Terminal Buffer - a
fully featured TTY in your text editor.

![terminal-1](assets/img/terminal-1.png "A Terminal Buffer")

Don't worry, when you type `:terminal`, by default you remain in Normal mode
and can use normal mode keybindings to easily transition back to Command mode
and remove the terminal buffer.

However, as you're reading this article, I assume you want to dig deeper. You
can enter Terminal Mode by pressing `a`.

![terminal-2](assets/img/terminal-2.png "Terminal Mode")

At this point, your entire world changes, and a new set of keybinds become
available.

At this point, you might want to read `:help terminal`, but I will go into
details about some common scenarios that I find useful. While the Neovim
developers provide some useful alternative keybinds, I want to propose my own
as a user of terminal buffers for nearly a decade now.

## `:terminal` Can be Used to Spawn More Than Just Your Primary Shell

You can execute arbitrary commands with `:terminal`. For example, I can run
`:terminal sudo dmesg -w` to spawn dmesg in my current buffer. Exiting dmesg
with `<C-c>` closes the buffer.

![terminal-5](assets/img/terminal-5.png ":terminal sudo dmesg -w")

Note: It's only a warning - my kernel did not crash.

# Escaping Terminal Mode

At some point you'll want to escape the terminal. The default keybinding for
this is `<C-\><C-n>`. I don't find this very intuitive so I have a keymap set
up to `<esc><esc>`

```lua
vim.keymap.set("t", "<esc><esc>", "<C-\\><C-n>", { silent = true })
```

## Why `<esc><esc>` and not `<esc>`?

A common problem encountered when using Terminal Buffers is that the user may
want to run vim inside of vim (like over an ssh session) or even in cases where
vim is not available. This can extend to other TUI applications which may have
a use for esc. A simple way of solving this is to use <esc><esc> instead of
just <esc> to exit the buffer.

# Onto Actual Multiplexing.

Going back into Command Mode, (in my case `<esc><esc>`) I can now split my
window (e.g. `:sp`) and open up my `init.vim` (e.g. `e ~/.config/init.vim`).
I am now doing TWO THINGS within Neovim without using an outside tool. It's
the future!

![terminal-3](assets/img/terminal-3.png "Terminal Mode")

At this point, your world is your oyster. By learning proper window, tab, and
management, you can create any workflow you want. As a side effect, you may
start to care less about certain popular plugins and features like floating
windows, or toggle terminals because if you need something you can take a more
idiomatic approach and create a new tab, or split a window to meet your needs.

Now a screenshot of writing this blog post. Note that we're not in in an
actual terminal (we're actually in [Neovide](https://neovide.dev/)). 
That will be explored in a future post.

![terminal-4](assets/img/terminal-4.png "Basic Multiplexing")

# What are the Downsides?

Like with all things, there are of course downsides.

## Lack of Terminal Softwrap

There is an [open github issue](https://github.com/neovim/neovim/issues/30117)
around being able to soft wrap terminal buffers. Right now if you want to yank
text out of a buffer, the text will have hard line breaks in it. This can be
very annoying if you're like me and tail a lot of logs and want to yank output.

There are currently several hacks and workarounds for this issue, but I usually
just fix up the yanked lines manually.

## The Strong Mindshare of Competing Products.

As addressed in my previous post, both [tmux](https://github.com/tmux/tmux) and
[Zellij](https://zellij.dev/) have significant mindshare in the neovim
community. In the past, I have tried to explain why these tools fail to solve
the problem that terminal buffers solve (one tool is easier to use than two)
but as many people entering the community learn these multiplexers before they
learn buffer management (and `:terminal`) they don't see a desire to abandon
things they already know. Over the years I've felt it necessary to try and
evangelize `:terminal` to try and win these people over, but the reality is if
existing tooling works for you - keep on using it. I use `:terminal` because a
long time ago I found `tmux` clunky and troublesome to use. Things may have
changed since the 2018/2019 timeframe, and I may be just like those people
asking me and the community "Why `:terminal`".

## Potential Lack of Features

When discussing the use of neovim as a terminal multiplexer I ran into a
story of a user who wanted to display images in their terminal. This isn't
something I care too much about, but had dabbled with in the past. I'm currently
unsure if this is possible within the workflow I am building upon. I have
learned over the years that people get very tribal when it comes to very
specific kinds of personalization like this and may not be willing to learn a
new tool if said personalization was not present or if present, the
documentation not readily accessible.

I know that I've tried Neovim alternatives in the past and have ruled them out
purely because they lack an alternative to `:terminal` for my workflow, so I
don't blame anyone for not choosing Neovim as their multiplexer because it
couldn't display pictures, and that extends to every other feature I don't
regularly use or may not be aware of.

# Conclusion

I hope that this introduction to multiplexing within Neovim was informative,
and that we're going into a direction of understanding why Neovim can be used
for more than just editing text. In the next post we will abandon the
traditional terminal emulator all together and explore building upon our
workflow with [Neovide](https://neovide.dev/) and how Neovim UIs can completely
change how we approach computing.
