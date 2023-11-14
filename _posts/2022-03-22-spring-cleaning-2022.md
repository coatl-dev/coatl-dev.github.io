---
layout: post
title: Spring cleaning 2022
tags:
- announcement
- homebrew
- ignition
- scada
date: 2022-03-22 23:01 -0700
---
It's that time of the year again, and we at `coatl.dev` have done our part.

Here are the most relevant changes:

## `ignition` had some refinements

The [`ignition`](https://formulae.coatl.dev/formula/ignition#default) formula has gone through a bit of a transformation; here's the `git-diff`:

```zsh
$ git diff 9bef009 4852467 ./Formula/ignition.rb
diff --git a/Formula/ignition.rb b/Formula/ignition.rb
index 7ccbe2d..e736653 100644
--- a/Formula/ignition.rb
+++ b/Formula/ignition.rb
@@ -1,9 +1,10 @@
 class Ignition < Formula
   desc "Unlimited Platform for SCADA and so much more"
   homepage "https://inductiveautomation.com/"
-  url "https://files.inductiveautomation.com/release/ia/8.1.14/20220127-1144/Ignition-osx-8.1.14.zip",
+  url "https://files.inductiveautomation.com/release/ia/8.1.15/20220301-1426/Ignition-macOs-x86-64-8.1.15.zip",
       referer: "https://inductiveautomation.com/"
-  sha256 "3c934d571d2295e41268f4432c2ca9eb437d62d113ae7f2fa5dbcd11c08f611e"
+  version "8.1.15"
+  sha256 "24331d78e843421807040e10455f10efd6bdd4f79dee1d603510d370773fd8a0"
   license :cannot_represent

   livecheck do
@@ -50,17 +51,10 @@ def post_install
     end

     # Unzip the new runtime
-    system "tar", "-C", "#{libexec}/lib/runtime", "-xf", "#{libexec}/lib/runtime/jre-mac.tar.gz"
+    system bin/"ignition", "checkruntimes"

     # Update ignition.conf
-    system "#{libexec}/lib/runtime/jre-mac/bin/java",
-           "-classpath",
-           "#{libexec}/lib/core/common/common.jar",
-           "com.inductiveautomation.ignition.common.upgrader.Upgrader",
-           ".",
-           "#{libexec}/data",
-           "#{libexec}/logs",
-           "file=ignition.conf"
+    system bin/"ignition", "runupgrader"
   end

   def caveats
```

Let's break it down into two major changes:

### A `version` stanza has been added

With the release of Ignition 8.1.15, the people at Inductive Automation have granted Apple's wish to use **macOS** instead of *OS X*, but they also decided to include the targetted architecture to the file name; `x86-64`.

As we went through the typical formula upgrade, we noticed that `brew` was "erroneously" detecting the version for 8.1.15 as `64-8` because it was being extracted from the `url`, so the only way we could get the correct version was through adding the `version` stanza:

```ruby
  version "8.1.15"
```

### `post_install` lost some weight

Usually, when Inductive Automation releases a new version for Ignition, we read the [docs](https://docs.inductiveautomation.com/display/DOC81/New+in+this+Version), but for [8.1.10](https://docs.inductiveautomation.com/display/DOC81/New+in+this+Version#NewinthisVersion-Newin8.1.10) we completely missed the announcement regarding a script file called `ignition-util.sh` which we've corrected in version 8.1.15.

The announcement is found at the [8.1.10 Release Notes](https://inductiveautomation.com/downloads/releasenotes/8.1.10) under the **Installers** section:

>Installers
>>Infrastructure
>>>Added ignition.sh runupgrader command as well as run-upgrader.bat on Windows to simplify running Ignition's Upgrader on upgrades.

This file provides two additional commands:

1. `checkruntimes` &rarr; Extracts the Ignition platform runtime if necessary

    On the `8.1.14` formula this used to be:

    ```ruby
        # Unzip the new runtime
        system "tar", "-C", "#{libexec}/lib/runtime", "-xf", "#{libexec}/lib/runtime/jre-mac.tar.gz"
    ```

    And now:

    ```ruby
        # Unzip the new runtime
        system bin/"ignition", "checkruntimes"
    ```

1. `runupgrader` &rarr; Runs the Ignition Upgrader utility

    Before:

    ```ruby
        # Update ignition.conf
        system "#{libexec}/lib/runtime/jre-mac/bin/java",
           "-classpath",
           "#{libexec}/lib/core/common/common.jar",
           "com.inductiveautomation.ignition.common.upgrader.Upgrader",
           ".",
           "#{libexec}/data",
           "#{libexec}/logs",
           "file=ignition.conf"
    ```

    Now:

    ```ruby
        # Update ignition.conf
        system bin/"ignition", "runupgrader"
    ```

### Acknowledgments

We acknowledge a mistake on our behalf as we were using `"."` when it should have been `libexec.to_s`, like this:

```ruby
    system "#{libexec}/lib/runtime/jre-mac/bin/java",
        "-classpath",
        "#{libexec}/lib/core/common/common.jar",
        "com.inductiveautomation.ignition.common.upgrader.Upgrader",
        libexec.to_s,
        "#{libexec}/data",
        "#{libexec}/logs",
        "file=ignition.conf"
```

Fortunately the `ignition` formula has always been on the `8.1` version, something that might have been an issue when upgrading to `8.2`.

## `ignition@8.0` deprecation and cleanup

November 12, 2020 was the *final* release for the `8.0` branch from Inductive Automation, and, as we announced it on our [previous post]({{ site.url }}/news/2022/03/07/march-2022-homebrew-tap-updates/#ignition-80-deprecation), `ignition@8.0` has a deprecation warning.

But that doesn't mean that we've stopped improving our formula. Au contraire, mes amis!

`ignition@8.0` also lost some weight at `post_install`, removing the "Update ignition.conf" and "Unzip the new runtime" steps.

According to `README.txt` instructions, for Ignition 8.0.17, on upgrading an existing installation:

>7) If this is a major version upgrade (8.0 to 8.1) then the data/ignition.conf file may have changed. If this is a minor version upgrade (8.0.1 to 8.0.2) then skip to step 8.

Since this formula will not install a minor version upgrade, only patch upgrades for `8.0`, there is no need to perform these actions.

## Final announcement

Check out the work from the `ignition-api` GitHub org (<https://github.com/ignition-api/>), which is behind the `ignition-api` Python package (<https://pypi.org/project/ignition-api/>). They have releases for `7.9`, `8.0`, and `8.1`.

And that's it!

Thanks for reading!
