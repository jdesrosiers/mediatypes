---
title: REST API Media Types
abbrev:
docname: draft-ietf-httpapi-rest-api-mediatypes-latest
category: info

ipr: trust200902
area: Applications and Real-Time
workgroup: HTTPAPI
keyword: Internet-Draft

stand_alone: yes
pi: [toc, tocindent, sortrefs, symrefs, strict, compact, comments, inline, docmapping]

author:
 -
    ins: R. Polli
    name: Roberto Polli
    org: Digital Transformation Department, Italian Government
    email: robipolli@gmail.com
    country: Italy

normative:
  yaml:
    title: YAML Ain't Markup Language Version 1.2
    date: 2002-10-01
    author:
    - ins: Oren Ben-Kiki
    - ins: Clark Evans
    - ins: Ingy dot Net
    target: https://yaml.org/spec/1.2/spec.html
  oas:
    title: OpenAPI Specification 3.0.0
    date: 2017-07-26
    author:
    - ins: Darrel Miller
    - ins: Jeremy Whitlock
    - ins: Marsh Gardiner
    - ins: Mike Ralphson
    - ins: Ron Ratovsky
    - ins: Uri Sarid

informative:

--- abstract

This document registers
the following media types used in APIs
on the IANA Media Types registry:
application/yaml,
application/openapi+json,
and application/openapi+yaml.

--- note_Note_to_Readers

*RFC EDITOR: please remove this section before publication*

