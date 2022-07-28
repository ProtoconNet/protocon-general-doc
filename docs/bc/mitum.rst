===================================================
Mitum Protocol
===================================================

---------------------------------------------------
What is MITUM?
---------------------------------------------------

| **Mitum** is a general privacy blockchain that is flexible and resilient.

| Mitum can be used for various kind of purposes.

* public and private blockchain like cryptocurrency network
* data-centric blockchain for arbitrary data
* secure anonymity voting system

| If you want to know more about **MITUM**, visit `Mitum Document <https://mitum-doc.readthedocs.io/en/proto2/index.html>`_.

---------------------------------------------------
Mitum Technical SPEC
---------------------------------------------------

* Mitum (blockchain core framework) uses *ISAAC+ consensus protocol* based on *PBFT*.
* The network transport protocol is `quic <https://en.wikipedia.org/wiki/QUIC>`_ (based on udp).
* *Gossip-Based* Node Discovery Protocol.
* The main storage engine of the blockchain uses *MongoDB* and the local file system is used for block storage.
* Parallel operation processing
* Main hash algorithm: `Keccak <https://keccak.team>`_ 256, SHA-3
* Supports multiple hash algorithm: ``Keccak 256``, ``Keccak 512``, ``Raw bytes.``
* Supports multiple message serialization format: *JSON*, *BSON*
* Small amount of code.
* *JSON logging*