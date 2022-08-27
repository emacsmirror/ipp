# ipp.el -- Emacs Lisp implementation of the Internet Printing Protocol


This Emacs package provides a partial implemention of the client component of the Internet Printing
Protocol (IPP). IPP was intended to replace the LPD protocol for interacting with network printers.
It specifies mechanisms for “driverless printing” (submitting and cancelling jobs), queue monitoring
and querying printer capabilities. More recent versions of the standard are called “IPP Everywhere”.

You can find out whether a device is IPP-capable by trying to telnet to port 631. If it accepts the
connection it probably understands IPP. You then need to discover the path component of the URI, for
example by reading the documentation or from a driver program. Tested or reported to work on the
following devices:

- Tektronix Phaser 750, with an URI of the form ipp://host:631/ (empty path component)

- HP Laserjet 4000, with a path component of /ipp/port1.

- HP Color LaserJet MFP M477fdw

- Lexmark E460dn, with an empty path component

- Xerox Document Centre 460 ST, with empty path component.

- [CUPS printer spooler](https://www.cups.org/).


## Usage

Load this package by putting in your `~/.emacs.el` initialization file

    (require 'ipp)

then try printing a file using `M-x ipp-print`. This will prompt you for a file name (which should
be in a format understood natively by the printer, such as PDF), and the URI of the printer. The URI
should be of the form

    ipp://10.0.0.1:631/ipp/port1   (unencrypted connection on port 631, path="/ipp/port1")
    ipps://10.0.0.1/               (TLS connection on port 631, empty path component)

There are also two functions for querying the capability of the device (`ipp-get-attributes`) and
examining its queue (`ipp-get-jobs`). Until I write display code for these functions you will have to
call them from an IELM buffer to examine their return value.

    ELISP> (ipp-get-attributes "ipps://127.0.0.1:631/")

The IPP network protocol is based on HTTP/1.1 POST requests (or potentially using HTTP/2 in the most
recent versions, though we do not support this), using a special `application/ipp` MIME
Content-Type. The data is encoded using simple marshalling rules.



The Internet Printing Protocol is described in
[RFC 8011](https://www.rfc-editor.org/rfc/rfc8011.html), and previously RFC numbers 3382, 3381,
2911, 2568, 2566, 2565. The Printer Working Group maintain a page at

  <https://www.pwg.org/ipp/>


Eventually it would be nice to modify the Emacs printing API to support this type of direct
printing, so that a user could set `ps-printer-name` to "ipp://modern-printer:631/" or
"lpd://ancient-printer/queue" (it would be easy to write a package similar to this one implementing
the LPD protocol at the network level; the LDP protocol is very simple).

Thanks to Vinicius Jose Latorre and Marc Grégoire for patches and to Colin Marquardt and Andrew
Cosgriff for help in debugging.


The **latest version** of this package should be available from

    <https://github.com/emarsden/ipp-el>
