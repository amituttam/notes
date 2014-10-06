Chapter 7: Multiprogramming
===========================

.. contents:: :depth: 2

#. The most characteristic program-modularization technique of Unix is
   splitting large programs into multiple cooperating processes. This
   has usually been called *multiprocessing* in the Unix world, but in
   this book we revive the older term *multiprogramming* to avoid
   confusion with multiprocessor hardware implementations.

#. The Unix operating system encourages us to break our programs into
   simpler subprocesses, and to concentrate on the interfaces between
   these subprocesses. It does this in at least three fundamental ways:

    * by making process-spawning cheap;

    * by providing methods (shellouts, I/O redirection, pipes,
      message-passing, and sockets) that make it relatively easy for
      processes to communicate;

    * by encouraging the use of simple, transparent, textual data
      formats that can be passed through pipes and sockets.

#. While the benefit of breaking programs up into cooperating processes
   is a reduction in global complexity, the cost is that we have to pay
   more attention to the design of the protocols which are used to pass
   information and commands between processes. (In software systems of
   all kinds, bugs collect at interfaces.)

#. It is also important to have a state-machine design that is
   effectively deadlock free.

#. A closely related red herring is threads (that is, multiple
   concurrent processes sharing the same memory-address space).
   Threading is a performance hack.

   * The advice of this book is to not use threads until absolutely
     necessary.

   * Since processes are very cheap to spawn, use those.

#. Another important reason for breaking up programs into cooperating
   processes is for better security. Under Unix, programs that must be
   run by ordinary users, but must have write access to
   security-critical system resources, get that access through a feature
   called the *setuid* bit.

   * A setuid program runs not with the privileges of the user calling
     it, but with the privileges of the owner of the executable. This
     feature can be used to give restricted, program-controlled access
     to things like the password file that nonadministrators should not
     be allowed to modify directly.

Taxonomy of Unix IPC Methods
----------------------------

*system* and *popen*
^^^^^^^^^^^^^^^^^^^^

#. Simplest form is to spawn another program using the *system(3)*
   command. This inherits user's keyboard and display and runs to
   completion.

   * In this case, calling program does not communicate with the called
     program.

   * The classic example is shelling out an editor from within a mail or
     news program.

   * For more complicated cases where the called programs needs to
     accept input and share output is to use *popen*.

   * *mutt* uses the *EDITOR* environment variable when
     composing/replying to messages. It creates a temporary file in the
     filesystem and spawns the editor to use this file. It then reads
     this file when it sends out the mail. Thus, it uses the filesystem
     to communicate with its called program.

   * Can use *EDITOR=emacsclient*, this is a proxy application that
     creates a new buffer in an already open emacs session.

Pipes, Redirection, and Filters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Doug McIlroy invented the *pipe* and the construct was very
   important through the design of Unix and its philosophy of do one
   thing and do it well.

   * This also inspired later forms of IPC (especially, socket
     abstraction used for networking).

#. Pipes depend on the convention that every program has initially
   available to it (at least) two I/O data streams: standard input and
   standard output (numeric file descriptors 0 and 1 respectively). Many
   programs can be written as filters, which read sequentially from
   standard input and write only to standard output.

#. Normally these streams are connected to the user's keyboard and
   display, respectively. But Unix shells universally support
   redirection operations (<, >) which connect these standard input and output
   streams to files.

#. It's important to note that all the stages in a pipeline run
   concurrently. Each stage waits for input on the output of the
   previous one, but no stage has to exit before the next can run. This
   property will be important later on when we look at interactive uses
   of pipelines, like sending the lengthy output of a command to *more(1)*.

   * Or when running *bc | espeak*.

#. **The major weakness of pipes is that they are unidirectional.** It's not
   possible for a pipeline component to pass control information back up
   the pipe other than by terminating (in which case the previous stage
   will get a SIGPIPE signal on the next write). Accordingly, the
   protocol for passing data is simply the receiver's input format.

#. There can be named pipes, where a file is opened between the two
   programs (one for reading and the other for writing). Largely
   displaced by sockets.

#. Code bloat can be avoided, for example, since utilities can use a
   *more* or *less* pagers instead of implementing their own pagers.

Slave Processes
^^^^^^^^^^^^^^^

#. Master uses *popen* to spawn and communicate with slave process.

#. Example is *scp(1)* calls *ssh(1)* as a slave process. intercepting
   enough information from ssh's output to format report as ASCII
   progress bar.

Peer-to-Peer Inter-Process Communication
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. The previous sections depicted a hierarchy of communication where one
   program controls the other.

**Tempfiles**

#. Useful for simple one-off programs and simple shellscript or
   wrappers. Shellout to an editor is best example.

