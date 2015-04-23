Programming
===========

.. contents:: :depth: 3

Character Encodings For Modern Programmers
------------------------------------------

`Reference <http://blog.gatunka.com/2014/04/25/character-encodings-for-modern-programmers/>`_

#. The story of Unicode really starts with ASCII.

#. ASCII is important because it basically defined the subset of
   punctuation marks and symbols that would be used in every programming
   language and operating system that followed.

#. ASCII is a 7-bit encoding

#. There were national variants of ASCII where certain symbols where
   replaced with their national variants. For example, *#* was replaced
   with *£*. This causes a lot of problems with programming and was
   eventually abandoned. **Note to enter unicode characters using
   *urxvt* hold *Ctrl+Shift* and type the hex code. Finally, release
   *Ctrl+Shift*.  For example, for *£* type *Ctrl+Shift+a3***

#. ASCII 8-bit encodings add extra 128 code points and thus adds support
   for national variants.

UNIX, Terminals, and C1
^^^^^^^^^^^^^^^^^^^^^^^

#. UNIX is a terminal-based system (which means that it is usually
   operated by entering commands on a terminal).

#. UNIX used to be controlled by a dumb terminal. It is a physical
   equipment with a 80x25 screen and a keyboard that connects to the
   UNIX server by a serial line.

#. If you send text to the serial port of the terminal, it is the
   terminal that decides which actual character to print on the screen.
   Similarly, when you press a key on the keyboard, it is the terminal
   that decides which code to send to the UNIX server.

#. On top of this, this same communication channel needs to be used for
   sending control sequences, such as for moving the cursor, clearing
   the screen, and these sequences are all sent in-line intermixed with
   the character codes. This means that we can’t assign characters to
   every code point. We need control codes, and these control codes
   cannot overlap the characters in the character encoding.

#. Note that *printable* ASCII range is from 32 to 126. Codes 128 and
   above, however, are a free-for-all. Terminals can use them as control
   codes, printable characters, or basically anything and it doesn’t
   matter to the UNIX server.

#. Block of control codes 0 to 31 (known as the C0 block) as well as
   code 127 in ASCII are already allocated to control codes.

#. For reasons of consistency and interoperability, a recommendation was
   developed that codes 128 to 159 should be reserved for a second
   control block (called the C1 block), and only codes 160 to 255 be
   used for printable characters, and this formed the basis for most
   pre-Unicode UNIX encodings.

#. However, it is not a hard and fast rule, and UNIX itself does not
   treat the C1 block any differently from the rest of the upper 128
   codes.

#. This makes UNIX basically language agnostic. Let’s say we have a UNIX
   system and we have two files with names consisting of the character
   codes 68 196 and 68 228. When we connect a Greek language terminal to
   the system and list the files, we will see “DΔ” and “Dδ”. When we
   connect a French terminal to the system, we will see “DÄ” and “Dä”,
   and when we connect a Thai terminal we will see “Dฤ” and “Dไ”. The
   fact that the three different terminals show 3 different things
   doesn’t matter. A Thai user can still open one of the files by typing
   “vi Dฤ” on their keyboard, the same as the Greek can by typing “vi
   DΔ”. As far as the OS is concerned, as long as the underlying codes
   are the same, everything works fine. We might note here that “δ” is
   the lower case letter for “Δ” in Greek, whereas “ฤ” and “ไ” are
   completely different unrelated letters in Thai. This doesn’t matter
   to UNIX because the OS doesn’t try to do anything fancy like
   case-insensitive file names. The same applies to file content. If you
   print a file to the screen, the codes gets passed directly to the
   terminal, and it is the terminal that does the rendering. The OS
   doesn’t need to get involved.

#. Thus UNIX itself does not need an “encoding” setting. It simply
   doesn’t care.

#. However, any command or system service that is going to output
   human-readable error messages needs to know which language to output.
   Similarly, any add-on programs, particularly those that might perform
   some kind of collation or text processing, want to know what language
   they should use. But again, this does not need to be a system-wide
   setting, and only need apply to a particular user’s session. The
   system thus uses an environment variable (LOCALE) which specifies
   both the language and character encoding.

DOS/Windows
^^^^^^^^^^^

#. DOS and Windows does not use terminals and thus screen is addressed
   directly through the video adapter.

#. This means that it does not need any control codes. So DOS/Windows
   assigns printable characters to all of the ASCII control codes (0 to
   31 and 127) and all codes above 128.

#. DOS/Windows is still considered ASCII based since maintains all of
   ASCII printable characters (32 to 126).

#. This obviously is a major problem as line feeds get messed up in text
   files. And there is a problem with printing to ASCII serial printers
   which uses ASCII control block for printer sequences.

#. Another problem with Windows is that it uses case-insensitive file
   names and thus the OS needs to know the encoding (whether Greek,
   Thai, etc). Thus, Windows requires a system wide encoding.

#. It should come as no surprise then that Microsoft was one of the big
   backers of Unicode and produced one of the first Unicode-based OSes.

Programming With 8-Bit Encodings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In most cases you don't need to write encoding-aware software. For
   example, the C compiler does not need encoding-awareness.

