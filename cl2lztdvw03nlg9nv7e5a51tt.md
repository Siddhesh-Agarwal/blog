## id() in python

In this post, I will try to improve your idea about memory in python using the in-built `id()` function. For those of you who don't know what `id()` is:

>  The `id()` function returns a unique ID of the object. All objects in python have a unique ID and no 2 different values correspond to the same ID.

So let us begin with a small example:

```py
a = b = 500
print(id(a) == id(b))
```

> **Fun fact:** In python, `id(a) == id(b)` is analogous to `a is b`.

The above code prints `True` because python creates the variable `b` with the value `500` and then creates a variable `a` pointing to the value of `b`. This implies that `a` and `b` are pointing towards the same memory location and hence the same ID.

<hr>

Now let's raise the bar:

```py
a = 500
b = 500
print(id(a) == id(b))
```

The above code prints `False` because: 

* Python creates a variable `a` pointing to the value `500` in the memory. 
* Then, it creates another variable `b` pointing to another value `500` (yeah, both 500 are different).

Hence, both have different IDs because both point toward different memory locations.

<hr>

I hope this isn't confusing because there is more to come. Guess the output for this:

```py
a = 50
b = 50
print(id(a) == id(b))
```

Some of you may think _"This is the previous question with different values. I know the answer is `False`"_ but not so fast.

![Not so fast](https://cdn.hashnode.com/res/hashnode/image/upload/v1651330953712/aVy-NUY-O.gif)

For small integers (The CPython range is -5 to 256, both inclusive), then integer objects (`<class 'int'>`) are shared. This is done entirely to save space. The memory imprint of the console would be significantly larger if these objects weren’t sharing their memory.

So the correct answer is `True`

<hr>

Okay, okay. Just one more to go. The last one:

```py
a = 500
id1 = id(a)
a = 500
id2 = id(a)
print(id1 == id2)
```

Well, even though I am re-declaring the same variable with the same value, the answer is **most likely** to be `False`. I'll explain to you why. When you re-declare a variable in python, the interpreter works in the same way as a declaration. i.e. It entirely deletes the previous existing value and creates a variable with the new value. So when we give `a = 500`, the second time the interpreter deletes the previously existing value of `a` and creates a new memory location for `500` where `a` would point towards. Both of these IDs are most likely different.

SO, the answer is `False`.

**NOTE:** If the above example had a number belonging to the inclusive range -5 to 256, the answer would have been `True`. this is because numbers belonging to the inclusive range have a fixed memory location.