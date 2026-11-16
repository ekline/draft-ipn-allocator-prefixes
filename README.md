<!-- regenerate: off (set to off if you edit this file) -->

# IPv6 Prefixes Derived from IPN Allocator Identifiers

This is the working area for the individual Internet-Draft, "IPv6 Prefixes Derived from IPN Allocator Identifiers".

The draft requests an IPv6 /16 common prefix (preferred value `5f01::/16`).
It derives an optional IPv6 /48 from each eligible 32-bit IPN Allocator
Identifier ([RFC 9758](https://www.rfc-editor.org/rfc/rfc9758)):
`5f01:H:L::/48`, where H and L are the high and low 16 bits of the identifier.
The derived prefix is an address assignment only. It does not define routing,
an IPN-to-IPv6 address mapping, or a Bundle Protocol convergence layer.

IANA has not allocated `5f01::/16`. Do not deploy addresses from it.
The intended status is Experimental. The "Open Issues" appendix lists
questions for review.

* [Editor's Copy](https://ekline.github.io/draft-ipn-allocator-prefixes/#go.draft-ek-tiptop-ipn-allocator-prefixes.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-ek-tiptop-ipn-allocator-prefixes)
* [Individual Draft](https://datatracker.ietf.org/doc/html/draft-ek-tiptop-ipn-allocator-prefixes)
* [Compare Editor's Copy to Individual Draft](https://ekline.github.io/draft-ipn-allocator-prefixes/#go.draft-ek-tiptop-ipn-allocator-prefixes.diff)


## Contributing

See the
[guidelines for contributions](https://github.com/ekline/draft-ipn-allocator-prefixes/blob/main/CONTRIBUTING.md).

The contributing file also has tips on how to make contributions, if you
don't already know how to do that.

## Command Line Usage

Formatted text and HTML versions of the draft can be built using `make`.

```sh
$ make
```

Command line usage requires that you have the necessary software installed.  See
[the instructions](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md).

