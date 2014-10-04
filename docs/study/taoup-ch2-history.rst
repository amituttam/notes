Chapter 2: Origins and History of Unix
======================================

#. Multics designed for time-sharing mainframe systems. Was too
   complicated and thus Unix was born from the ashes of Multics. The
   design was to not repeat smae mistakes and thus much simpler design.

#. Invented by Ken Thompson from Bell Laboratories. Partnered with
   Dennis Ritchie (labeled co-inventor) and creator of C programming
   language. This was around 1969-1970.

#. Developed on PDP-7.

#. Unix was originally called "UNICS" (UNiplexed Information and
   Computing Service).

#. Multics had thousand of pages of specs while Unix was designed and
   programmed originally by three people (third person was Doug
   McIlroy).

#. Unix was originally written in assembler and an interpreted language
   called *B*. But *B* was not powerful enough to do systems
   programming, thus Ritchie added data types and structure. Thus, *C*
   was born in 1971.

#. In 1973, Unix was re-written completely in C to achieve better
   performance.

#. Ritchie and Thompson wrote: **"constraint has encouraged not only
   economy, but also a certain elegance of design"**.

#. Unix was given to corporations, academia, etc. The improvements were
   shared back to Bell Labs and Version 7 of Unix was released by Bell
   Labs in late 70s included all these improvements.

#. Ken Thompson ended up teaching at Berkley during 75-76 sabbatical and
   further influenced an already strong influence of Unix research at Berkley.
   First BSD release was in 77 and a lot of great software came out of
   Berkley labs (including *vi* editor). Bill Joy was a grad student
   heading the labs at Berkley that released the BSDs.

#. DARPA chose Berkley Unix as a platform to implement its brand new
   TCP/IP protocol stack (which was running on VAX at the time). First
   released in 83 with Berkley 4.2 Unix.

#. 1981 Microsoft partnered with IBM and marketed MS-DOS (re-packaged
   QDOS "Quick and Dirty OS".

#. 1982 Bill Joy founded Sun Microsystems with two others. Found the
   workstation industry by building together hardware designed from
   Stanford and OS from BSD.

#. DEC cancelled successor to PDP-10 and VAXs running Unix were powering
   Internet backbone (until being displaced by Sun Microsystems). When
   DEC cancelled PDP-10's successor, MIT AI Lab's PDP-10 hacker named
   **Richard Stallman** became motivated to build a completely free
   clone of Unix called GNU.

#. Productization of Unix happened and there were many commercial
   versions. AT&T marketed System V licenses around 1983 when
   anti-trust department broke them up again. This destroyed free
   exchange of source code.

#. Unix became heavily fragmented, which each commercialized version
   marketing their differences. Also, Unix players ignored Microsoft's
   rise in the commercial personal computer market.

#. Rivalry between System V and BSD - sockets vs streams. Corporations
   sided with AT&T System V while programmers and hackers backed BSD.

#. Around the early 80s, a programmer/linguist named Larry Wall quietly
   invented the **patch** utility. Huge impact on Unix development. Now,
   programmers could send *diffs* instead of whole files.

#. When Intel shipped the first 386 chip in 1985, it could address 4GB
   of memory and was powerful enough to run Unixes. This started the end
   of workstation companies such as Sun Microsystems.

#. 1985 was also the year Stallman published GNU Manifesto and X window
   was released with full source code under the X permissive license.
   Which resulted in X becoming de-facto graphics engine in all
   Unixes.

#. Unix standardization started in 1983 with System V and BSD started
   reconciling their APIs. This became officially the POSIX standard in
   1985. Used superior Berkley job control and signal handling with
   System V terminal handling. Only major Unix API to come after was
   Berkley **sockets**.

#. Larry Wall created Perl in 1986, first and most widely used
   open-source scripting language. 1987 first version of GCC is
   released. Thus, GNU now had compiler, editor, debugger, and other
   basic tools to arm next gen developers in the 90s (along with almost
   all workstations running X).

