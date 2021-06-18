---
layout: post
title: Spring cleaning
subtitle: and the future of Ignition formulae
tags:
- announcement
- homebrew
- ignition
- scada
date: 2021-06-18 15:47 -0700
---
Just a few days before [spring](https://en.wikipedia.org/wiki/Spring_(season)){:targe="blank"} comes to an end, we at coatl.dev have done our part in cleaning our house. So after considering multiple factors we have decided to retire all Edge formulae, as well as all versioned formulae, and maintain only three Ignition formulae.

We have been paying close attention to analytics provided by Homebrew on install events for the past [30](https://formulae.brew.sh/analytics/install/30d/){:targe="blank"}, [90](https://formulae.brew.sh/analytics/install/90d/){:targe="blank"}, and [365](https://formulae.brew.sh/analytics/install/365d/){:targe="blank"} days, and have never seen an Edge version install event, nor any other formulae apart from `ignition`, `ignition@8.1.1`, and `jython@2.7.1`.

Instead we have created two new formulae, which are [`ignition@7.9`](https://formulae.coatl.dev/formula/ignition@7.9){:targe="blank"}, and [`ignition@8.0`](https://formulae.coatl.dev/formula/ignition@8.0){:targe="blank"}. These formulae will work the same way as [`ignition`](https://formulae.coatl.dev/formula/ignition){:targe="blank"} works; providing a way to keep up to date with the versions currently supported by Inductive Automation.

In case you need a guide on how to install `ignition@7.9` or `ignition@8.0`, please refer the [this post]({{ site.baseurl }}/blog/2021/03/04/stay-up-to-date-with-the-ignition-formula/), just replace all commands with the version you intend to install.

On top of that, all Ignition formulae include a [`livecheck`](https://docs.brew.sh/Brew-Livecheck){:targe="blank"} block which will serve as a way to check if your installation is up to date with the latest release from Inductive Automation.

If you believe we are missing a formula or want to leave a comment, please drop by our [Discussions](https://github.com/coatl-dev/discussions/discussions){:targe="blank"}.

Thanks for reading.