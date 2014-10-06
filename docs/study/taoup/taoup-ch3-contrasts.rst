Chapter 3: Contrasts
====================

.. toctree::
   :maxdepth: 3

#. Different operating systems were designed by the influences of
   culture, limitations (usually economic), and ideas of their
   designers.

#. The designer's idea is usually is baked into the operating system and
   thus unifies its design. For Unix, this idea was *everything is a
   file* and *pipes* metaphor that builds on top of this.

#. To design the perfect anti-Unix, have no unifying idea at all, just
   an incoherent pile of ad-hoc features.

#. One way in which OSes differ is in the way they handle multiple
   processes or *Multitasking*. *DOS* and *CP/M* were basically
   sequential loaders with to multitasking abilities.

#. *Cooperative Multitasking* is the ability to share multiple processes
   with. However, there was no memory management unit or locking. Thus,
   a bug in a program could freeze the entire system.

#. Unix has *preemptive multitasking**, in which timeslices are
   allocated by a scheduler which routinely interrupts or pre-empts the
   running process in order to hand control to the next one. Almost all
   modern operating systems support preemption.

#. Note that *multitasking* does not mean *multiuser*. Many OSes are
   multitasking but can only support one user at a time logged in to the
   machine. True multi user requires multiple user privilege domains
   (multi console).

#. In the Unix experience, **inexpensive process-spawning and easy
   inter-process communication (IPC)** makes a whole ecology of small
   tools, pipes, and filters possible.

#. A subtle but important property of pipes and the other classic Unix
   IPC methods is that they require communication between programs to be
   held down to a level of simplicity that encourages separation of
   function. Conversely, the result of having no equivalent of the pipe
   is that programs can only be designed to cooperate by building in
   full knowledge of each others' internals.

#. In operating systems without flexible IPC and a strong tradition of
   using it, programs communicate by sharing elaborate data structures.

#. *Doug McIlroy*: Word and Excel and PowerPoint and other Microsoft
   programs have intimate — one might say promiscuous — knowledge of
   each others' internals. In Unix, one tries to design programs to
   operate not specifically with each other, but with programs as yet
   unthought of.

#. Unix encourages interal boundaries by encouraging creating different
   users with different privileges. System programs often have their own
   pseudo-user accounts to confer access to special system files without
   requiring unlimited (or superuser) access.

#. Unix has at least three levels of internal boundaries:

   * Unix uses its hardware's memory management unit (MMU) to ensure
     that separate processes are prevented from intruding on the others'
     memory-address spaces.

   * A second is the presence of true privilege groups for multiple
     users — an ordinary (nonroot) user's processes cannot alter or read
     another user's files without permission.

   * A third is the confinement of security-critical functions to the
     smallest possible pieces of trusted code. Under Unix, even the
     shell (the system command interpreter) is not a privileged program.

#. OSes need strong internal boundaries for stability and security.

#. Unix files have neither record structure nor attributes. Other OSes
   know about the file and the type of the file. For example, other
   OSes associate file extension with application to open that file. In
   Unix, applications recognize the files by their *magic number* or
   other data type within the file itself.

#. OS-level record structures are generally an optimization hack, and do
   little more than complicate APIs and programmers' lives. They
   encourage the use of opaque record-oriented file formats that generic
   tools like text editors cannot read properly.

#. Critical data in Unix is stored in text files. Thus, it can easily
   read by programs (not a security risk since critical data such as
   passwords are salted and stored as hashes). Binary data is evil.

#. In Unix, belief is that OS should have strong CLI facilities because
   of the following reasons:

   * Easy remote administration.

   * Programs will not be designed to cooperate with each other in a
     nice way.

   * Servers, daemons, and other background programs will be difficult
     to program.

#. Unix is designed for programmers by programmers. Thus, it makes no
   assumptions on what the user needs or wants. Other OSes designed for
   end users often make these assumptions and sometimes get them wrong.

#. In Unix, there is no major barrier for a user to become a developer.
   The culture promotes it and the development tools are freely
   available for everyone. *Unix* pioneered casual programming.

Operating System Comparisons
----------------------------

VMS
^^^

#. VMS was released by DEC in 1978 and still kind of survives (maybe
   receives support). It is also a CLI based OS.

