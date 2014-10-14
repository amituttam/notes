Chapter 5: Textuality
=====================

.. contents:: :depth: 2

*It's a well-known fact that computing devices such as the abacus were
invented thousands of years ago. But it's not well known that the first
use of a common computer protocol occurred in the Old Testament. This,
of course, was when Moses aborted the Egyptians' process with a
control-sea.*

-- Tom Galloway rec.arts.comics, February 1992

#. **Good Protocols Make Good Practice**

#. Two different designs in Unix are closely related. Both involve
   serialization of in-memory data structures.

   * Design of file formats for storing application data.

   * And design of application protocols for passing data between
     programs and network.

#. For transmission and storage, the traversable, quasi-spatial layout
   of data structures like linked lists needs to be flattened or
   serialized into a byte-stream representation from which the structure
   can later be recovered.

#. Interoperability, transparency, extensibility, and storage or
   transaction economy: these are the important themes in designing file
   formats and application protocols. Note that these are in order of
   most important to least important.

   * For interoperability and transparency it is important to have clear
     and clean data representations rather than thinking of what is best
     for performance.

   * Extensibility is suitable for text formats since they can be
     extended very easily.

   * Storage or transtion economy often pushes the opposite direction
     but it is not wise to put this of importance first.

#. Pipes and sockets will pass binary data as well as text. However, it
   is always better to use textual data because they are easy for humans
   to read and write.

#. Some programmers are concerned about performance and size. Thus, they
   design binary formats. But implementing compression below or above
   the application protocol will probably result in a better, faster
   design since text compresses very well.

#. Textual protocol future proofs your system. IPv4 allowed only for
   32-bits to an address, changing to 128-bits took a long time and a
   lot of effort.

#. Sometimes binary protocol should be used. For example, to get the
   most bit-density out of your data (e.g. multimedia) or very concerned
   about speed (e.g. network protocols with hard latency requirements).

#. SMTP and HTTP are text protocols but are very bandwidth intensive.
   The smallest X-server request is 4 bytes, while the smallest HTTP
   request is 100 bytes. Thus, X requests can be executed in about 100
   instructions while Apache can take around 7000 instructions to
   process an HTTP request.

   * Problem eventually showed up in X, where it was very difficult to
     extend it.

#. For the Unix ``passwd`` file, there isn't really much saving you can do
   with a binary file format. Colon ':' separators are quite small,
   binary equivalent would not be much smaller.

   * Also need to understand the application of the passwd file. Its not
     read often and not really modified that often either (relatively
     speaking in terms of other admin operations).

#. In the case of PNG format, it is a losless graphics format and using
   text to store data would cause significant problems in transaction
   economy (long download times for large files). However, transparency
   was sacrificed.

   * Very thoughtful design with interoperability in mind.
   * PNG specifies byte orders, integer word lengths, endianness, and
     (lack of) padding between fields.
   * A PNG file consists of a sequence of chunks, each in a
     self-describing format beginning with the chunk type name and the
     chunk length. Because of this organization, PNG does not need a
     release number. New chunk types can be added at any time; the case
     of the first letter in the chunk type name informs PNG-using
     software whether or not each chunk can be safely ignored.
   * Also designed to easily detect file corruption.

#. In Unix, there are already established textual format designs that
   should be used since libraries are already written to parse them.
   Also, users probably recognize them already. Examples are *DSV
   (Delimiter Separated Values)*. On the other hand, *CSV (Comma
   Separated Values)* are not really used in Unix.

   * CSV has a lot more complicated parser since if comma is found in a
     record, the whole record needs to be escaped with quotes. And if the
     record has quotes already, need to escape with more quotes.

   * In DSV, only need to escape ':' with a backslash. And backslashes
     are escaped by another backslash.

#. RFC 822 is a textual format for Internet electronic messages. It
   allows for attachments using MIME (Multipurpose Internet Media
   Extension).

   * Record attributes are stored one per line. Thus, new records are
     easily added (we can see this with mail headers).

   * Used by HTTP 1.1 (and later).

   * One weakness is when putting more than one RFC 822 message in a
     file (e.g. Mailbox). Hard to distinguish where one starts and the
     other message ends.

#. XML syntax resembles HTML. It is basically a low-level syntax. Needs
   document type definition (DTDs) such as XHTML to give it semantics.

   * Suited for complex data formats but overkill for small ones.
   * Useful for complex nested/recursive structure for which RFC 822 is
     not useful at all.
   * Down side is it can get bulky. Hard to see real data in between all
     the syntax. Traditional Unix tools don't play well with it so you
     need external and complex parsers.

#. Windows INI format is useful for two-level data organization. A
   section header and related records under that. If all you need is a
   simple key-value pair, it is better to use DSV.

#. When designing a format, include version or include the data in
   self-describing independent chunks.

#. Conversion of floating-point numbers from binary to text format and
   back can lose precision, depending on the quality of the conversion
   library you are using. Sometimes you have to dump the floating point
   field as raw binary.

