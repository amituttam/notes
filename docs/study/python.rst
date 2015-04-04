Python
======

.. contents:: :depth: 3

Common Questions
----------------

How are arguments passed - by reference or by value?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is actually *call by object*, *call by sharing*, or *call by object
reference*, so the answer is neither.

In Python, everything is an object and all variables hold references to
objects. So what's being passed are objects (references) and can only be
changed if they are mutable (lists and dicts). Note that numbers,
strings, and tuples are immutable.

Sum/multiply all the elements in a list
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    # basic
    s = 0
    for x in range(10):
        s += x

    # the right way
    s = sum(range(10))

    # basic
    s = 1
    for x in range(1, 10):
        s *= x

    # the other way
    from operator import mul
    s = reduce(mul, range(1,10))

This brings up the discussion of functional programming concepts in
python. These functions can be used in conjunction with *lambda* instead
of longer for loops.

*map(function, seq)*

Takes a function and applies it to each item in the sequence. The
resulting object is an iterable object. Thus, apply *list()* to the map
object to get a list output. Or loop through it.

.. code-block:: python

    a = map(lambda x: x*2, range(0,10))
    for i in a:
        print(i)

*filter(function, seq)*

Filter extracts elements in the sequence that return *True*. Note that
function can be *None*, and thus it will return items that are *True*.

.. code-block:: python

    a = filter(lambda x: x > 1, range(0,10))
    list(a)

*reduce(function, seq)*

Reduce applies a function of *two* arguments, cumulatively to the items
of a sequence. It returns one value back (the cumulative value).

.. code-block:: python

    from functools import reduce
    a = reduce(lambda x, y: x * y, [1,2,3,4])
    a == 24 # True

Note that that function takes two arguments. The sequence of operations
goes as follows *(((1*2) * 3) * 4)*.

Difference between tuples and list
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Lists are mutable while tuples are not. More importantly, tuples can be
*hashed* (used as keys for dictionaries). Tuples are used if order of
elements in a sequence matters (e.g. coordinates, points of a path,
etc).

.. code-block:: python

    t = ((1,'a'), (2,'b'))
    dict(t)
    # OUT: {1: 'a', 2: 'b'}

    dict((y,x) for x,y in t)
    # OUT: {'b': 2, 'a': 1}

    {y:x for x,y in t}
    # OUT: {'b': 2, 'a': 1}

What are decorators and what is their usage?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Decorators allow you to inject or modify code in functions or clases.
Basically, a wrapper to an existing function. Thus, allows you to
execute a code before or after the original code. For example, logging a
function.

.. code-block:: python

    from __future__ import print_function

    def log(fn):
        def wrapper(*args, **kw):
            res = fn(*args, **kw)
            print("%s(%r) -> %s" % (fn.__name__, args, res))
            return res
        return wrapper

    @log
    def ispal(word):
        if len(word) < 2:
            return True
        return (word[0] == word[-1]) & ispal(word[1:-1])


    ispal("test")
    ispal("kayak")


Common Mistakes
---------------
