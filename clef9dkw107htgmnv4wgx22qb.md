# Formatter for Jupyter notebooks

# Introduction

If you are into Data Science or Machine Learning, you have probably come across jupyter notebooks (.ipynb files). The problem I faced when using jupyter notebooks was that the black formatter didn't work on them. I had tried using the 

```
$ black notebook.ipynb
```

command many times. This article is meant to help with code formatting in Python Notebooks.

___

# nbQA

So, we will be using a python library called nbQA along with code formatters like Black and isort.

## Installation

Install the library using:

```
$ pip install nbqa
```

## Usage

You can use various formatters along with nqba and I will demonstrate how to use a few of them. before trying the formatters, make sure you have installed them already.

### black

Format the notebook using black as shown below:
```
$ nbqa black notebook.ipynb
reformatted notebook.ipynb
All done! ✨ 🍰 ✨
1 files reformatted.
```

### isort

Similarly, format the notebook using isort:
```
$ nbqa isort notebook.ipynb
Fixing notebook.ipynb
```

### yapf
```
$ nbqa yapf --in-place notebook.ipynb
```

### autopep8
```
$ nbqa autopep8 -i notebook.ipynb
```

### mdformat

To format the markdown cells in your notebook, use:
```
$ nbqa mdformat notebook.ipynb --nbqa-md --nbqa-diff
```

### doctest

To run tests for iPython notebooks using doctypes:
```
$ nbqa doctest notebook.ipynb
```

___

I hope you liked it. That's all for this time.
![The end](https://cdn.hashnode.com/res/hashnode/image/upload/v1677045136672/01050d6a-43a1-4392-a27c-a49d35ec583d.gif)