---
layout: post
title: ignition-api-stubs and the path forward
tags:
- announcement
- ignition
- ignition-api
- pypi
- python3
- release
- stubs
date: 2023-02-04 12:27 -0800
---
We are announcing that `ignition-api-stubs` 8.1.24 will be the last release officially supporting Python 3.6. Starting with version 8.1.25 our stubs package can be installed using Python 3.7 through 3.10.

## Why drop Python 3.6 while we still support Python 2.7?

We understand this may sound odd considering we are actively supporting Python 2.7, but we are planning on reducing technical debt on the tools we rely for running our tests; specifically `tox`.

## Why not Python 3.11?

Since we run `mypy` against Python 2 codebase we must depend on `mypy[python2]==0.971`. This particular version (the last to support Python 2) depends on `typed_ast`, which fails to install on Python 3.11. For more details, please read [`typed_ast#179`](https://github.com/python/typed_ast/issues/179).

## The path forward

We strongly suggest our users to switch to Python 3.10 at their convenience, but rest assured we will continue supporting Python 3.7 until further notice.

Thanks for reading.
