---
title: "Publishing Packages using Poetry"
datePublished: Sat Feb 03 2024 08:40:05 GMT+0000 (Coordinated Universal Time)
cuid: cls5tq0x400000al49lhdciqr
slug: publishing-packages-using-poetry
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706949488188/bd900968-06b4-4a2d-900a-6cb8baafbb1e.png
tags: python, python3, poetry, pypi

---

Poetry is rapidly gaining recognition as an excellent dependency manager in the Python community. It has risen rapidly to become the dependency manager for various projects across the Python community. But did you know that not only can [poetry](https://python-poetry.org/) be used as a dependency manager but it can also be used for publishing Python packages to [PyPI](https://pypi.org)? I have previously written about publishing to PyPI using the tedious setup.py-bdist-twine method

%[https://blog.siddhesh.tech/packaging-and-publishing-on-pypi] 

## Initiating a project

To start a new project, run the following command:

```bash
poetry new poetry-demo
```

This will create a file `poetry-demo` with the following file structure:

```plaintext
poetry-demo
├── pyproject.toml
├── README.md
├── poetry_demo
│   └── __init__.py
└── tests
    └── __init__.py
```

> **NOTE**: Check the availability of the name on [PyPI](https://pypi.org/)

## Adding Dependencies

To add dependencies to the project, we run the command:

```bash
poetry add <dependency>
```

For example, to add pandas as our dependency, we run the command:

```bash
poetry add pandas
```

If you are adding a dependency for the first time in a project, you will notice a new `poetry.lock` file being generated. It contains dependencies along with their corresponding versions and checksums/hashes. This helps users to always be on the same depency versions. It also results in faster dependency conflict resolution.

> Read more about [poetry](https://python-poetry.org/docs/)

Poetry also allows us to create groups. We can do them something like this:

```bash
poetry add black isort --group dev
```

The installation command will now be:

```bash
pip install "poetry-demo[dev]"
```

## Tweaking `poetry.toml`

The `poetry.toml` file contains metadata similar to the `setup.py` file. We can enter various things like name, version, description, license, authors, readme, repository, keywords and classifiers among other things.

> Read more about the [`poetry.toml` file](https://python-poetry.org/docs/pyproject/)

## Publishing on PyPI

Before publishing our project on PyPI, We need to create a build:

```bash
poetry build
```

This generates a `dist` folder that contains a `targ.gz` and a `whl` file. Poetry works only for vanilla Python builds (no C/C++/Rust builds 😞). Now, to publish these files to PyPI, we run the command:

```bash
poetry publish
```

You will be prompted for your PyPI username and password. Enter them to upload the packages. Instead of running 2 sperate commands, uou could combine the 2 commands and run this instead:

```bash
poetry publish --build
```

And congrats, you published a library using poetry!