---
title: "IPv6 Prefixes Derived from IPN Allocator Identifiers"
abbrev: "IPN Allocator Prefixes"
category: exp

docname: draft-ipn-allocator-prefixes-latest
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
  latest: "https://ekline.github.io/draft-ipn-allocator-prefixes/draft-ipn-allocator-prefixes.html"

author:
 -
    fullname: "Erik Kline"
    organization: "Aalyria Technologies Inc."
    email: "ek.ietf@gmail.com"

normative:
  RFC4291:
  RFC6890:
  RFC8190:
  RFC9758:
  IANA-IPN:
    title: "'ipn' Scheme URI Allocator Identifiers"
    author:
      org: IANA
    target: https://www.iana.org/assignments/uri-schemes

informative:
  RFC5952:
  RFC6480:
  RFC8949:
  RFC9171:
  RFC9602:
  RFC9812:
  RFC1897:
  RFC7421:
  RFC7608:
  RFC3701:
  RFC5156:
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


--- middle

# Introduction {#intro}

An IPN Allocator Identifier {{RFC9758}} identifies an authority that assigns IPN Node Numbers.
This document associates an optional IPv6 prefix with that authority.
The association does not describe network topology.
It does not identify a next hop or establish a route.

An Allocator can use the derived prefix to assign IPv6 addresses within networks under its administration.
An Allocator can also delegate parts of its derived prefix to other parties under its own policy.
Delegation does not transfer the Allocator Identifier.
The method reuses the existing "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}}.
It does not require a new per-identifier registry.


## Scope {#scope}

This document defines an address assignment mechanism.
The mechanism applies to terrestrial networks and to space networks.

This document does NOT define:

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

Physical location does NOT determine network connectivity or transit policy.
This document does NOT assign prefixes to celestial bodies.

An Allocator can retain its derived prefix when its equipment changes location.
Continued reachability depends on the available routes and applicable routing policy.
This document does not change routing scalability nor define a mobility mechanism.


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
Eligibility under this document is different from permission to deploy the requested prefix today.
This document does not authorize deployment of 5f01::/16 before that allocation.
Until then, an Allocator MUST NOT treat an eligible identifier as permission to use 5f01::/16.

The requested prefix is in 5f00::/8.
The first instance of the 6bone experimental network used 5f00::/8 for IPv6 testing {{RFC1897}} {{RFC3701}}.
That block was later returned to IANA for future allocation {{RFC5156}}.
This document specifies an experiment.
Thus, the former 6bone testing block is a suitable source for the common prefix.

{{RFC9602}} assigned 5f00::/16 for Segment Routing over IPv6 (SRv6) Segment Identifiers (SIDs).
After that assignment, the free space in 5f00::/8 is eight aligned blocks, from 5f01::/16 up to 5f80::/9.
Allocation of 5f01::/16 uses one of these blocks completely.
Allocation of any other /16 splits a larger block into smaller blocks.
Thus, this document requests the next available /16.
The mechanism does not depend on the specific /16 value or on SRv6.

Note to the RFC Editor:
Replace "5f01" with the value that IANA assigns, then remove this note.


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
The length of the decimal text form has no effect on the field.
The length of the Concise Binary Object Representation (CBOR) {{RFC8949}} encoding also has no effect.
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
The entire /48 is available to the Allocator.
Only the address assignment and routing policies of the Allocator govern its use.

This document does not require a particular structure or encoding in these 80 bits.
In particular, it does not require a /64 subnet boundary or a 64-bit interface identifier.
The Allocator can assign or delegate prefixes of any length within P(A).
IPv6 forwarding supports prefixes of any length {{RFC7608}}.
The policy of the Allocator determines whether to use mechanisms that depend on 64-bit interface identifiers {{RFC7421}}.

{{Section 2.5.1 of RFC4291}} requires 64-bit interface identifiers for most unicast addresses.
Addresses in the common prefix are an exception to that requirement.
{{RFC9602}} describes a similar exception for SRv6 SIDs in 5f00::/16.
This document might need to update {{RFC4291}} to make this exception formal ({{open-issues}}).


# Eligibility and Optional Use {#eligibility}

The current IANA "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}} determines eligibility.
The rules that follow apply to each Allocator Identifier A.

1. Ordinary assignment:
   A current ordinary assignment of A permits the Allocator of A to use P(A).
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
   Their derived prefixes are not uniquely assigned.
   This document does not specify operational use of these derived prefixes.

6. Example Range:
   The IPN Example Range (974848 to 978943) is for documentation ({{Section 9.1 of RFC9758}}).
   Its derived prefixes are not for operational deployment.

7. Allocator Identifier zero:
   Allocator Identifier zero does not permit ordinary use under this specification.
   {{default-allocator}} gives the reason.
{: spacing="compact"}

An Allocator Identifier is an eligible identifier only if rule 1 applies and rules 3 to 7 do not exclude it.
No other text in this document makes an Allocator Identifier eligible.
A party MUST NOT use addresses from P(A) under this specification unless both conditions that follow are true:

* A is an eligible identifier.

* The party is the Allocator of A, or the Allocator of A has authorized the party.
{: spacing="compact"}


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

An operator MUST NOT infer reachability from the Allocator Identifier alone.
Use of a derived prefix does not require another network to accept traffic or routing information for that prefix.

This document does not define an inherent administrative domain boundary for the common prefix.
Operators MAY exchange routes and traffic for derived prefixes between administrative domains.
Such exchange is subject to agreement between the operators and to their routing policies.
This design does not guarantee acceptance of derived prefixes on the public Internet.