#. VMS has full preemptive multitasking, but makes process-spawning very
   expensive. The VMS file system has an elaborate notion of record
   types (though not attributes). 

#. Had elaborate COBOL system commands and extensive help system. But
   commands where quite long to type and help system had no good search
   functionality.

#. VMS had MMU and true multiuser capabilities. Security cracks on VMS
   were quite rare.

#. VMS dev tools and docs were expensive. Docs were only available in
   paper form and thus was tiresome to go through and search through.

MacOS
^^^^^

#. Debut in 1984 with the Macintosh and has heavy GUI influenced
   designed obtained from Xerox's Palo Alto Research Center.

#. MacOS's very strong unifying idea was its GUI guidelines. Specified
   in great detail how the application should look like and behave.

#. One key idea of these guidelines was that all the documents,
   directories, and other persistent objects had its place in the
   desktop and desktop context was preserved across reboots.

#. All programs have GUIs. MacOS's captive-interface GUI metaphor
   (organized around a single main event loop) leads to a weak scheduler
   without preemption. The weak scheduler, and the fact that all
   MultiFinder applications run in a single large address space, implies
   that it is not practical to use separated processes or even threads
   rather than polling.

#. MacOS applications are not, however, invariably monster monoliths.
   The system's GUI support code, which is partly implemented in a ROM
   shipped with the hardware and partly implemented in shared libraries,
   communicates with MacOS programs through an event interface that has
   been quite stable since its beginnings. Thus, the design of the
   operating system encourages a relatively clean separation between
   application engine and GUI interface.

#. MacOS files have both a ‘data fork’ (a Unix-style bag of bytes that
   contains a document or program code) and a ‘resource fork’ (a set of
   user-definable file attributes). Mac applications tend to be designed
   so that (for example) the images and sound used in them are stored in
   the resource fork and can be modified separately from the application
   code.

#. The MacOS system of internal boundaries is very weak. There is a
   wired-in assumption that there is but a single user, so there are no
   per-user privilege groups. Multitasking is cooperative, not
   pre-emptive.

#. Security cracks against MacOS machines are very easy to write; the OS
   has been spared an epidemic mainly because very few people are
   motivated to crack it.

#. Mac OS X merged the above ideas with the strong internals of BSD
   Unix. At the same time, leading-edge Unixes such as Linux are
   beginning to borrow ideas like file attributes (a generalization of
   the resource fork) from MacOS.

OS/2
^^^^

#. Currently (2003) still used in some automated teller machines. Never
   really was competition to MacOS or Windows. Was initially designed as
   an *advanced DOS*.

#. OS/2 was designed with preemptive multitasking and thus would not run
   on systems without an MMU. However, it was not designed to be
   multiuser. Also, it allowed for relatively inexpensive process
   spawning but had a difficult IPC.

#. Had networking support for LAN protocols but TCP/IP was later added.

#. Had both CLI/GUI. The OS/2 WPS (Workplace Shell) was its desktop. It
   was licensed from AmigaOS and had strong and clean object-oriented
   design and good extensibility. This would become the model from GNOME
   desktop.

#. OS/2 had the internal boundaries one would expect in a single-user
   OS. Running processes were protected from each other, and kernel
   space was protected from user space, but there were no per-user
   privilege groups. This meant the file system had no protection
   against malicious code. Another consequence was that there was no
   analog of a home directory; application data tended to be scattered
   all over the system.

#. Since there were no per-user privilege group. Trusted programs would
   be jammed into kernel or WPS thus resulting in bloat.

#. Used both text and binary formats.

#. Eventually IBM released tools for free and hobby groups evolved but
   was pushed towards Java because of Microsoft's dominance on the
   desktop. Finally, a lot of devs moved towards Linux.

#. Lesson learned, can't really go too far with multitasking OS with no
   multi-user capabilities.

Windows NT (New Technology)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Designed for high-end personal and server use. All Microsoft's OSes
   from Windows 2000 onwards are NT based.

#. NT genetically descended from VMS. NT grew by accretion (continuous
   growth by adding layers) and doesn't really have a unifying design
   idea like MacOS or Unix.