Discussion of this draft takes place on the HTTP APIs working group
mailing list (httpapi@ietf.org), which is archived at
[https://mailarchive.ietf.org/arch/browse/httpapi/](https://mailarchive.ietf.org/arch/browse/httpapi/).

The source code and issues list for this draft can be found at
[https://github.com/ioggstream/draft-polli-rest-api-mediatypes](https://github.com/ioggstream/draft-polli-rest-api-mediatypes).

--- middle

# Introduction

OpenAPI Specification [oas] version 3 and above
is a consolidated standard for describing
HTTP APIs using the JSON {{!JSON=RFC8259}} and yaml [yaml] data format.

To increase interoperability when processing API specifications
and leverage content negotiation mechanisms when exchanging
OpenAPI Specification resources
this specification register the following media-types:
`application/yaml`,
`application/openapi+json`
and `application/openapi+yaml`.

## Notational Conventions

{::boilerplate bcp14+}

This document uses the Augmented BNF defined in {{!RFC5234}} and updated
by {{!RFC7405}}.

# Media Type registrations

This section describes the information required to register
the above media types.

## Media Type application/yaml {#application-yaml}

The following information serves as the registration form for the `application/yaml` media type.

Type name:  application

Subtype name:  yaml

Required parameters:  None

Optional parameters:  None; unrecognized parameters should be ignored

Encoding considerations:  Same as {{JSON}}

Security considerations:  see {{security-considerations}} of this document

Interoperability considerations:  None

Published specification: (this document)

Applications that use this media type:  HTTP

Fragment identifier considerations:  Same as for application/json
  {{JSON}}

Additional information:

  Deprecated alias names for this type:  application/x-yaml, text/yaml, text/x-yaml

  Magic number(s):  n/a

  File extension(s):  yaml, yml

  Macintosh file type code(s):  n/a

Person and email address to contact for further information:
  See Authors' Addresses section.

Intended usage:  COMMON

Restrictions on usage:  None.

Author:  See Authors' Addresses section.

Change controller:  n/a

## The OpenAPI Media Types

The OpenAPI Specification Media Types convey OpenAPI document (OAS) files
as defined in [oas] for version 3.0.0 and above.

Those files can be serialized in {{JSON}} or [yaml].
Since there are multiple OpenAPI Specification versions,
those media-types support the `version` parameter.

The following examples conveys the desire of a client
to receive an OpenAPI Specification resource preferably in the following
order:

1. openapi 3.1 in yaml
2. openapi 3.0 in yaml
3. any openapi version in json

~~~ example

Accept: application/openapi+yaml;version=3.1,
        application/openapi+yaml;version=3.0;q=0.5,
        application/openapi+json;q=0.3
~~~

### Media Type application/openapi+json {#openapi-json}

The following information serves as the registration form for the `application/openapi+json` media type.

Type name:  application

Subtype name:  openapi+json

Required parameters:  None

Optional parameters:  version; unrecognized parameters should be ignored

Encoding considerations:  Same as {{JSON}}

Security considerations:  see {{security-considerations}} of this document

Interoperability considerations:  None

Published specification: (this document)

Applications that use this media type:  HTTP

Fragment identifier considerations:  Same as for application/json
  {{JSON}}

Additional information:

  Deprecated alias names for this type:  n/a

  Magic number(s):  n/a

  File extension(s):  json

  Macintosh file type code(s):  n/a

Person and email address to contact for further information:
  See Authors' Addresses section.

Intended usage:  COMMON

Restrictions on usage:  None.

Author:  See Authors' Addresses section.

Change controller:  n/a

### Media Type application/openapi+yaml {#openapi-yaml}

The following information serves as the registration form for the `application/openapi+yaml` media type.

Type name:  application

Subtype name:  openapi+yaml

Required parameters:  None

Optional parameters:  version; unrecognized parameters should be ignored

Encoding considerations:  Same as {{JSON}}

Security considerations:  see {{security-considerations}} of this document

Interoperability considerations:  None

Published specification: (this document)

Applications that use this media type:  HTTP

Fragment identifier considerations:  Same as for application/json
  {{JSON}}

Additional information:

  Deprecated alias names for this type:  n/a

  Magic number(s):  n/a

  File extension(s):  yaml, yml

  Macintosh file type code(s):  n/a

Person and email address to contact for further information:
  See Authors' Addresses section

Intended usage:  COMMON

Restrictions on usage:  None.

Author:  See Authors' Addresses section

Change controller:  n/a

# Interoperability Considerations

## YAML and JSON {#sec-yaml-and-json}

Since YAML [yaml] is a superset of JSON [JSON],
the same interoperability considerations apply when using that syntax.
It is important to note though, that when serializing a YAML document
in JSON, information can be discarded:
this includes comments (see Section 3.2.3.3 of [yaml])
and alias nodes (see Section 7.1 of [yaml])
that do not have a JSON counterpart.

~~~ example
# This comment will be lost
# when serializing in JSON.
Title:
  type: string
  maxLength: &text_limit 64

Name:
  type: string
  maxLength: *text_limit  # Replaced by the value 64.
~~~
{: title="JSON replaces alias nodes with static values." #example-json-discards-information}

When using YAML
to serialize information to be consumed in JSON,
implementers need to verify
that information that they consider relevant
will not be lost during the processing.
For example, they might consider acceptable
that alias nodes are replaced by static values.

In some cases an implementer can adopt a restricted YAML schema
such as the "YAML Failsafe schema" (see Section 10.1 of [yaml])
or limit to the admitted tags.
As an example, one could decide to only support the following ones:

- `!!str`, `!!int`, `!!map`, `!!seq`;
- `!!float` without the `.inf` and `.nan` values;
- `!!merge`

YAML features that may have interoperability
issues with JSON include:

- non UTF-8 encoding, since YAML supports UTF-16 and UTF-32 in addition to UTF-8;
- `.inf` and `.nan` float values,
  despite the fact they are enumerated in the "YAML JSON schema"
  (see Section 10.2.1.4 of [yaml]);
- non-JSON types, including  the following tags:
  `!!timestamp`, `!!set`, `!!omap` and `!!pairs`;
- custom and local tags such as `!!python/object` and
  `!mytag` (see Section 2.4 of [yaml]);
- mapping keys that are non-scalar or of non-JSON types (see Section 2.2 of [yaml]);

~~~ example
non-json-keys:
  2020-01-01: a timestamp
  [0, 1]: a sequence
  ? {k: v}
  : a map
~~~
{: title="Example of mapping keys not supported in JSON" #example-unsupported-keys}

- circular references represented using anchor (see {{sec-exhaustion}}
  and {{example-yaml-cyclic}}).


# Security Considerations

Security requirements for both media type and media type suffix
registrations are discussed in Section 4.6 of {{!MEDIATYPE=RFC6838}}.

## YAML media types

### Arbitrary code execution {#sec-code-execution}

YAML has some features like explicit typing (e.g. `!!omap`) and local tags that,
depending on the implementation, might trigger unexpected code execution.

~~~ python
document = "!!python/object/apply:os.system ['echo boom!']"
yaml.unsafe_load(document)
# boom!
~~~

Code execution in deserializers should be disabled by default,
and only be enabled explicitly.
In those cases, the implementation should ensure - for example, via specific functions -
that the code execution results in strictly bounded time/memory limits.

Many implementations provide safe deserializers addressing these issues
(e.g. the `yaml.safe_load` function in `pyyaml`, ...).

### Resource exhaustion {#sec-exhaustion}

YAML documents are rooted, connected, directed graph
and can contain reference cycles,
so they can't be treated as simple trees (see Section 3.2.1 of [yaml]).
An implementation that attempts to do that
can infinite-loop at some point (e.g. when trying to serialize a YAML document in JSON).

~~~ yaml
x: &x
  y: *x
~~~
{: title="A cyclic document" #example-yaml-cyclic}

Even if a document is not cyclic, treating it as a tree could lead to improper behaviors
(such as the "billion laughs" problem).

~~~ yaml
x1: &a1 ["a", "a"]
x2: &a2 [*a1, *a1]
x3: &a3 [*a2, *a2]
~~~
{: title="A billion laughs document" #example-yaml-1e9lol}

This can be addressed using processors limiting the anchor recursion depth
and validating the input before processing it;
even in these cases it is important
to carefully test the implementation you are going to use.
The same considerations apply when serializing a YAML representation graph
in a format that do not support reference cycles (see {{sec-yaml-and-json}}).

# IANA Considerations

This specification defines the following new Internet media types {{MEDIATYPE}}.

IANA has updated the "Media Types" registry at <https://www.iana.org/assignments/media-types>
with the registration information provided below.

|--------------------------|---------------------------------------|
| Media Type               |                         Section       |
|--------------------------|---------------------------------------|
| application/yaml         | {{application-yaml}} of ThisRFC       |
| application/openapi+yaml | {{openapi-yaml}} of ThisRFC           |
| application/openapi+json | {{openapi-json}} of ThisRFC           |
|--------------------------|---------------------------------------|

--- back

# Acknowledgements

Thanks to Erik Wilde and David Biesack for being the initial contributors of this specification,
and to Darrel Miller and Rich Salz for their support during the adoption phase.

In addition to the people above, this document owes a lot to the extensive discussion inside
and outside the HTTPAPI workgroup,
including Eemeli Aro and Ingy dot Net.


# FAQ
{: numbered="false"}

Q: Why this document?
:  After all these years, we still lack a proper media-type for yaml.
   This has some security implications too
   (eg. wrt on identifying parsers or treat downloads)

# Change Log
{: numbered="false"}

RFC EDITOR PLEASE DELETE THIS SECTION.
