The Art of Unix Programming
===========================

Notes taken from `The Art of Unix Programming <http://www.faqs.org/docs/artu/index.html>`_

#. The Unix API is the closest thing to a hardware-independent standard
   for writing truly portable software that exists. It is no accident
   that what the IEEE originally called the Portable Operating System
   Standard quickly got a suffix added to its acronym and became POSIX.
   A Unix-equivalent API was the only credible model for such a
   standard.

#. Doug McIlroy (invertor of Pipes): This is the Unix philosophy: Write
   programs that do one thing and do it well. Write programs to work
   together. Write programs to handle text streams, because that is a
   universal interface.

#. Rob Pike (Master of C): Data dominates. If you've chosen the right
   data structures and organized things well, the algorithms will almost
   always be self-evident. Data structures, not algorithms, are central
   to programming.

#. Rule of Repair: When you must fail, fail noisily and as soon as
   possible.

#. Rule of Modularity: Write simple parts connected by clean interfaces.
   The only way to write complex software that won't fall on its face is
   to hold its global complexity down — to build it out of simple parts
   connected by well-defined interfaces, so that most problems are local
   and you can have some hope of upgrading a part without breaking the
   whole.