#. Technology becomes obsolete every few years and devs have to re-learn
   APIs, concepts.

#. Pre-emptive multitasking is supported but process spawning is several
   times more expensive (0.1s) than Unix.

#. Makes extensive use and distinction between binary formats and text
   files.

#. Programs communicate via complex and fragile RPCs.

#. System configuration is stored in registries.

   * The registry makes the system completely non-orthogonal.
     Single-point failures in applications can corrupt the registry,
     frequently making the entire operating system unusable and
     requiring a reinstall.

   * The registry creep phenomenon: as the registry grows, rising access
     costs slow down all programs.

#. NT has weak internal boundaries. Although it has access control
   lists, they are ignored by older programs.

#. To achieve speed, recent versions of the NT wire the webserver into
   the kernel to achieve the same speed as Unix.

#. These holes in the boundaries have the synergistic effect of making
   actual security on NT systems effectively impossible.

#. Because Windows does not handle library versioning properly, it
   suffers from a chronic configuration problem called “DLL hell”, in
   which installing new programs can randomly upgrade (or even
   downgrade!) the libraries on which existing programs depend.

#. Microsoft started to publish all APIs and kept tools inexpensive.
   However, around Windows 95 time frame, they started to hide APIs and
   did not publish internal APIs to the general public. Only devs who
   signed NDAs could use them.

BeOS
^^^^

#. Started out as a hardware vendor building machines around PowerPC
   arch in 1989.

#. BeOS was Be's attempt to add value to the hardware by inventing a
   new, network-ready operating system model incorporating the lessons
   of both Unix and the MacOS family, without being either. The result
   was a tasteful, clean, and exciting design with excellent performance
   in its chosen role as a multimedia platform.

#. BeOS's unifying ideas were ‘pervasive threading’, multimedia flows,
   and the file system as database. Designed also to minimize latency in
   the kernel. BeOS ‘threads’ were actually lightweight processes in
   Unix terminology, since they supported thread-local storage and
   therefore did not necessarily share all address spaces. IPC via
   shared memory was fast and efficient.

#. Followed Unix by having no file structure above byte level but halso
   had file attributes ala MacOS. The filesystem database could be
   indexed by any attribute.

#. One of the things BeOS took from Unix was intelligent design of
   internal boundaries. It made full use of an MMU, and sealed running
   processes off from each other effectively. While it presented as a
   single-user operating system (no login), it supported Unix-like
   privilege groups in the file system and elsewhere in the OS
   internals. Easy to add multi-user capability. There was a guest user
   (default) and a root user.

#. BeOS tended to use binary file formats and the native database built
   into the file system, rather than Unix-like textual formats.

#. Had clean GUI but also good CLI (port of bash). Had a POSIX
   compatibility layer as well.

#. Was designed as a multimedia workstation. Followed Apple in only
   allowing BeOS to run in its own hardware. Eventually there were
   lawsuits by Microsoft and Linux started gaining some multimedia
   capabilities. Finally, it tried releasing an x86 port but it was too
   late and by 2001 it was pretty much obscure.

MVS
^^^

#. Multiple Virtual Storage was IBM's flagship OS for mainframes.

#. Older than Unix so there really isn't much Unix design principles in
   it. Unifying idea is that all work is a *batch*. The system is
   designed to make the most efficient possible use of the machine for
   batch processing of huge amounts of data, with minimal concessions to
   interaction with human users.

#. Process spawning is a slow operation. The I/O system deliberately
   trades high setup cost (and associated latency) for better
   throughput. These choices are a good match for batch operation, but
   deadly to interactive response.

#. MVS uses the machine MMU; processes have separate address spaces.
   Interprocess communication is supported only through shared memory.
   There are facilities for threading (which MVS calls “subtasking”),
   but they are lightly used, mainly because the facility is only easily
   accessible from programs written in assembler.

#. Many system configuration files are in text format, but application
   files are usually in binary formats specific to the application.

#. File system security was an afterthought in the original design.
   However, when security was found to be necessary, IBM added it in an
   inspired fashion: They defined a generic security API, then made all
   file access requests pass by that interface before being processed.
   As a result, there are at least three competing security packages
   with differing design philosophies — and all of them are quite good,
   with no known cracks against them between 1980 and mid-2003.

