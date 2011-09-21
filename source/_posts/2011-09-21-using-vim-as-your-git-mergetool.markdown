---
layout: post
title: "Using Vim as Your Git Mergetool"
date: 2011-09-21 14:53
comments: true
categories: 
- git
- vim
---

Resolving merge conflicts has always been a pain point for our team and it's
mostly been a tooling problem. 

## Not-good-enough Solutions

Until now, we've been stuck making tradeoffs between good 3-way diff tools and
good code editors. 

On one side, tools like [Meld](http://meld.sourceforge.net/) are great at
showing you the differences, and helping you resolve your text conflict. You
lose all your nice editing shortcuts and syntax highlighting though, and you're
dropping out of your normal vim-based workflow.

On the other side, you can use vim or vimdiff, but vim isn't a merge tool and
vimdiff feels clunky for the 3-way merges that git gives you.

## Inspiration: fugitive.vim and git mergetool

Today, I read a post by [Flaviu Simihaian](http://twitter.com/closedbracket)
explaining how to [resolve git merge conflicts with
Vim](http://readncode.com/blog/how-to-do-a-git-merge-with-vim/). Eureka! You
get to stay in your familar editor while keeping all of the niceties of a 3-way
diff!

## A little nicer via git mergetool

I have one more step to add to make the normal workflow a little easier. Git
actually gives you the ability to define arbitary merge tool command for use
when resolving merge conflicts, so let's use that plus the power of vim to
change your merge work flow to a simple

    $ git merge
    $ git mergetool

### Configuring mergetool

We're goingto use the magic of 
[VIM startup commands](http://vimdoc.sourceforge.net/htmldoc/starting.html) combined with
[git-mergetool](http://www.kernel.org/pub/software/scm/git/docs/v1.6.4.5/git-mergetool.html)
to automate loading the appropriate diffs inside vim with fugitive.vim's 3-way
merging awesomeness.

#### Install fugitive.vim

The [fugitive.vim plugin](https://github.com/tpope/vim-fugitive) gives you
access to git commands and information from inside vim. It has the handy
ability to do a 3-way git-style diff.

Assuming you have [pathogen.vim](https://github.com/tpope/vim-pathogen)
installed (you should), just run:

	$ cd ~/.vim/bundle
	$ git clone git://github.com/tpope/vim-fugitive.git

#### Configure gvim as your mergetool.

	$ git config merge.tool gvim
	$ git config mergetool.gvim.cmd 'gvim "+Gdiff" $MERGED'

## Usage

[Flaviu's post](http://readncode.com/blog/how-to-do-a-git-merge-with-vim/) has
great instructions on usage, but the gist is to use `]c` to navigate to
conflicts and then `:diffget //2` or `:diffget //3` to choose the version to
keep. `:diffupdate` fixes your whitespace issues and then `:only` lets you see
what you've changed before you save.