#. Drawback is it leaves garbage behind that needs to be cleaned up if
   process is interrupted before tempfile can be deleted.

#. Other problem is non unique filenames. Most shell scripts use *$$*
   which expands to PID of process in the filename (Linux kernel wraps
   around and re-uses old PIDs if it reaches max of 32768
   */proc/sys/kernel/pid_max*).

#. There is also security risk if the tempfile name is easily predicted
   or known/visible attacker can modify file while process is running
   and thus inject own input back into program.

**Signals**

#. Simplest and crudest way for two processes to communicate with each
   other.

   * Signal handler is executed asynchronously when the signal is
     received.

#. Not really designed as an IPC but more of a way for OS to notify
   programs of certail errors and events.

   * The *SIGHUP* signal, for example, is sent to every program started
     from a given terminal session when that session is terminated. This
     is why *nohup* is used to spawn a program (ignores *SIGHUP*) and
     keeps running in background.

   * The SIGINT signal is sent to whatever process is currently attached
     to the keyboard when the user enters the currently-defined
     interrupt character (often control-C).

   * *SIGUSR1* and *SIGUSR2* are part of POSIX standard used for some
     IPC situation. A way for operator or another program to tell a
     daemon that it needs to either reinitialize itself, wake up to do
     work, or write internal-state/debugging information to a known
     location.

#. Technique used with signals is *pidfile*. Programs that will need to
   be signaled will write their PID to a file in a known location
   (*/var/run* for example).

   * Other programs can read that file to discover that PID. The pidfile
     may also function as an implicit lock file in cases where no more
     than one instance of the daemon should be running simultaneously.

#. SIGTERM (‘terminate’) is often accepted as a graceful-shutdown signal
   (this is as distinct from SIGKILL, which does an immediate process
   kill and cannot be blocked or handled). SIGTERM actions often involve
   cleaning up tempfiles, flushing final updates out to databases, and
   the like.

**Sockets**

#. Developed in BSD as a way to encapsulate access to data networks.

#. Two programs communicating over a socket see a bi-directional byte
   stream.

#. Byte streams are sequenced (single bytes will be received in the same
   order they were sent).

#. Byte streams are reliable (socket users are guaranteed that the
   underlying network will do error detection and retry to ensure
   delivery).

#. Socket descriptors once obtained, behave essentially like file
   descriptors.

#. Ken Arnold: **Sockets differ from read/write in one important case.
   If the bytes you send arrive, but the receiving machine fails to ACK,
   the sending machine's TCP/IP stack will time out. So getting an error
   does not necessarily mean that the bytes didn't arrive; the receiver
   may be using them. This problem has profound consequences for the
   design of reliable protocols, because you have to be able to work
   properly when you don't know what was received in the past. Local I/O
   is ‘yes/no’. Socket I/O is ‘yes/no/maybe’. And nothing can ensure
   delivery — the remote machine might have been destroyed by a comet.**

#. At the time a socket is created, you specify a protocol family which
   tells the network layer how the name of the socket is interpreted.

   * *AF_INET* family in which addresses are interpreted as host-address
     and service-number pairs.

   * *AF_UNIX* (aka *AF_LOCAL*) protocol family supports the same socket
     abstraction for communication between two processes on the same
     machine (names are interpreted as the locations of special files
     analogous to bidirectional named pipes). As an example, client
     programs and servers using the X windowing system typically use
     AF_LOCAL sockets to communicate.

#. To use sockets gracefully, in the Unix tradition, start by designing
   an application protocol for use between them — a set of requests and
   responses which expresses the semantics of what your programs will be
   communicating about in a succinct way.

#. For example in PostgresSQL: Because the front end and back end are
   separate, the server doesn't need to know anything except how to
   interpret SQL requests from a client and send SQL reports back to it.
   The clients, on the other hand, don't need to know anything about how
   the database is stored. Clients can be specialized for different
   needs and have different user interfaces.

#. Sockets inherently separates the address space of processes and
   implicitly defines a client/server or peer-to-peer model of
   communication.

**Shared Memory**

#. If your communicating processes can get access to the same physical
   memory, shared memory will be the fastest way to pass information
   between them.

#. Typically use *mmap* to map files into memory that can be shared
   between processes. Or can use POSIX *shm_open* API to create a file
   that can be shared. Basically, tells OS not to flush the pseudofile
   data to disk.

#. Because access to shared memory is not automatically serialized by a
   discipline resembling read and write calls, programs doing the
   sharing must handle contention and deadlock issues themselves,
   typically by using semaphore variables located in the shared segment.

#. X uses shared memory for performance gains to pass large images
   between client and server.