#. There is no concept of one interface for both network connections and
   local files; their programming interfaces are separate and quite
   different. 

#. Casual programming for MVS is almost nonexistent except within the
   community of large enterprises that run MVS.

#. The intended role of MVS has always been in the back office.

VM/CMS
^^^^^^

#. VM/CMS is IBM's other mainframe operating system. Historically
   speaking, it is Unix's uncle: the common ancestor is the CTSS system,
   developed at MIT around 1963 and running on the IBM 7094 mainframe.
   Since the group that wrote CTSS went on to write Multics.

#. The unifying idea of the system, provided by the VM component, is
   virtual machines, each of which looks exactly like the underlying
   physical machine.

#. A scripting language called Rexx supports programming in a style not
   unlike shell, awk, Perl or Python. Consequently, casual programming
   (especially by system administrators) is very important on VM/CMS.

#. VM/CMS even went through the same cycle of de facto open source to
   closed source back to open source, though not as thoroughly as Unix
   did.

#. What VM/CMS lacks, however, is any real analog to C. Both VM and CMS
   were written in assembler and have remained so implemented.

#. Since the year 2000, IBM has been promoting VM/CMS on mainframes to
   an unprecedented degree — as ways to host thousands of virtual Linux
   machines at once.

Linux
^^^^^

#. Linux does not include any code from the original Unix source tree,
   but it was designed from Unix standards to behave like a Unix. 

#. The desire to reach end users has also made Linux developers much
   more concerned with smoothness of installation and software
   distribution issues than is typically the case under proprietary Unix
   systems. One consequence is that Linux features binary-package
   systems far more sophisticated than any analogs in proprietary
   Unixes, with interfaces designed (as of 2003, with only mixed
   success) to be palatable to nontechnical end users.

#. Linux 2.5's incorporation of extended file attributes, which among
   other things can be used to emulate the semantics of the Macintosh
   resource fork, is a recent major one at time of writing. This mainly
   to support other filesystems from other OSes natively on Linux.

#. Indeed, a substantial fraction of the Linux user community is
   understood to be wringing usefulness out of hardware as technically
   obsolete today as Ken Thompson's PDP-7 was in 1969. As a consequence,
   Linux applications are under pressure to stay lean and mean that
   their counterparts under proprietary Unix do not experience.

What Goes Around Comes Around
-----------------------------

#. Many of the major OSes today have adopted Unix principles. For
   example, MacOS merged Unix to its core. Windows is the only
   major alternative.

#. In a world of pervasive networking, even an operating system designed
   for single-user use needs multiuser capability (multiple privilege
   groups) — because without that, any network transaction that can
   trick a user into running malicious code will subvert the entire
   system (Windows macro viruses are only the tip of this iceberg).

#. Windows gets away with having severe deficiencies in these areas only
   by virtue of having developed a monopoly position before networking
   became really important, and by having a user population that has
   been conditioned to accept a shocking frequency of crashes and
   security breaches as normal. This is not a stable situation, and it
   is one that partisans of Linux have successfully (in 2003) exploited
   to make major inroads in the server-operating-system market.

#. The trend toward client operating systems was so intense that server
   operating systems were at times dismissed as steam-powered relics of
   a bygone age.

#. But as the designers of BeOS noticed, the requirements of pervasive
   networking cannot be met without implementing something very close to
   general-purpose timesharing. Single-user client operating systems
   cannot thrive in an Internetted world.

#. Retrofitting server-operating-system features like multiple privilege
   classes and full multitasking onto a client operating system is very
   difficult, quite likely to break compatibility with older versions of
   the client, and generally produces a fragile and unsatisfactory
   result rife with stability and security problems.

#. Retrofitting a GUI onto a server operating system, on the other hand,
   raises problems that can largely be finessed by a combination of
   cleverness and throwing ever-more-inexpensive hardware resources at
   the problem. As with buildings, it's easier to repair superstructure
   on top of a solid foundation than it is to replace the foundations
   without trashing the superstructure.

#. The Unix design proved more capable of reinventing itself as a client
   than any of its client-operating-system competitors were of
   reinventing themselves as servers.