#. In regular expression, all characters are byte-based and thus user
   needs to only input correct expressions for their language (e.g. a
   Norwegian person wanting to match all upper case letters is going to
   have to use the regexp /[A-ZÅÆ]/ instead of /[A-Z]/, but the actual
   regular expression parser does not need to be changed).

#. The basic assumption is that we have the same encoding end-to-end, so
   tools don’t have to do any conversion, they just spit out whatever
   input they get in.

#. If you want language-dependent operations, then use *<locale.h>* and
   *<ctype.h>*. You will need to perform *setlocale* and then do
   *isupper*, *islower*, etc.

Multibyte Encodings
^^^^^^^^^^^^^^^^^^^

#. Multibyte encodings are needed for East Asian languages of China,
   Japan, and Korea (CJK). ASCII is not enough to hold all possible
   combinations.

#. Other encodings based on ASCII, use multi-bytes to represent a single
   character.

#. The basic idea is that all non-ASCII letters are encoded as
   multi-byte sequences with all bytes in the 128 and higher code range.
   This means that we can suddenly start naming files in Japanese and
   Chinese without making any changes to the underlying OS.

#. However, the following will not work ``char mychar = '字';`` since
   this will appear to the compiler as two characters. Also, regexes to
   do not work as expected. Finally, outputting to fixed-width terminal
   or printer will not work as expected since these characters now take
   up two bytes (thus two spaces) to display one character. The
   exception here is *Shift JIS*. Single byte characters in Shift JIS
   take up single character cell while double-byte characters always
   take two character cells.

#. However, with *Shift JIS*, they use the code 92 (backslash) as part
   of second byte. Thus, ``printf("十");`` actually looks like
   ``printf("X\");`` to the compiler. There are 42 characters that use
   backslash in *Shift JIS*.

#. Prior to Unicode, the C and C++ standard libraries do not have any
   functions for handling CJK encodings. *Note that you can use
   ``mblen()`` to see how many bytes a character takes*.

Pre-Unicode Summary
^^^^^^^^^^^^^^^^^^^

#. Firstly, just about every application runs off the idea of a single
   encoding end-to-end. On UNIX, in particular, many protocols are
   encoding agnostic.

#. FTP is a great example. It has no concept of encoding or any way of
   specifying encodings. Whatever raw character data it receives it
   sends on through untouched. It’s up to the sys admin to make sure
   everyone using it sticks to the same encoding.

Unicode
^^^^^^^

#. The very first thing we need to understand is that UTF-8 did not
   exist and was not envisioned when Unicode first came into use, and
   Unicode referred basically to what we now call UTF-16.

#. Windows NT was the first major OS based on Unicode, and was coupled
   with NTFS, the first major filesystem to support Unicode.

#. However, NT still had the concept of a default non-Unicode encoding,
   the same as DOS and Windows 95.

#. This non-Unicode encoding is called the ANSI code page on Windows.
   The Win32 API thus comes with two complete sets of API, the ANSI API
   which accept string arguments of type ``char*`` encoded in the default
   non-Unicode encoding, and the Unicode (or wide char) API which accept
   string arguments of type ``wchar_t*``.

#. On UNIX, things were slightly different, and this is reflected in the
   C/C++ standard libraries. On UNIX, the expectation was that people
   would continue to use non-Unicode encodings in conjunction with the
   LOCALE environment variable, but programmers would be given the
   option of an automatic conversion to Unicode mode of file operation,
   that would allow them to code using Unicode while the underlying OS
   and file content would continue to use non-Unicode encodings.

#. When we open a file in C (using ``fopen()``), the file does not have an
   orientation. If we call a non-Unicode file function such as ``fgets()``,
   the file gets set to non-Unicode orientation, and we simply get
   passed the data from the file.

#. However, if we call a Unicode file function such as fgetws(), the
   file is put into Unicode orientation. This does not mean that the
   file on the disk is treated as containing Unicode. Instead, the C
   standard library assumes that the file is encoded in the non-Unicode
   encoding as specified by LOCALE and performs implicit automatic
   conversion of the file content from that non-Unicode encoding into
   Unicode in the form of ``wchar_t*``.

#. On Windows NT, however, we have a problem. The C standard library
   still assumes that files are saved in a non-Unicode filesystem. There
   is no support for Unicode filenames. There is no variant of the
   ``fopen()`` function which accepts a ``wchar_t*`` for the filename.

#. UNIX chose 32-bit Unicode standard (ISO based) while Windows went
   with 16-bit universal encoding (Unicode organization). Thus,
   eventually we ended up with a 32-bit Unicode standard with UTF-16 as
   a variable-length 16-bit encoding.

#. Around the sametime UTF-8 was being developed, which was another
   variable length encoding.

#. Once UTF-8 was released, UNIX had a clear path towards total Unicode
   compatibility. Simply adopt UTF-8 as your standard encoding and all
   your problems go away.

#. UTF-8 is the basic standard encoding used in Linux and it's
   derivatives. Since UTF-8 can be encoded in a char*, it even works
   fine with the broken C standard libraries. UTF-8 plugs into the C
   standard library on what was meant to be the non-Unicode side since
   it is stored in char* and specified as an encoding using LOCALE.
   However, Linux/Windows interoperation is worse than ever.

Basic Programming With Unicode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. 