Some networks filter 5f00::/8 as unallocated former 6bone space {{RFC5156}}.
Some networks also filter 5f00::/16 at Segment Routing domain boundaries {{RFC9602}}.
A filter that covers more than 5f00::/16 also blocks derived prefixes.
An operator that uses derived prefixes needs to confirm that such filters do not block its traffic.

Eligibility follows the current registry assignment of each Allocator Identifier.
If the registry withdraws or reassigns A, the previous Allocator of A loses its authority to use P(A) under this document.
Delegations from the previous Allocator also lose their basis.
An Allocator MUST NOT retain authority over P(A) solely because it previously held A.
The change can require address changes, route withdrawal, and updates to authorization records.
The previous and new Allocators need to coordinate to prevent simultaneous use of P(A).
This document does not define a reassignment delay or an automatic revocation protocol.


# Security Considerations {#security}

An address in a derived prefix exposes the associated Allocator Identifier.
The Allocator Identifier is public information in the IANA registry.
An observer can use it to associate traffic with the Allocator.

An Allocator Identifier is not proof of identity.
An attacker can configure an address in the derived prefix of another Allocator.
The arithmetic mapping does not authenticate packets.
It also does not authorize route advertisements.

Route leaks and unauthorized route advertisements remain possible.
An operator that accepts a route for a derived prefix needs to verify the origin under its own policy.
This document does not create Resource Public Key Infrastructure (RPKI) {{RFC6480}} certificates.
It does not create Route Origin Authorizations (ROAs) or a new trust anchor.
Routing authorization for derived prefixes is an issue for review ({{open-issues}}).

Reassignment can leave stale configuration and stale authorization.
Stale filters can block traffic for the new Allocator of A.
Stale authorization can let the previous Allocator of A continue to originate routes.

Experimental identifiers do not provide exclusive ownership.
More than one party can use the same experimental identifier.
For this reason, rule 5 in {{eligibility}} excludes these identifiers from ordinary use.

IPv6 prefix eligibility can create new incentives to request IPN Allocator Identifiers.
It can also create incentives to request large identifier ranges.
An aligned range of 2^k identifiers maps to an aggregate of length 48 - k.
The existing IPN assignment procedures were not designed solely to distribute IPv6 address space.
Coordination of registry policy is an issue for review ({{open-issues}}).


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
: The date of allocation by IANA

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
It does not require acceptance on the public Internet.

The parent entry does not authorize use of every derived prefix.
The eligibility rules in {{eligibility}} still apply.
IANA is requested to add this note to the parent entry:
"Use of a prefix in this block requires eligibility under this document. The block properties do not authorize use of any derived prefix."


## More-Specific Entries {#iana-exceptions}

Some derived prefixes are not for ordinary use.
IANA is requested to add more-specific entries for these prefixes.
These entries take precedence over the parent entry.

| Address Block    | Name                        | Allocator Identifiers    |
|------------------|-----------------------------|--------------------------|
| 5f01::/48        | Reserved (Default)          | 0                        |
| 5f01:e:e000::/36 | Documentation               | 0xEE000-0xEEFFF          |
| 5f01:4000::/18   | Reserved (Experimental Use) | 0x40000000-0x7FFFFFFF    |
| 5f01:8000::/17   | Reserved                    | 0x80000000-0xFFFFFFFF    |
{: #tab-exceptions title="Proposed More-Specific Entries"}

For each entry, IANA is requested to set all five properties to False.
These values match the Documentation entry for 3fff::/20 and rules 4 to 7 of {{eligibility}}.

This document does not request entries for unassigned Allocator Identifiers.
The set of unassigned identifiers changes as the IPN registry assigns identifiers.
The note on the parent entry covers these identifiers.

Editor's Note:
The entries in this section are a proposal for review by IANA and the community.
Their form, names, and properties are not settled policy.
A future document can update these entries.
{{open-issues}} lists this issue.


## 'ipn' Scheme URI Allocator Identifiers Registry {#iana-ipn}

This document makes no changes to the "'ipn' Scheme URI Allocator Identifiers" registry {{IANA-IPN}}.
It makes no changes to the registration procedures of that registry.


--- back

# Open Issues {#open-issues}
{:removeinrfc="true"}

The issues that follow need review.
Each item gives the current working choice.

Intended publication status:
: The working choice is Experimental.
  An IPv6 address allocation does not expire with an experiment.
  Thus, Informational or Standards Track status can be more suitable.
  Review needs to decide the final status.

Routing scope and registry properties:
: The working choice permits routing between administrative domains, subject to agreement and routing policy.
  The parent entry sets "Globally Reachable" to True.
  An alternative is a limited-domain design, such as the design for 5f00::/16.

Registry treatment of excluded identifiers:
: The working choice is the set of more-specific entries in {{iana-exceptions}}.
  Unassigned identifiers receive no entries.
  IANA review can change this treatment.

Effects on IPN assignment policy:
: The working choice makes no change to IPN assignment procedures.
  The Designated Experts for the IPN registry need to review the incentives in {{security}}.

Relation to the RFC 4291 interface identifier rule:
: The working choice imposes no /64 boundary within a derived prefix ({{allocator-space}}).
  This conflicts with the 64-bit interface identifier rule in {{Section 2.5.1 of RFC4291}}.
  {{RFC9602}} states a similar exception without a formal update to RFC 4291.
  This document follows that approach for now.
  Unlike SRv6 SIDs, addresses in derived prefixes can be assigned to host interfaces.
  Thus, review can require a formal update.
  In that case, this document will add "Updates: 4291".

Routing authorization and reassignment procedures:
: The working choice ties authority to the current IPN registry assignment.
  This document defines no new authorization mechanism.
  The source of routing authorization for derived prefixes needs review.
