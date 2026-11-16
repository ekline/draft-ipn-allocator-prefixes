---
title: "IPv6 Prefixes Derived from IPN Allocator Identifiers"
abbrev: "IPN Allocator Prefixes"
category: exp
updates: 4291

docname: draft-ek-tiptop-ipn-allocator-prefixes-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
# area: INT
# workgroup: TIPTOP
keyword:
 - IPv6 prefixes
 - IPN Allocators
 - DTN
venue:
#  group: TIPTOP
#  type: Working Group
#  mail: tiptop@ietf.org
#  arch: https://example.com/WG
  github: "ekline/draft-ipn-allocator-prefixes"
  latest: "https://ekline.github.io/draft-ipn-allocator-prefixes/draft-ek-tiptop-ipn-allocator-prefixes.html"

author:
 -
    fullname: "Erik Kline"
    organization: "Aalyria Technologies Inc."
    email: "ek.ietf@gmail.com"

normative:
  RFC4291:
  RFC7136:
  RFC6890:
  RFC8190:
  RFC9758:
  IANA-IPN:
    title: "'ipn' Scheme URI Allocator Identifiers"
    author:
      org: IANA
    target: https://www.iana.org/assignments/uri-schemes

informative:
  RFC1897:
  RFC3701:
  RFC4862:
  RFC5156:
  RFC5952:
  RFC6480:
  RFC7421:
  RFC7608:
  RFC8949:
  RFC9171:
  RFC9602:
  RFC9812:
  IANA-IPV6-SPACE:
    title: "IPv6 Address Space"
    author:
      org: IANA
    target: https://www.iana.org/assignments/ipv6-address-space
  IANA-IPV6-SPECIAL:
    title: "IPv6 Special-Purpose Address Space"
    author:
      org: IANA
    target: https://www.iana.org/assignments/iana-ipv6-special-registry

...

--- abstract

This document requests an IPv6 /16 prefix for use by IPN Allocators.
It defines a method to derive an IPv6 /48 prefix from a 32-bit IPN Allocator Identifier.
Use of the derived prefix is optional.
The method reuses the existing IPN Allocator registry.
It does not encode physical location or define a routing mechanism.
The preferred /16 prefix is 5f01::/16.
This document also updates RFC 4291 to permit other interface identifier lengths within the allocated block.


--- middle

# Introduction {#intro}

An IPN Allocator Identifier {{RFC9758}} identifies an authority that assigns IPN Node Numbers.
This document associates an optional IPv6 prefix with that authority.
The association does not describe network topology.
It does not identify a next hop or establish a route.

An Allocator can use the derived prefix to assign IPv6 addresses within networks under its administration.
The method reuses the existing "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}}.
It does not require a new per-identifier registry.


## Scope {#scope}

This document defines an address assignment mechanism.
The mechanism applies to terrestrial networks and to space networks.
This document also updates {{RFC4291}} for addresses within the common prefix ({{update-4291}}).

This document does not define:

* a conversion between an IPN endpoint identifier and an IPv6 address;

* an encoding of an IPN Node Number or Service Number in an IPv6 address;

* a Bundle Protocol {{RFC9171}} convergence layer;

* a method to carry IPv6 packets in bundles;

* a routing protocol or an automatic route discovery mechanism; or

* a requirement to use IPv6 in all Delay-Tolerant Networking (DTN) deployments.
{: spacing="compact"}


## Motivation {#motivation}

An Allocator already manages a namespace for IPN Node Numbers.
A corresponding IPv6 prefix can simplify address administration for networks that use both IPN identifiers and IPv6 addresses.

Physical location does not determine network connectivity or transit policy.
This document does not assign prefixes to celestial bodies.

An Allocator can retain its derived prefix when its equipment changes location.
Continued reachability depends on the available routes and applicable routing policy.
This document makes no routing scalability guarantee.
It does not define a mobility mechanism.


## Experiment Scope {#experiment}

This document specifies an experiment.
The experiment evaluates these questions:

* whether registry-derived prefixes simplify address administration;

* whether participating implementations support the intended address structures;

* whether routing and filtering permit the intended connectivity; and

* whether changes in Allocator control can occur without conflicting assignments.
{: spacing="compact"}

This document encourages operators and implementers to publish reports of their deployment experience.
The experiment makes no claim of automatic route aggregation or improved routing scalability.


# Conventions and Definitions {#conventions}

{::boilerplate bcp14-tagged}

This document uses the terms "Node Number" and "Service Number" as defined in {{RFC9758}}.
This document also uses the terms that follow.

Allocator:
: An organization or other authority that assigns IPN Node Numbers ({{Section 3.2 of RFC9758}}).

