---
layout: post
title: New Ignition 8.1.3 formulae now available
tags:
- announcement
- brew
- formulae
- homebrew
- ignition
- scada
date: 2021-03-04 13:51 -0800
---
We are proud to announce three new formulae!

- [ignition](https://formulae.coatl.dev/formula/ignition) (More info [here]({{ site.url }}/blog/2021/03/04/stay-up-to-date-with-the-ignition-formula))
- [ignition-edge@8.1.3](https://formulae.coatl.dev/formula/ignition-edge@8.1.3)
- [ignition@8.1.3](https://formulae.coatl.dev/formula/ignition@8.1.3)

## How do I install these formulae?
To install these formulae, first tap our Homebrew tap:

```bash
$ brew tap coatl-dev/coatl-dev
```

And then, for `ignition-edge@8.1.3`:

```bash
$ brew install ignition-edge@8.1.3
```

Or for `ignition@8.1.3`:

```bash
$ brew install ignition@8.1.3
```

Or for `ignition`:

```bash
$ brew install ignition
```

Or with one single command.

For `ignition-edge@8.1.3`:

```bash
$ brew install coatl-dev/coatl-dev/ignition-edge@8.1.3
```

Or for `ignition@8.1.3`:

```bash
$ brew install coatl-dev/coatl-dev/ignition@8.1.3
```

Or for `ignition`:

```bash
$ brew install coatl-dev/coatl-dev/ignition
```

## How do I start the Ignition Gateway?

{: .box-warning}
Warning: Please make sure you don't have another Ignition Gateway running on port `8088`. If you do, please stop it by running `ignition@X.X.X stop`, `ignition-edge@X.X.X stop` or `ignition stop`.

Once installed, open a new Terminal window and run the following command:

```bash
$ ignition-edge@8.1.3 start
```

Or

```bash
$ ignition@8.1.3 start
```

Or

```bash
$ ignition start
```

Once the Service starts, you can go to http://localhost:8088/ to provision your Gateway.

## How do I start Ignition automatically at login?

Run the following command:

```bash
$ brew services start coatl-dev/coatl-dev/ignition-edge@8.1.3
```

Or

```bash
$ brew services start coatl-dev/coatl-dev/ignition@8.1.3
```

Or

```bash
$ brew services start coatl-dev/coatl-dev/ignition
```

## Discussions

If you run into any issue, feel free to post something at our [Discussions](https://github.com/coatl-dev/discussions/discussions).
