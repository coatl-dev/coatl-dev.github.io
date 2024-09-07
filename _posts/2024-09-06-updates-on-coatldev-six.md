---
layout: post
title: Updates on coatldev/six
subtitle: Focusing on Python 3.12
tags:
- announcement
- continuous integration
- docker
- python2
- python3
date: 2024-09-06 11:00 -0700
last-updated: 2022-09-06 21:17 -0700
---
Approximately six months ago, we introduced our official [Docker image for CI]({{ site.url }}/news/2024/03/23/announcing-our-docker-image-for-ci/). However, a major change was made three months ago that significantly impacts our setup. Let’s dive into the details!

## What happened?

Before June 17, 2024, our Docker image supported Python versions ranging from 3.8 to 3.13 (including alpha releases). However, with the merge of PR [python/cpythons#117412](https://github.com/python/cpython/pull/117412), a change was introduced in Python 3.13.0a6 that affected the installation of `mypy[python2]==0.971`. This was anticipated, as the maintainers of typed_ast had previously announced it would no longer be maintained (see [python/typed_ast#179](https://github.com/python/typed_ast/issues/179)).

Trying to install it results in the following error:

```sh
$ python -VV
Python 3.13.0a6 (main, Apr 24 2024, 06:52:02) [GCC 12.2.0]
$ python -m pip install 'mypy[python2]==0.971'
Collecting mypy==0.971 (from mypy[python2]==0.971)
  Downloading mypy-0.971-py3-none-any.whl.metadata (1.8 kB)
Collecting typing-extensions>=3.10 (from mypy==0.971->mypy[python2]==0.971)
  Downloading typing_extensions-4.12.2-py3-none-any.whl.metadata (3.0 kB)
Collecting mypy-extensions>=0.4.3 (from mypy==0.971->mypy[python2]==0.971)
  Downloading mypy_extensions-1.0.0-py3-none-any.whl.metadata (1.1 kB)
Collecting typed-ast<2,>=1.4.0 (from mypy[python2]==0.971)
  Downloading typed_ast-1.5.5.tar.gz (252 kB)
  Preparing metadata (setup.py) ... done
Downloading mypy-0.971-py3-none-any.whl (2.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.6/2.6 MB 17.1 MB/s eta 0:00:00
Downloading mypy_extensions-1.0.0-py3-none-any.whl (4.7 kB)
Downloading typing_extensions-4.12.2-py3-none-any.whl (37 kB)
Building wheels for collected packages: typed-ast
  Building wheel for typed-ast (setup.py) ... error
  error: subprocess-exited-with-error

  × python setup.py bdist_wheel did not run successfully.
  │ exit code: 1
  ╰─> [30 lines of output]
      running bdist_wheel
      running build
      running build_py
      creating build
      creating build/lib.linux-x86_64-cpython-313
      creating build/lib.linux-x86_64-cpython-313/typed_ast
      copying typed_ast/ast27.py -> build/lib.linux-x86_64-cpython-313/typed_ast
      copying typed_ast/conversions.py -> build/lib.linux-x86_64-cpython-313/typed_ast
      copying typed_ast/ast3.py -> build/lib.linux-x86_64-cpython-313/typed_ast
      copying typed_ast/__init__.py -> build/lib.linux-x86_64-cpython-313/typed_ast
      creating build/lib.linux-x86_64-cpython-313/typed_ast/tests
      copying ast3/tests/test_basics.py -> build/lib.linux-x86_64-cpython-313/typed_ast/tests
      running build_ext
      building '_ast27' extension
      creating build/temp.linux-x86_64-cpython-313
      creating build/temp.linux-x86_64-cpython-313/ast27
      creating build/temp.linux-x86_64-cpython-313/ast27/Custom
      creating build/temp.linux-x86_64-cpython-313/ast27/Parser
      creating build/temp.linux-x86_64-cpython-313/ast27/Python
      gcc -fno-strict-overflow -Wsign-compare -DNDEBUG -g -O3 -Wall -fPIC -Iast27/Include -I/usr/local/include/python3.13 -c ast27/Custom/typed_ast.c -o build/temp.linux-x86_64-cpython-313/ast27/Custom/typed_ast.o
      In file included from /usr/local/include/python3.13/pyport.h:358,
                       from /usr/local/include/python3.13/Python.h:50,
                       from ast27/Custom/typed_ast.c:1:
      ast27/Custom/../Include/compile.h:12:12: error: unknown type name ‘PyFutureFeatures’
         12 | PyAPI_FUNC(PyFutureFeatures *) PyFuture_FromAST(struct _mod *, const char *);
            |            ^~~~~~~~~~~~~~~~
      /usr/local/include/python3.13/exports.h:94:53: note: in definition of macro ‘PyAPI_FUNC’
         94 | #       define PyAPI_FUNC(RTYPE) Py_EXPORTED_SYMBOL RTYPE
            |                                                     ^~~~~
      error: command '/usr/bin/gcc' failed with exit code 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for typed-ast
  Running setup.py clean for typed-ast
Failed to build typed-ast
ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (typed-ast)
```

## What's next?

In response, we have updated our image by dropping support for Python versions 3.8 through 3.11. Moving forward, we will focus exclusively on Python 3.12 to ensure a more streamlined and reliable CI process.

To reassure our users, Python 3.12 will be actively supported until [October 2028](https://peps.python.org/pep-0693/#lifespan), giving us and our users four years of stability before its end-of-support. This provides ample time to plan for future transitions and upgrades.

Stay tuned for more updates!

## One more thing...

We not only publish to [Docker Hub](https://docs.docker.com/docker-hub/), but also you can find us on [Red Hat Quay](https://www.redhat.com/en/technologies/cloud-computing/quay), where we publish images for both `linux/amd64` and `linux/arm64` platforms.

Pull the container or use it as a base image from either:

```sh
docker pull coatldev/six
```

or

```sh
docker pull quay.io/coatldev/six
```

Thanks for reading.