Allocator Identifier:
: An unsigned 32-bit integer A that identifies an Allocator ({{Section 3.2 of RFC9758}}).
  "The Allocator of A" is the Allocator that the IANA registry {{IANA-IPN}} currently lists for A.

Common prefix:
: The IPv6 /16 prefix that this document requests.
  The preferred value is 5f01::/16.

Derived prefix:
: The IPv6 /48 prefix P(A) associated with one Allocator Identifier A.

Eligible identifier:
: An Allocator Identifier that this document permits for ordinary use.
  {{eligibility}} gives the rules.

An Allocator can hold more than one Allocator Identifier ({{Section 3.2 of RFC9758}}).
Each Allocator Identifier is associated with only one Allocator.
This document provides one derived prefix for each eligible identifier.
Thus, one Allocator can have more than one derived prefix.


# Prefix Construction {#construction}

## Common Prefix {#common-prefix}

This document requests 5f01::/16 as the common prefix.
Until IANA allocates the common prefix, 5f01::/16 is requested and unassigned for this purpose.
Until then, an Allocator MUST NOT treat an eligible identifier as permission to use 5f01::/16.
The allocation requires IETF Review ({{iana-space}}).

The first instance of the 6bone experimental network used 5f00::/8 for IPv6 testing {{RFC1897}} {{RFC3701}}.
That block was later returned to IANA for future allocation {{RFC5156}}.
{{RFC9602}} then assigned 5f00::/16 for Segment Routing over IPv6 (SRv6) Segment Identifiers (SIDs).

Allocating 5f01::/16 extends the allocation adjacent to 5f00::/16.
Within the remaining 5f00::/8 space, this choice avoids splitting a larger aligned block.
The mechanism does not depend on the specific /16 value or on SRv6.

Note to the RFC Editor:
If IANA allocates a /16 other than 5f01::/16, replace "5f01" with the allocated value throughout this document.
This replacement includes the derived prefixes, the examples, and the IANA entries.
Also revise the rationale in this section and the filter text in {{routing}}.
Then remove this note.


## Address Layout {#layout}

This document numbers the 128 bits of an IPv6 address from 0 to 127.
Bit 0 is the most significant bit.

