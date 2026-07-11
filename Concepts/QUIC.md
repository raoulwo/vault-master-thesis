---
tags:
  - networks
  - transport-layer
---
# QUIC

- Standardized by IETF under RFC 9000
- Connection-oriented protocol originally developed by Google, now an Internet Standard
- HTTP/3 is the mapping of HTTP over QUIC
- Quick supported in all major browsers
- Runs over [[User Datagram Protocol (UDP)]]
- Provides reliable communication by implementing
	- Flow control
	- Congestion control
	- Loss detection

## Strengths over [[Transmission Control Protocol (TCP)]]

- Supports client-side connection migration
	- When a client changes their IP address, QUIC manages the migration transparently without interrupting ongoing transfers
- According to [[@puliafito2022]], QUIC doesn't support server-side migration (2022)
- TCP runs in kernel space (pushing changes to TCP stacks requires operating system upgrades)
- QUIC is implemented in user space
- QUIC avoid head-of-line blocking of TCP
- Streams within QUIC connection can be handled independently from one another
	- Packet loss of one stream don't affect packets of other streams
- TCP isn't secure by default, requires TLS
- QUIC is encrypted by default, includes TLS 1.3 handshake to establish connections
- QUIC takes less time to establish connections
	- TCP with TLS takes 3 RTTs to establish connection via TCP handshake
	- QUIC takes one RTT to establish connection towards unknown server
	- QUIC takes zero RTTs to establish connection to known servers
- TCP connections are identified by 4-tuple: source IP, source port, destination IP, destination port 
	- If one of these changes, TCP connection must be re-established
- QUIC defines a set of connection identifiers for each connection, independent of IP or port

## Client-Side Connection Migration

The client-side connection migration of QUIC works as follows:

- After changing address, client sends a non-probing packet to the server
- Server receives the packet and understands that client has migrated to a new address
- Server sets new address as client's primary address (primary path)
- Server starts path validation which verifies the client reachability
	- Server sends `PATH CHALLENGE` frame
	- Client responds with `PATH RESPONSE` frame
	- NOTE: Path validation only done if address has not been validated previously
- If path validation succeeds, connection is migrated to new address

## Server-Side Connection Migration

- As of writing [[@puliafito2022]], migrating a connection to a new server address mid-communication is not possible using the current version of QUIC
- However, QUIC allows migrating a connection to a new server address right after establishing the connection
	- QUIC allows servers to accept connections on one IP address and transfer these connections to a preferred address shortly after handshake
	- While establishing the connection, a server can inform a client of a preferred address by including the `preferred_address` parameter during the [[Transport Layer Security (TLS)]] handshake
	- After confirmed handshake, client starts path validation using preferred address

---

## Links

%% Links to related concepts. %%

- [RFC 9000](https://datatracker.ietf.org/doc/html/rfc9000)
- [QUIC (Wikipedia)](https://en.wikipedia.org/wiki/QUIC)
