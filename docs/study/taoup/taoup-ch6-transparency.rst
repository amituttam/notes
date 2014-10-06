Chapter 6: Transparency
=======================

.. contents:: :depth: 2

#. Elegant code is not only correct but visibly, transparently correct.
   It does not merely communicate an algorithm to a computer, but also
   conveys insight and assurance to the mind of a human that reads it.

#. Emacs Lisp libraries are discoverable but not transparent. Which
   means it is easy to modify a certain part but difficult to comprehend
   whole system. Linux kernel on the otherhand is very transparent but
   not easily discoverable since it is easy to understand its
   organization but hard to modify a certain piece of the code.

#. *"Discoverability is about reducing barriers to entry; transparency
   is about reducing the cost of living in the code"*.

#. *fetchmail -v* outputs detailed protocol exhanges (IMAP, SMTP, or
   POP3). And thus makes this discoverable.

#. GCC is transparent.

   * GCC is organized as a sequence of processing stages knit together
     by a driver program. The stages are: preprocessor, parser, code
     generator, assembler, and linker.

   * The first three stages are all textual and thus easy to debug.

#. Transparency in UI code is when the UI does not hide too much from
   the user. For example, *kmail* shows the SMTP transactions in its
   status bar. Thus, can easily be debugged if there are problems or
   failures. It is not loud thus does not bother simple users.

#. Terminfo uses the file system itself as a simple hierarchical
   database. This is a superb bit of constructive laziness, obeying the
   Rule of Economy and the Rule of Transparency. It means that all the
   ordinary tools for navigating, examining and modifying the file
   system can be used to navigate, examine, and modify the terminfo
   database;

#. If you want transparent code, the most effective route is simply not
   to layer too much abstraction over what you are manipulating with the
   code.

#. Static depth of a procedural-call hierarchy should really not be more
   than four i.e. to accomplish a certain procedure if the function you
   call is more than four deep, might need to re-think the design.

#. If there is a single global structure that reflects system state,
   make sure it is easily explorable.

#. For the reasons stated above, client authentication is rarely used
   with TLS.  A common technique is to use TLS to authenticate the
   server to the client and to establish a private channel, and for the
   client to authenticate to the server using some other means - for
   example, a username and password using HTTP basic or digest
   authentication.

#. Some programs such as *sng* take a non-transparent format *png* and
   convert it back and forth to a transparent one. This is very
   important in debugging.

   * With *sng*, for example, you can convert *png* to text, run some
     scripts through it to add annotations and then convert it back.

#. If the binary object is dynamically generated or very large, then it
   may not be practical or possible to capture all the state with a
   textualizer. In that case, the equivalent task is to write a browser.
   This is used for visualizing databases.

#. Unix programmers learn a tendency to scrap and rebuild rather than
   patching grubby code.

#. One very important practice is an application of the Rule of Clarity:
   choosing simple algorithms. In Chapter 1 we quoted Ken Thompson:
   *When in doubt, use brute force*.

#. It is also important to include hacker guides. Helpful docs on
   helping discoverablity of the code.
