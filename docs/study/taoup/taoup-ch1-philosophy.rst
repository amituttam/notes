Chapter 1: Philosophy
=====================

.. toctree::
   :maxdepth: 3

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

#. *Rule of Separation: Separate policy from mechanism; separate
   interfaces from engines.* Another way is to separate your application
   into cooperating front-end and back-end processes communicating
   through a specialized application protocol over sockets; we discuss
   this kind of design in Chapter 5 and Chapter 7. The front end
   implements policy; the back end, mechanism. The global complexity of
   the pair will often be far lower than that of a single-process
   monolith implementing the same functions, reducing your vulnerability
   to bugs and lowering life-cycle costs.

#. *Rule of Representation: Fold knowledge into data, so program logic
   can be stupid and robust.* Data is more tractable than program logic.
   It follows that where you see a choice between complexity in data
   structures and complexity in code, choose the former. More: in
   evolving a design, you should actively seek ways to shift complexity
   from code to data.

#. *Rule of Silence: When a program has nothing surprising to say, it
   should say nothing.* One of Unix's oldest and most persistent design
   rules is that when a program has nothing interesting or surprising to
   say, it should shut up.  Well-behaved Unix programs do their jobs
   unobtrusively, with a minimum of fuss and bother. Silence is golden.

#. *Rule of Optimization: Prototype before polishing. Get it working
   before you optimize it.*

#. *Rule of Extensibility: Design for the future, because it will be
   here sooner than you think.* If it is unwise to trust other people's
   claims for “one true way”, it's even more foolish to believe them
   about your own designs. Never assume you have the final answer.
   Therefore, leave room for your data formats and code to grow;
   otherwise, you will often find that you are locked into unwise early
   choices because you cannot change them while maintaining backward
   compatibility.

#. *To do the Unix philosophy right, you have to be loyal to
   excellence.*
   You have to believe that software design is a craft worth all the
   intelligence, creativity, and passion you can muster. Otherwise you
   won't look past the easy, stereotyped ways of approaching design and
   implementation; you'll rush into coding when you should be thinking.
   You'll carelessly complicate when you should be relentlessly
   simplifying — and then you'll wonder why your code bloats and
   debugging is so hard.

#. Software design and implementation should be a joyous art, a kind of
   high-level play.

   If this attitude seems preposterous or vaguely embarrassing to you,
   stop and think; ask yourself what you've forgotten. Why do you design
   software instead of doing something else to make money or pass the
   time? You must have thought software was worthy of your passion
   once....

   To do the Unix philosophy right, you need to have (or recover) that
   attitude. You need to care. You need to play. You need to be willing
   to explore.