The Pros and Cons of File Compression
-------------------------------------

#. Many modern Unix projects (OpenOffice, AbiWord) use XML compressed
   with *zip* or *gzip* as the data file format.

   * When compressing whole file, the compression tool can look at whole
     file for repetitive data to compress. Thus, in some cases, results
     in smaller file than binary data format (e.g. Microsoft Word's
     native format).

   * Can also change compression method in future, since it is not tied
     to XML.

#. Compression can limit data transparency. Some Unix tools such as file
   can't see past the compression header (as of 2003). And other tools
   might require you to uncompress data before you can, for example,
   *grep* through it (can use *zgrep* today I guess).

   * Can probably use straight up *gzip* compressed XML data without
     self-identifying structure or header provided by *zip*.

#. Idea is to think of tradeoffs from the beginning of the design on the
   program.

Application Protocol Design
---------------------------

#. All the good reasons for being textual apply to application-specific
   protocols as well.

#. When your application protocol is textual and easily parsed by
   eyeball, many good things become easier. Transaction dumps become
   much easier to interpret. Test loads become easier to write.

#. In the case of SMTP, command requests are simple *<command>
   <arguments>* format. While responses are *<status code> <message>*.
   It is easy to debug. SMTP is a *push* protocol where requests are
   initiated by the client.

#. POP3 is a *pull* protocol with transactions initiated by the mail
   receiver. POP3 is also textual and line oriented. Similar to SMTP in
   request/response formats. POP3, however, uses status tokens instead
   of status codes like SMTP.

#. IMAP is similar as well. However, instead of ending payload with a
   dot, it sends back the length in bytes first. It makes it easier on
   client so client knows how much buffer to allocate.

   * IMAP also adds a sequence number to each request. Thus, requests
     can be sent to server in bulk at once.

#. Most applications nowadays layer their special purpose protocols on
   top of HTTP. HTTP has become a universal application protocol.

   * Can use existing HTTP methods *GET (fetch resource)*, *PUT (modify/create
     resource)*, and *POST (ship data to a form or backend process)*.

   * Has a RFC 822/MIME message format. Thus, can contain arbitrary
     messages in them.

   * Also has support for authentication and extensible headers.

   * Application can tunnel through native HTTP port 80 instead of a
     custom TCP/IP port which may need to be opened up in the firewall.

     * However, it is not good practice to use same port, especially if
       the application is serving data quite different from normal HTTP.

     * Thus, with a separate port, you can also easily distinguish the
       traffic and maybe filter it out if necessary.

     * Also note that if different port is used, a new URL scheme needs
       to be registered (e.g. *git://*).

   * However, there is definitely a risk. When the webserver and plugins
     become more complicated, cracks in the code can have large security
     implications.

   * `RFC 3205 Use of of HTTP as a Substrate <http://tools.ietf.org/html/rfc3205>`_ has good advice for
     using HTTP as under layer of an application protocol.

     * Be careful when re-using HTTP status codes. For example, a 200
       error your application returns means success in HTTP. Thus, a
       proxy caching responses will send that response back to other
       requests. Similarly with 500 error, the proxy might respond and
       add a helpful message but the 500 error means something else to
       your application.

     * If the different codes needs to be returned, they should not be
       returned in the standard HTTP headers but in the body of the
       message.

     * A layered application which cannot operate in the presence of
       intermediaries or proxies that cache and/or alter error
       responses, should not use HTTP as a substrate.

#. *IPP (Internet Printing Protocol)* is used to control
   network-accessible printers.

   * Uses HTTP 1.1 as a transport layer. All IPP requests are passed via
     an HTTP POST method call. Responses are ordinary HTTP responses.

   * HTTP 1.1 allows persistent connections that make a multi-message
     protocol more efficient. Thus you can chunk the files without
     having to pre-scan the files to determine length of request. *Note
     that in this case, Transfer-Encoding header is used instead of
     Content-Lenght*. Thus, keep sending requests as the process scans the files.

   * Also, using HTTP redirection (301 Code), the server can tell client
     to redirect the submission of the job to another printer server
     sine this is not available.

   * Can run over TLS/SSL to encrypt messages.

   * Most network aware printers already embed a web server do display
     status to users. Thus, natural to use HTTP to also control printer.

   * The only drawback in that the protocol is completely driven by
     client requests. No way to really ship asynchronous alerts from
     printers back to client (can use AJAX nowadays to do polling).

   * Note that IPP uses HTTP as underlying protocol but uses different
     port (631) for security reasons.

#. XML-RPC, SOAP, and Jabber all use XML with MIME to structure requests
   and payloads.

    * XML-RPC is simple and extensible. However, currently being
      replaced by JSON.

    * SOAP is more heavy weight and includes arrays and C-like structs.
      Considered bloated by many.

    * Jabber is a peer-to-peer protocol that support instant messaging
      and presence. Passes around XML forms and live documents.