#. While Unix wars were going on, Microsoft released Windows 3.0 in 1990
   and sealed its dominance in the personal computer market.

#. Unix hackers preferred Motorola's elegant RISC based 68000 processor
   compared to Intel's ugly 8086 arch. But Motorola lost out to Intel's
   inexpensive chips. Also, GNU failed to release a free Unix clone by
   this time.

#. Finally in 1991 Linus Torvalds a grad student from Finland announced
   Linux. He wanted a free and cheap clone of Unix running on 386
   hardware. There was 386BSD that started in 90s but was not shipped
   until 92.

#. 1993-1994 Internet exploded and so did development on Linux and BSD.
   BSD suffered because AT&T started lawsuits alleging copied source
   code. Thus, motivated some BSD developers to jump and develop for
   Linux.

#. XFree86 used Internel development and was more effective than X
   consortium and provided BSD and Linux with graphics engine in 1992.

#. Internet Engineering Task Force (IETF) and originated the tradition
   of standardization through Requests For Comment (RFCs) started from
   same group of *hackers* who managed ARPANET at MIT AI Lab.

#. Interesting was that these *geeks* were not Unix programmers. Early
   Unix programmers were from academia and corporations that were
   directly involved in Unix. However, these *geeks* were young and
   bright and sharing through the Internet was their *religion*.

#. Eventually, ARPANET hackers learned Unix and C and Unix hackers
   learned TCP/IP.

#. RMS created *Free Software* term that labeled the goal of a lot of
   *hackers*. However, not all hackers believed in it. BSD license
   remained popular as well.

#. Most hackers did not want to get into the GPL/anti-GPL debate and
   just wrote code. Linus Torvalds used this effectively and licensed
   his kernel with GPL to protect it and used the mature GNU user land
   tools. He avoided the *religious* aspects of the GPL and did not like
   the strong ideology behind it. He sometimes even used proprietary
   programs when there was no better Free Software alternative. This
   made hackers follow his ideology more.

#. Around 1993-1997, Linux already had a strong technical foundation and
   also had distributions, support services, and strong development
   community.

#. When Linux 0.1 was released in 1995, it could beat proprietary Unixes
   in performance and uptime. At this time *Apache* webserver was
   released for Linux and immediately was the most popular web server
   due to its stability and free nature. This cemented Linux as a server
   platform.

#. According to Eric S. Raymond (author of *TAOUP*) **"Given a
   sufficiently large number of eyeballs, all bugs are shallow"**. Thus,
   the argument changed from **Free software because all software should
   be free** to **ree software because it works better**.

#. The paper's contrast between *cathedral* (centralized, closed,
   controlled, secretive) and *bazaar* (decentralized, open,
   peer-review-intensive) modes of development became a central metaphor
   in the new thinking.

#. Early 1998, Netscape inspired by the new thinking released Mozilla
   browser as open source. This brought Linux to Wall Street with the
   tech boom.

#. In March of 1998 an unprecedented summit meeting of community
   influence leaders representing almost all of the major tribes
   convened to consider common goals and tactics. That meeting adopted a
   new label for the common development method of all the factions: open
   source.

#. Most Unix hackers and tribes adopted this new *open source* banner.
   However, the major standout was RMS. He specifically did not want
   *Free Software* = *Open Source*. He claimed and ideological
   difference.

#. The other main intention behind *open source* was to present the
   hacker community's method to the rest of the world. And this was a
   success.

#. The largest-scale pattern in the history of Unix is this: when and
   where Unix has adhered most closely to open-source practices, it has
   prospered. Attempts to proprietarize it have invariably resulted in
   stagnation and decline.

#. The lesson for the future is that over-committing to any one
   technology or business model would be a mistake — and maintaining the
   adaptive flexibility of our software and the design tradition that
   goes with it is correspondingly imperative.

#. Never bet against cheap-plastic and commodity solutions in Economy.
   They always win. This is how Linux has thrived.
