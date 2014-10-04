Chapter 4: Modularity
=====================

#. In the beginning, everything was one big lump of machine code. The
   earliest procedural languages brought in the notion of partition by
   subroutine. Then we invented service libraries to share common
   utility functions among multiple programs. Next, we invented
   separated address spaces and communicating processes. Today we
   routinely distribute program systems across multiple hosts separated
   by thousands of miles of network cable.

#. Modularity of hardware has of course been one of the foundations of
   engineering since the adoption of standard screw threads in the late
   1800s.

#. The only way to write complex software that won't fall on its face is
   to build it out of simple modules connected by well-defined
   interfaces, so that most problems are local and you can have some
   hope of fixing or optimizing a part without breaking the whole.

#. Dennis Ritchie encouraged modularity by telling all and sundry that
   function calls were really, really cheap in C. However, this wasn't
   really the case at first but Dennis tricked everyone! However,
   everyone was hooked.

#. The first and most important quality of modular code is
   encapsulation. Well-encapsulated modules don't expose their internals
   to each other. They don't call into the middle of each others'
   implementations, and they don't promiscuously share global data. They
   communicate using application programming interfaces (APIs) — narrow,
   well-defined sets of procedure calls and data structures. This is
   what the Rule of Modularity is about.

#. The APIs enforce a strong isolation and also it defines what the
   architecture of the program is.

#. One good test for whether an API is well designed is this one: if you
   try to write a description of it in purely human language (with no
   source-code extracts allowed), does it make sense?

#. Some of the most able developers start by defining their interfaces,
   writing brief comments to describe them, and then writing the code —
   since the process of writing the comment clarifies what the code must
   do. Such descriptions help you organize your thoughts, they make
   useful module comments, and eventually you might want to turn them
   into a roadmap document for future readers of the code.

#. There has to be a trade-off. Can't really have super small modules or
   very large modules. There has to be a balance.

#. **Brooks's Law predicts that adding programmers to a late project
   makes it later. More generally, it predicts that costs and error
   rates rise as the square of the number of programmers on a project.**

#. In nonmathematical terms, Hatton's empirical results imply a sweet
   spot between 200 and 400 logical lines of code that minimizes
   probable defect density, all other factors (such as programmer skill)
   being equal.

Compactness
-----------

#. Compactness is the property that a design can fit inside a human
   being's head. A good practical test for compactness is this: Does an
   experienced user normally need a manual? If not, then the design (or
   at least the subset of it that covers normal use) is compact. Idea is
   compact tools make you productive.

#. Apparently Lisp language is compact but has difficult concepts. But
   once the user masters these concepts, the concepts become simple.

#. The Unix system call API is semi-compact, but the standard C library
   is not compact in any sense.

#. The Magical Number Seven, Plus or Minus Two: Some Limits on Our
   Capacity for Processing Information [Miller] is one of the foundation
   papers in cognitive psychology (and, incidentally, the specific
   reason that U.S. local telephone numbers have seven digits). It
   showed that the number of discrete items of information human beings
   can hold in short-term memory is seven, plus or minus two. This gives
   us a good rule of thumb for evaluating the compactness of APIs: Does
   a programmer have to remember more than seven entry points? Anything
   larger than this is unlikely to be strictly compact.

#. Among general-purpose programming languages, C and Python are
   semi-compact; Perl, Java, Emacs Lisp, and shell are not (especially
   since serious shell programming requires you to know half-a-dozen
   other tools like sed(1) and awk(1)). C++ is anti-compact — the
   language's designer has admitted that he doesn't expect any one
   programmer to ever understand it all.

#. Sometimes, can't really make programs compact but this has to be last
   choice. An example is BSD sockets API. Can't really make this compact
   because of the complexity of the problem it is trying to solve.

Orthogonality
-------------

#. Orthogonality is one of the most important properties that can help
   make even complex designs compact. In a purely orthogonal design,
   operations do not have side effects; each action (whether it's an API
   call, a macro invocation, or a language operation) changes just one
   thing without affecting others. There is one and only one way to
   change each property of whatever system you are controlling.

#. One common class of design mistake, for example, occurs in code that
   reads and parses data from one (source) format to another (target)
   format. **A designer who thinks of the source format as always being
   stored in a disk file may write the conversion function to open and
   read from a named file. Usually the input could just as well have
   been any file handle.** If the conversion routine were designed
   orthogonally, e.g., without the side effect of opening a file, it
   could save work later when the conversion has to be done on a data
   stream supplied from standard input, a network socket, or any other
   source.

#. There is an excellent discussion of orthogonality and how to achieve
   it in The Pragmatic Programmer [Hunt-Thomas]. As they point out,
   orthogonality reduces test and development time, because it's easier
   to verify code that neither causes side effects nor depends on side
   effects from other code.

#. The concept of refactoring, which first emerged as an explicit idea
   from the ‘Extreme Programming’ school, is closely related to
   orthogonality. To refactor code is to change its structure and
   organization without changing its observable behavior. 

#. The basic Unix APIs were designed for orthogonality with imperfect but
   considerable success. We take for granted being able to open a file
   for write access without exclusive-locking it for write, for example.

#. There are large non-orthogonal patches like the BSD sockets API and
   very large ones like the X windowing system's drawing libraries.

The SPOT Rule
-------------

#. Coined by Brian Kernighan: *Single Point of Truth*

#. Constants, tables, and metadata should be declared and initialized
   once and imported elsewhere. Any time you see duplicate code, that's
   a danger sign. Complexity is a cost; don't pay it twice.

#. Use tools such as code generators to generate common data that is
   being represented in multiple places. Have the data in one place and
   use the tool to generate the representations in different places.

#. If documentation duplicates what you say in the code, use a document
   generator that generates the docs from your code comments.

#. Try and generate header files and interface declarations
   automatically.

#. *No junk, No confusion*. Data structures should be designed to fit
   one representation of data well. Don't make it so generic. Try to
   also make data structure represent the real thing you are trying to
   model.

#. This is an often-overlooked strength of the Unix tradition. Many of
   its most effective tools are thin wrappers around a direct
   translation of some single powerful algorithm.

#. Doug McIlroy: By virtue of a mathematical model and a solid
   algorithm, Unix diff contrasts markedly with its imitators. First,
   the central engine is solid, small, and has never needed one line of
   maintenance. Second, the results are clear and consistent, unmarred
   by surprises where heuristics fail.

#. Other examples, are *grep* which is a thing wrapper around a formal
   algebra of regexs. *yacc* is based on LR-1 grammars at its core.

#. The opposite of a formal approach is using heuristics—rules of thumb
   leading toward a solution that is probabilistically, but not
   certainly, correct.

#. Sometimes, can't avoid designing using heuristics. Mail spam
   filtering uses heuristics since there really isn't a mathematical
   model describing spam.

#. Virtual memory management is also built on heuristics.

#. *"...constraint has encouraged not only economy, but also a certain
   elegance of design"*. That simplicity came from trying to think not
   about how much a language or operating system could do, but of how
   little it could do — not by carrying assumptions but by starting from
   zero (what in Zen is called “beginner's mind” or “empty mind”).

