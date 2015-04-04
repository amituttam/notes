Python
======

.. contents:: :depth: 3

Common Questions
----------------

Difference between *class A(object):* and *class A:*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Subclassing *object* yields a new-style class (in Python 3, *class A:*
defaults to new style). Some differences:

#. Method Resolution Order (MRO) defined by *__mro__* attribute of
   class, defines how inheritance hierarchies are walked. Before, it was
   depth first. Now, it is more sane and is based on *__mro__*.

#. The *__new__* constructor is added. This allows class to act as
   factory method, rather than return new instance of class. Useful for
   returning particular subclasses, or reusing immutable objects rather
   than creating new ones without having to change the creation
   interface.

#. Descriptors. These are the feature behind such things as properties,
   classmethods, staticmethods etc. Essentially, they provide a way to
   control what happens when you access or set a particular attribute on
   a (new style) class.

.. code-block:: python

    class D(object):
        pass

    class E:
        pass

    dir(D)
    # ['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__' , '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__wea kref__']

    dir(E)
    # ['__doc__', '__module__']

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

Misusing expressions as defaults for function arguments
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    def foo(bar=[]):
        bar.append("paz")
        return bar

#. Expect to return *paz* everytime *foo()* is called. But this is not
   the case.

#. After calling *foo()* three times, you will get *["baz", "baz", "baz"]*

#. This is because, the the default value for a function argument is
   only evaluated once, at the time that the function is defined.

#. To get around it:

.. code-block:: python

    def foo(bar=None):
        if bar == None:
            bar = []
        bar.append("paz")
        return bar

