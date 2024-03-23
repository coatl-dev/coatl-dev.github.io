---
layout: post
title: Announcing our Docker image for CI
subtitle: Finally
tags:
- announcement
- continuous integration
- docker
- python2
- python3
date: 2024-03-23 15:59 -0700
---
We are thrilled to announce the release of our new Docker image called `six`, designed specifically for developers who are still working on Jython or Python 2.7 projects. This Docker image is tailored for seamless integration into Azure Pipelines and GitHub workflows, making it easier than ever to manage and deploy your legacy projects.

## Key features

- Supports Python 2.7.18 for legacy projects.
- Compatible with Python versions from 3.8 to 3.13 for modern development needs.
- Optimized for Azure Pipelines and GitHub workflows.
- Lightweight and easy to use.

## Why `six`?

We created the `six` Docker image because GitHub dropped Python 2.7 support from their default images back in June 2023 (See: [actions/runner-images#7401](https://github.com/actions/runner-images/issues/7401)). With `six`, you can continue working on your Python 2.7 projects without worrying about compatibility issues in your CI/CD workflows or pipelines.

Naming things is hard, but since we aim to provide support for both Python 2 and 3 we were inspired by the Python `six` package ([six at PyPI](https://pypi.org/project/six/)).

## Get started

The examples below will demonstrate how to use this image in [Azure Pipelines], and [GitHub Workflows].

### Azure Pipelines

```yml
jobs:
  - job: tox

    pool:
      vmImage: ubuntu-latest

    container: coatldev/six:latest

    steps:
      - script: |
          sudo chown -R $(whoami):$(id -ng) "${PYTHON_ROOT}"
        displayName: Change owner

      - script: |
          python -m pip install --upgrade pip tox
        displayName: Install dependencies

      - script: |
          tox
        displayName: Run tests
```

### GitHub Workflows

```yml
jobs:
  tox:

    runs-on: ubuntu-latest

    container: coatldev/six:latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip tox

      - name: Run tests
        run: |
          tox
```

### GitHub  repos using `six`

- [coatl-dev/workflows]
- [jonhadfield/python-hosts]
- [markreidvfx/pyavb]
- [saqimtiaz/SQPL]

### Conclusion

We believe that `six` will be a valuable addition to your development toolkit, offering a seamless and efficient experience for legacy project maintenance. Your feedback is crucial to us, so please don't hesitate to share your thoughts, suggestions, and any issues you encounter while using `six`.

Thank you for choosing `six` for your Jython/Python 2.7 development needs. Happy coding!

<!-- External links -->
[Azure Pipelines]: https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/jobs-job-container?view=azure-pipelines
[GitHub Workflows]: https://docs.github.com/en/actions/using-jobs/running-jobs-in-a-container
[coatl-dev/workflows]: https://github.com/coatl-dev/workflows/blob/coatl/.github/workflows/tox-docker.yml
[jonhadfield/python-hosts]: https://github.com/jonhadfield/python-hosts/blob/9ed99ada371dcbe589f7f6fb5a75e2d70dd2af9c/.github/workflows/ci.yml#L8
[markreidvfx/pyavb]: https://github.com/markreidvfx/pyavb/blob/ff64e06b60077ecabe5db3fef239898b08363898/.github/workflows/workflow.yml#L161
[saqimtiaz/SQPL]: https://github.com/saqimtiaz/SQPL/blob/808a8fa40304d766ee48d49bf231bcb933fc7ef9/.github/workflows/build.yml#L17
