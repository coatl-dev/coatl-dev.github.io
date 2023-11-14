---
layout: post
title: March 2022 Homebrew Tap updates
tags:
- announcements
- formulae
- homebrew
- ignition
date: 2022-03-07 22:39 -0800
---
## Ignition 8.1.15 release

Inductive Automation released Ignition 8.1.15 on March 1, 2022, and as we typically do we at coatl.dev we updated our [`ignition`](https://formulae.coatl.dev/formula/ignition) formula.

Additionally we did some refactoring, and simplified the `post_install` step of our formula. So instead of manually uncompressing the Java runtime and upgrading the `ignition.conf` file we now rely on two additional commands from the `ignition` executable; `chekruntimes` and `runupgrader`. Both of them were introduced on an earlier release and we now make use of both.

To install or update to the latest Ignition version from our tap, please refer to the instructions found [here]({{ site.url }}/news/2021/03/04/stay-up-to-date-with-the-ignition-formula), which apply to all versions.

You may also find more information about Ignition on [the official announcement](https://inductiveautomation.com/news/ignition-8115-perspective-components-get-a-boost-creating-tags-gets-easier-and-much-more), the [release notes](https://inductiveautomation.com/downloads/releasenotes/8.1.15), or the [docs](https://docs.inductiveautomation.com/display/DOC81/New+in+this+Version#NewinthisVersion-Newin8.1.15).

## Ignition 8.0 deprecation

We have kept the `ignition@8.0` formula in case there are users out there who still rely on this version, but it appears Inductive Automation has abandoned development on it.

With Ignition 8.0.17 being the latest version having a released date of November 12, 2020, we decided to put a deprecation warning for `ignition@8.0` dated `2020-11-12`.

## Upcoming releases

We are expecting version 7.9.19 to be released any time now, so expect us to update our `ignition@7.9` formula soon.

## Help wanted

We believe that there may be some Linux users who rely on Homebrew as another option for installing packages which also happen to be Ignition users. If you fit into that description, we are looking for some help on making `ignition` available for Linux.

Feel free to chime in on our [ignition for Linux](https://github.com/coatl-dev/discussions/discussions/2) discussion.

Thanks for reading!
