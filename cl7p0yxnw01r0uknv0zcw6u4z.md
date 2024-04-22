---
title: "Using if __name__ == '__main__'"
seoTitle: "if __name__ == '__main__': in python"
seoDescription: "What is the use of the famous __name__ == '__main__' snippet in python? Find out..."
datePublished: Mon Sep 05 2022 17:16:46 GMT+0000 (Coordinated Universal Time)
cuid: cl7p0yxnw01r0uknv0zcw6u4z
slug: if-name-main
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1662398433346/MLY7pPibm.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1662398194266/QAMqVE4oe.png
tags: python, python3, best-practices

---

Many of us have seen the:
```python
import something

def function():
    pass

if __name__ == '__main__':
    # function calls here...
    pass
```
in one documentation or the other, but what is it?

- [`__main__`](https://docs.python.org/3/reference/toplevel_components.html?highlight=__main__#complete-python-programs)
- [`'__name__'`](https://docs.python.org/3/reference/import.html?highlight=__name__#__name__)

# Short Answer

It protects users from accidentally invoking the script when they didn't intend to. It also helps the user identify whether the file has to be executed or not. The lack of it will make identification harder for the user.

# Long Answer

In a large process, there are 2 types of files:
1. Files that contain function definitions.
2. Files that call those functions and display output.

**Files of type 1 do not need to be executed** since they won't display output and simply define the functions and classes and constants used.

On the other hand, **files of type 2 need to be executed** since they display and output. To help the user identify that this file has to be execute, one uses `if __name__ == '__main__'`

When the interpreter runs a module (that is the main program), the `__name__` variable will have the value  `__main__`. 

```python
print(f"__name__ = {__name__}")    # __name__ = '__main__'
```

But If the code is importing from another module, then the `__name__`  variable will be set to that module’s name.

```python
import something

print(f"__name__ = {__name__}")    # __name__ = 'something'
```

# Conclusion

 We can use an `if __name__ == "__main__"` block to allow or prevent parts of code from being run when the modules are imported.

![ending gif](https://upload.wikimedia.org/wikipedia/commons/e/ea/Thats_all_folks.svg)