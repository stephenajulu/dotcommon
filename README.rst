dotcommon is a crawler that is built to answer questions
*What are the most popular Bash aliases?*,
*What are the most popular Vundle plugins for Vim?*, etc.
It crawls GitHub repos that match topic ``dotfiles`` and counts such things.

.. contents:: Navigation:
   :backlinks: none

Data
====

All of the below was generated by crawling first 500 results of
GitHub search ``topic:dotfiles``.

Vim
---

``vimrc`` was found in 165 repos

Most common set statements
~~~~~~~~~~~~~~~~~~~~~~~~~~

====================  ===
``set incsearch``     114
``set hlsearch``      110
``set ignorecase``    108
``set expandtab``     104
``set nocompatible``  103
``set laststatus=2``  103
``set autoindent``    103
``set number``        101
``set smartcase``     95
``set showcmd``       83
====================  ===

Try it yourself
===============

Clone the repository. The only external dependency is PyGithub_.

``dotcommon.py`` is an executable that imports dotcommon modules
and brings you into Python REPL. First of all, you need to generate
an access token for GitHub and create an instance of ``Github`` class:

.. code-block:: python

    g = Github("your_token_here")

The primary operation is *counting atoms* where an *atom* is an alias,
a readline macro, an import statement, etc.

You may use any of the existing presets to generate an instance
of ``collections.Counter`` which will contain counted atoms.

.. code-block:: python

    counter = crawler.count_atoms(g, *presets.vim_set_statements)

Writing custom atomizers
------------------------

A preset is a tuple of a sequence of paths where to look for and an atomizer.
An atomizer is a function that takes text and returns a sequence of atoms.
For example, if we want to get the most commonly exported variables in bashrc:

.. code-block:: python

    def atomize(text):
        for line in text.splitlines():
            if line.split()[0] == "export":
                yield line

    bashrc_paths = (".bashrc", "bashrc")

    counter = crawler.count_atoms(g, atomize, bashrc_paths)
    for export, count in counter.most_common(10):
        print(export, count)

.. LINKS
.. _PyGithub: https://github.com/PyGithub/PyGithub