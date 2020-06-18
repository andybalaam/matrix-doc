# MSC0000: matrix.to URI syntax v2

TODO: A general summary

## Proposal

### Current syntax

This summarises the [currently specified matrix.to URI
format](https://matrix.org/docs/spec/appendices#matrix-to-navigation) as an aid
to the reader.

A matrix.to URI has the following format, based upon the specification defined
in RFC 3986:

```
https://matrix.to/#/<identifier>/<extra parameter>?<additional arguments>
```

The `identifier` (required) may be a:

| type | literal value | encoded value |
| ---- | ------------- | ------------- |
| room ID | `!somewhere:example.org` | `!somewhere%3Aexample.org` |
| room alias | `#somewhere:example.org` | `%23somewhere%3Aexample.org` |
| user ID | `@alice:example.org` | `%40alice%3Aexample.org` |
| group ID | `+example:example.org` | `%2Bexample%3Aexample.org` |

The `extra parameter` (optional) is only used in the case of permalinks where an
event ID is referenced:

| type | literal value | encoded value |
| ---- | ------------- | ------------- |
| event ID | `$event:example.org` | `%24event%3Aexample.org` |

The ``<additional arguments>`` and the preceding question mark are optional and
only apply in certain circumstances:

* `via=<server>`
  * One or more servers [should be
    specified](https://matrix.org/docs/spec/appendices#routing) in the format
    `example.org` when linking to a room (including a permalink to an event in a
    room) since room IDs are not currently routable

If multiple ``<additional arguments>`` are present, they should be joined by `&`
characters, as in `https://matrix.to/!somewhere%3Aexample.org?via=example.org&via=alt.example.org`

The components of the matrix.to URI (``<identifier>`` and ``<extra parameter>``)
are to be percent-encoded as per RFC 3986.

Examples of matrix.to URIs are:

* Room alias: ``https://matrix.to/#/%23somewhere%3Aexample.org``
* Room: ``https://matrix.to/#/!somewhere%3Aexample.org?via=example.org&via=alt.example.org``
* Permalink by room: ``https://matrix.to/#/!somewhere%3Aexample.org/%24event%3Aexample.org?via=example.org&via=alt.example.org``
* Permalink by room alias: ``https://matrix.to/#/%23somewhere:example.org/%24event%3Aexample.org``
* User: ``https://matrix.to/#/%40alice%3Aexample.org``
* Group: ``https://matrix.to/#/%2Bexample%3Aexample.org``

### Revised syntax

### Summary of changes

* When permalinking to a specific event, the room ID is no longer required and
  event IDs are now permitted in the identifier position, so URIs like
  `https://matrix.to/#/%24event%3Aexample.org` are now acceptable
  * TODO: To support this, a new client-server API will be defined which
    allows clients to query the mapping from event ID to room ID.
* Clients should prefer creating URIs with room aliases instead of room IDs
  where possible, as it's more human-friendly and `via` params are not needed
* A new, optional `client` parameter allows clients to indicate
  which client shared the URI
    * Clients should identify themselves via a schemeless `https` URL pointing
      to a download / install page, such as:
      * `foo.com`
      * `apps.apple.com/app/foo/id1234`
      * `play.google.com/store/apps/details?id=com.foo.client`
* A new, optional `federated` parameter allows indicating whether a room exists
  on a federating server (assumed to be the default), or if the client must
  connect via the server identified by the room ID or event ID (when set to
  `false`)
* A new, optional `sharer` parameter allows indicating the MXID of the account
  which created the link, in case that is meaningful to include

## Potential issues

## Alternatives

## Security considerations

## Unstable prefix