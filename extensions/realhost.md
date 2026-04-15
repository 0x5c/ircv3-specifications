---
title: Realhost and User IP Tags
layout: spec
work-in-progress: true
copyrights:
  -
    name: "0x5c"
    email: "irc@0x5c.io"
    period: "2026"
---

## Notes for implementing work-in-progress version

This is a work-in-progress specification.

Software implementing this work-in-progress specification MUST NOT use the
unprefixed `realhost` and `ip` tags or `realhost-tag` capability name. Instead, implementations SHOULD
use the `draft/realhost` and `draft/ip` tags and `draft/realhost-tag` capability name to be interoperable with other
software implementing a compatible work-in-progress version.

The final version of the specification will use an unprefixed capability name and unprefixed tag names.


## Introduction

This specification introduces two new capability-gated message tags that allow servers to indicate
the real hostname and IP address of a user.

## Motivation

Network moderation scripts sometimes need the real hostname and/or IP address of a user.
TODO: finish

## Architecture

### Capabilities

This specification adds the `draft/realhost-tag` capability.

When negotiated, it enables the optional `draft/realhost` and `draft/ip` message tags.

### Tags

This specification adds the `draft/realhost` and `draft/ip`, for messages sent from server to client.

The realhost tag contains the real resolved hostname of the source user.

The ip tag contains either the source user's IPv4 address in dotted-decimal format,
or their IPv6 address in canonical format.

TODO: better way to define the format of these tags?? RFCs?
 -> Is it even viable to assume address representation formats?

TODO: Should it be explicitly-stated that servers may ommit either tag for any reason?

## Security Considerations

[choice]
option1:
Where a [source?] user has their IP address or hostname hidden [with a vhost/cloak/etc?], servers SHOULD
limit inclusion of those tags to clients with elevated priviledges.

option2:
Server implementations SHOULD restrict inclusion of a vhost/cloak user's real host
and IP address to clients with elevated privileges.
[/choice]

Server implementations MAY include the `draft/ip` tag for [source?] users
with a non-hidden host.

### Examples

This section is non-normative.

Exchange between 3 users with a hidden host, visible host, and plain ip, respectively,
seen from a client with sufficient priviledges
```
@draft/realhost=naptime.example,draft/ip=3fff::3c :cat!purr@meow/meowmeowmeow PRIVMSG #townsquare :guys, check out my cool new cloak
@draft/ip=198.51.100.240 :fox!vulpes@gekkering.example PRIVMSG #townsquare :why get cloak when domain do trick?
:dog!bite@203.0.113.197 PRIVMSG #townsquare :wait, i can hide my IP??
```
and from one without
```
:cat!purr@meow/meowmeowmeow PRIVMSG #townsquare :guys, check out my cool new cloak
@draft/ip=198.51.100.240 :fox!vulpes@gekkering.example PRIVMSG #townsquare :why get cloak when domain do trick?
:dog!bite@203.0.113.197 PRIVMSG #townsquare :wait, i can hide my IP??
```

## Alternatives

This section is non-normative.

It would be possible to rewrite the hostname in the source of messages sent to clients with
sufficiently elevated priviledges. However, such a scheme would break basic protocol assumptions, potentially causing
a number of problems like automated ban scripts leaking real hostnames.
Rewriting the source also limits network operators from seeing more than either the real hostname, ip, or publicly-
visible hostname of a user at once without further lookup commands.