| Bits   | Length  | Field                              |
|--------|--------:|------------------------------------|
| 0-15   | 16 bits | Common prefix                      |
| 16-47  | 32 bits | Allocator Identifier               |
| 48-127 | 80 bits | Allocator-controlled address space |
{: #tab-layout title="Layout of an Address in a Derived Prefix"}

The requirements that follow apply to each party that constructs a derived prefix.
The derived prefix MUST contain the complete 32-bit Allocator Identifier immediately after the common prefix.
The identifier MUST appear in network byte order.
Leading zero bits are significant in this binary field.
The lengths of the decimal text form and of the Concise Binary Object Representation (CBOR) {{RFC8949}} encoding have no effect on the field.
A 16-bit common prefix and a 32-bit Allocator Identifier produce a /48.


## Derivation {#derivation}

For an Allocator Identifier A, define two 16-bit values:

* H = floor(A / 65536), the 16 most significant bits of A

* L = A modulo 65536, the 16 least significant bits of A
{: spacing="compact"}

With the requested common prefix, the derived prefix is:

~~~
P(A) = 5f01:H:L::/48
~~~

H and L are written as hexadecimal fields of the IPv6 text form ({{Section 2.2 of RFC4291}}).
The examples in this document use the recommended text form ({{Section 4 of RFC5952}}).


## Allocator-Controlled Address Space {#allocator-space}

Bits 48 to 127 of each address in P(A) are under the control of the Allocator of A.
The Allocator controls address assignment and prefix delegation within P(A).
These activities remain subject to this document and applicable IPv6 specifications.
This document does not require a particular structure or encoding in these 80 bits.

Assignment:
: The Allocator can assign prefixes of any length within P(A).
  Ordinary /64 subnets remain permitted.

Routing and forwarding:
: IPv6 forwarding processes support prefixes of any length {{RFC7608}}.
  Routing policy determines which prefix lengths operators advertise and accept.
  Forwarding support does not establish support for host address configuration.

Host interface addresses:
: {{update-4291}} permits interface identifiers other than 64 bits within the common prefix.
  Host and link support for other lengths varies ({{slaac}}).


## Update to RFC 4291 {#update-4291}

For addresses within the common prefix, this document removes the requirement for a 64-bit interface identifier.
This exception updates Sections 2.5.1 and 2.5.4 of {{RFC4291}}.
It applies only to the common prefix allocated by this document.

{{Section 2.5.1 of RFC4291}}, as updated by {{Section 5 of RFC7136}}, requires 64-bit interface identifiers for unicast addresses that do not start with binary 000.
{{Section 2.5.4 of RFC4291}} states the same constraint for Global Unicast addresses.
This document adds the text that follows after each of these two statements:

> This requirement does not apply to addresses within the IPv6 block allocated for IPv6 Prefixes Derived from IPN Allocator Identifiers (RFC XXXX).
> In that block, an interface identifier can have any length that the subnet prefix permits.

Note to the RFC Editor:
Replace "RFC XXXX" with the number of this document, then remove this note.

All other text of Section 2.5.1, as updated by {{RFC7136}}, still applies.
For example, a 64-bit interface identifier derived from an IEEE MAC-layer address still uses the Modified EUI-64 format.

{{RFC9602}} is a limited precedent for this exception.
Its discussion of {{RFC4291}} concerns SRv6 SIDs that are not assigned to host interfaces.


## Address Configuration {#slaac}

This exception does not change SLAAC or link-specific address configuration procedures.
Operators must select address structures that their hosts and links support.

In Stateless Address Autoconfiguration (SLAAC), a host forms an address from an advertised prefix and an interface identifier.
A link-type specification defines the length of that interface identifier.
Many link-type specifications define a length of 64 bits ({{Section 4.1 of RFC7421}}).
The host ignores the prefix for address formation if the two lengths do not sum to 128 bits ({{Section 5.5.3 of RFC4862}}, item (d)).
Thus, SLAAC on such links forms addresses only from /64 prefixes.
This document does not define a new SLAAC mechanism.


# Eligibility and Optional Use {#eligibility}

The current IANA "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}} determines eligibility.
The rules that follow apply to each Allocator Identifier A.

1. Ordinary assignment:
   A current ordinary assignment of A permits the Allocator of A to assign addresses and delegate prefixes from P(A).
   An ordinary assignment is a registry entry that assigns A to a named Allocator, alone or in a range.
   Rules 3 to 7 take precedence over this rule.

2. Optional use:
   The Allocator of A decides whether to use P(A).
   This document does not require a second registration for each identifier.

3. Unassigned identifiers:
   An unassigned identifier does not permit use of its derived prefix.

4. Reserved identifiers:
   A reserved identifier does not permit ordinary use.
   The reserved identifiers include 0x80000000 to 0xFFFFFFFF.

5. Experimental identifiers:
   Identifiers in the Experimental Use range (0x40000000 to 0x7FFFFFFF) do not confer exclusive control of globally unique IPv6 prefixes.
   This document does not specify operational use of their derived prefixes.

6. Example Range:
   The IPN Example Range (974848 to 978943) is for documentation ({{Section 9.1 of RFC9758}}).
   Its derived prefixes are not for operational deployment.

7. Allocator Identifier zero:
   Allocator Identifier zero does not permit ordinary use under this specification.
   {{default-allocator}} gives the reason.
{: spacing="compact"}

An Allocator Identifier is an eligible identifier only if rule 1 applies and rules 3 to 7 do not exclude it.
No other text in this document makes an Allocator Identifier eligible.

A party MUST NOT assign addresses or delegate prefixes from P(A) unless A is eligible and the party has authority from the Allocator of A.
These eligibility rules govern address assignment and prefix delegation.
They do not require a router to query the IPN registry when forwarding a packet.


## The Default Allocator {#default-allocator}

{{RFC9758}} assigns Allocator Identifier zero to the Default Allocator ({{Section 3.2.2 of RFC9758}}).
The Default Allocator serves multiple independent organizations.
Assignment of a Node Number by the Default Allocator does not assign control of the derived /48.
This document reserves P(0), which is 5f01::/48, for future specification.
This document does not define a subdivision of P(0).


# Examples {#examples}

The examples in this section are illustrations.
They use the requested common prefix 5f01::/16 and identifiers from the IPN Example Range.
They do not represent operational IPv6 assignments.

| Allocator Identifier | Hexadecimal value | Derived prefix   |
|---------------------:|-------------------|------------------|
|               974848 | 0x000EE000        | 5f01:e:e000::/48 |
|               974849 | 0x000EE001        | 5f01:e:e001::/48 |
|               978943 | 0x000EEFFF        | 5f01:e:efff::/48 |
{: #tab-examples title="Example Derived Prefixes"}

For A = 974849, H is 14 (0xe) and L is 57345 (0xe001).
The complete Example Range contains 4096 (2^12) identifiers, from 0x000EE000 to 0x000EEFFF.
The 12 least significant bits of 0x000EE000 are zero.
Thus, the range maps to a prefix of length 48 - 12 = 36:

~~~
5f01:e:e000::/36
~~~


# Routing and Operational Considerations {#routing}

This document specifies address assignment only.
The principles that follow apply to derived prefixes:

* A derived prefix does not establish connectivity.

* An address assignment does not require another network to accept a route.

* A shared Allocator does not imply a shared routing domain.

* An Allocator can operate disconnected networks.

* Routing policy determines permitted route advertisements and transit.

* A prefix does not indicate contact times, delay, or available capacity.

* Ordinary IPv6 addresses remain available through other assignment mechanisms.
{: spacing="compact"}

An Allocator Identifier alone provides no evidence of reachability.
Operators determine reachability through their routing mechanisms and policies.

This document does not define an inherent administrative domain boundary for the common prefix.
Operators MAY exchange routes and traffic for derived prefixes between administrative domains.
Such exchange is subject to agreement between the operators and to their routing policies.
This design does not guarantee acceptance of derived prefixes on the public Internet.

Existing filters can reject traffic from this block.
Operators need to check filters for unallocated IPv6 space and former 6bone space {{RFC5156}}.
A filter that matches only the SRv6 block 5f00::/16 {{RFC9602}} does not match 5f01::/16.
Broader filters, such as a filter for 5f00::/8, can match both blocks.

If the IPN registry withdraws or reassigns A, the previous Allocator of A loses its authority over P(A) under this document.

An Allocator MUST NOT retain authority over P(A) solely because it previously held A.
A change to the contact information of a registry entry alone does not reassign the Allocator Identifier.
Registry changes do not automatically update deployed addresses, routes, filters, or authorization records.
The previous and new Allocators need to coordinate these updates to prevent simultaneous use of P(A).


# Security Considerations {#security}

An address in a derived prefix exposes the associated Allocator Identifier.
The Allocator Identifier is public information in the IANA registry.
An observer can use it to associate traffic with the Allocator.

An Allocator Identifier is not proof of identity.
An attacker can configure an address in the derived prefix of another Allocator.
The arithmetic mapping does not authenticate packets or authorize route advertisements.
Registry information does not, by itself, authenticate a route origin.

Route leaks and unauthorized route advertisements remain possible.
An operator that accepts a route for a derived prefix needs to verify the origin under its own policy.
This document does not create Resource Public Key Infrastructure (RPKI) {{RFC6480}} certificates.
It does not create Route Origin Authorizations (ROAs) or a new trust anchor.

After reassignment, stale filters can block traffic for the new Allocator of A.
Stale authorization can let the previous Allocator of A continue to originate routes.

More than one party can use the same experimental identifier.
For this reason, rule 5 in {{eligibility}} excludes these identifiers from ordinary use.

Interface identifiers shorter than 64 bits reduce the address space that an attacker needs to scan ({{Section 5 of RFC7421}}).

IPv6 prefix eligibility can create new incentives to request IPN Allocator Identifiers or large identifier ranges.
An aligned range of 2^k identifiers maps to an aggregate of length 48 - k.
The existing IPN assignment procedures were not designed solely to distribute IPv6 address space.


# IANA Considerations {#iana}

## IPv6 Address Space Registry {#iana-space}

IANA is requested to allocate a /16 address block for "IPv6 Prefixes Derived from IPN Allocator Identifiers".
The preferred address block is 5f01::/16.

This block is in 5f00::/8, the former 6bone testing block ({{common-prefix}}), within 4000::/3.
The "IPv6 Address Space" registry {{IANA-IPV6-SPACE}} lists 4000::/3 as "Reserved by IETF".
Allocations from this space require IETF Review {{RFC9812}}.

A 16-bit common prefix and a 32-bit identifier produce a /48.
A smaller block cannot preserve this complete mapping.

IANA is requested to update the "IPv6 Address Space" registry entry for 4000::/3 to reference this document.


## IPv6 Special-Purpose Address Space Registry {#iana-special}

IANA is requested to add the entry that follows to the "IPv6 Special-Purpose Address Space" registry {{IANA-IPV6-SPECIAL}}.
{{RFC6890}} and {{RFC8190}} define the fields.

Address Block:
: 5f01::/16

Name:
: IPv6 Prefixes Derived from IPN Allocator Identifiers

RFC:
: This document

Allocation Date:
: The date that IANA allocates the common prefix

Termination Date:
: N/A

Source:
: True

Destination:
: True

Forwardable:
: True

Globally Reachable:
: True

Reserved-by-Protocol:
: False
{: spacing="compact"}

These values are deliberate initial design choices.
They permit ordinary IPv6 forwarding across administrative boundaries.
"Globally Reachable" indicates that a router can forward these datagrams beyond an administrative domain {{RFC8190}}.
It does not guarantee a route.

The parent entry does not make every derived prefix eligible.
IANA is requested to add this note to the parent entry:
"Address assignment and prefix delegation within this block are subject to the eligibility rules in Section 4 of this document."


## More-Specific Entries {#iana-exceptions}

Some derived prefixes are not for ordinary use.
IANA is requested to add the more-specific entries in {{tab-exceptions}}.
These entries take precedence over the parent entry.

| Address Block    | Name                                  | IPN Identifiers       |
|------------------|---------------------------------------|-----------------------|
| 5f01::/48        | Reserved (IPN Default Allocator)      | 0                     |
| 5f01:e:e000::/36 | Documentation (IPN Example Range)     | 0xEE000-0xEEFFF       |
| 5f01:4000::/18   | Reserved (IPN Experimental Use range) | 0x40000000-0x7FFFFFFF |
| 5f01:8000::/17   | Reserved (IPN Reserved range)         | 0x80000000-0xFFFFFFFF |
{: #tab-exceptions title="Proposed More-Specific Entries"}

Each entry uses the values that follow for the other fields:

RFC:
: This document

Allocation Date:
: The date that IANA allocates the common prefix

Termination Date:
: N/A

Source, Destination, Forwardable, Globally Reachable, and Reserved-by-Protocol:
: False
{: spacing="compact"}

These values match the Documentation entry for 3fff::/20 and rules 4 to 7 of {{eligibility}}.
In each name, the first word gives the IPv6 status, and the parenthetical identifies the IPN identifiers.
For example, 5f01:4000::/18 is reserved IPv6 space and does not permit IPv6 experimental deployment.

A later change to the IPN registry does not remove an IPv6 exclusion specified by this document.
Use of an excluded IPv6 block requires a separate specification and the applicable IANA updates.

IANA does not record individual derived prefixes.
An ordinary assignment in an eligible range becomes usable without a second per-identifier registration ({{eligibility}}).
This document does not request entries for currently unassigned identifiers.

Editor's Note:
The author requests IANA and community review of the entries in this section ({{open-issues}}).


## 'ipn' Scheme URI Allocator Identifiers Registry {#iana-ipn}

This document makes no changes to the "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}}.
It makes no changes to the registration procedures of that registry.


--- back

# Open Issues {#open-issues}
{:removeinrfc="true"}

The issues that follow need review.
Each item gives the current working choice.

Intended publication status:
: The working choice is Experimental ({{experiment}}).
  An Experimental document that updates {{RFC4291}} can require review of this choice.

Allocation lifetime:
: This draft requests an allocation without a scheduled termination date.
  Publication status and allocation lifetime are separate choices.
  Review can choose a different lifetime.

Acceptance and interoperability of the RFC 4291 exception:
: The working choice updates RFC 4291 for the common prefix ({{update-4291}}).
  Review needs to decide whether to accept this exception.
  Review also needs to assess interoperability with hosts and links that assume 64-bit interface identifiers.

Routing scope and registry properties:
: The working choice permits routing between administrative domains, subject to agreement and routing policy.
  The parent entry sets "Globally Reachable" to True.
  An alternative is a limited-domain design, such as the design for 5f00::/16.

Registry treatment of excluded identifiers:
: The working choice is the set of more-specific entries in {{iana-exceptions}}.
  Unassigned identifiers receive no entries.

Effects on IPN assignment policy:
: The working choice makes no change to IPN assignment procedures.
  The Designated Experts for the IPN registry need to review the incentives in {{security}}.

Routing authorization and reassignment procedures:
: The working choice ties authority to the current IPN registry assignment.
  This document defines no new authorization mechanism.
  The source of routing authorization for derived prefixes needs review.


# Changes Since -00 {#changes}
{:removeinrfc="true"}

* Added "Updates: 4291" and specified the interface identifier exception as an update to RFC 4291, as updated by RFC 7136.

* Separated assignment, forwarding, and host address configuration in {{allocator-space}}.

* Added {{slaac}} on SLAAC and link-type constraints.

* Added the Experiment Scope subsection.

* Limited the authorization requirement to address assignment and prefix delegation.

* Stated that forwarding does not require registry queries.

* Completed the fields of the more-specific IANA entries and clarified their names.

* Stated that later IPN registry changes do not remove IPv6 exclusions.

* Removed the claim that former 6bone use makes the block suitable for an experiment.

* Separated allocation lifetime from publication status in the Open Issues appendix.

* Revised the filter, reachability, and reassignment text.
{: spacing="compact"}
