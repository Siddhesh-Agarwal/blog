## print() in python

In this post, I will talk about about the `print()` statement and it's parameters.

So, I was experimenting with the `help()` function in python and tried the following command:

```markup
>>> help(print)
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
```

me, after seeing `file` and `flush` arguments in the output:

![Haha, Meme based on my pain](https://cdn.hashnode.com/res/hashnode/image/upload/v1651330942759/5Uvi_sMeG.gif)

I knew about the `sep` and `end` parameters but not about the other 2 parameters. But what are these **unpopular-parameters-that-online-tutorials-do-not-talk-about**?

Today, I am going to share what they are. But before that, let me explain what the first 2 parameters are.

_____________

- `sep` is the string that is inserted between 2 values. For example:

```py
>>> print(1, 2, 3, sep='-')
1-2-3
>>> print(1, 2, 3) # The default value of "sep" is a space
1 2 3
```

- `end` is the string that is appended after the last value. For example:

```py
print(1, 2, 3, end='-')
print(4, 5, 6) # The default value of "end" is a newline
print("Hello")
```

Will print:

```markup
1 2 3-4 5 6
Hello
```

_____________

Now that I have explained what `sep` and `end` are, let's talk about the 3rd parameter. The 3rd parameter is `file`. 

It is a **file-like object** (stream). The default value of `file` is `sys.stdout` (sys is a built-in module). If you don't pass this argument, it will default to `stdout` and the output will be printed to the **standard output**. It is the terminal where you execute your code. Standard output is full for `stdout` (Python is implemented in C that's the reason we get to see `stdout` in Python).  If you specify a value for `file`, the output will be **printed to that file**. For example:

```py
print(1, 2, 3, sep="\n", file=open("output.txt", "w+"))
```

the `open("output.txt", "w+")` will create a file called `output.txt` (if it doesn't exist already) and write the output to it. So our `output.txt` file will look like this:

```markup
1
2
3
```

This **allows us to write to a file directly without having to convert to a string**. It also allows us to use the `sep` and `end` parameters of the `print()` function without worrying about how to implement them.

_____________

Finally, let's talk about the `flush` parameter. The `flush` parameter is a boolean value. It "flushes" the internal buffer/stream. Let's see a small example for better understanding:

```py
from time import sleep

# output is not flushed here
print("Hello", end=', ')
sleep(5)
print("world!")
```

would result in:

```markup
Hello, world!
```

The output looks perfect but there is the problem: **the 5-second pause** that was supposed to happen between the 2 words! It is not there. Now we run the same code but this time, we clear the output stream:

```py
from time import sleep

# output is flushed this time
print("Hello", end=', ' flush=True)
sleep(5)
print("world!")
```

Now, when you run the program "Hello, " will be printed first and then after 5 seconds "world!" will be printed.

_____________

That's all for this time.

![That's all folks!](https://cdn.hashnode.com/res/hashnode/image/upload/v1651330944421/KQbguHSJR.gif)