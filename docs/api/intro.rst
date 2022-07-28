.. _api:

===================================================
REST API
===================================================

| **Digest API** is a service that allows nodes to search blockchain data.
| It can be used in various applications, such as wallet or blockchain explorer.

* API is provided through *HTTP/2 network protocol*.
* Response message follows `HAL <https://datatracker.ietf.org/doc/html/draft-kelly-json-hal-08>`_ and is delivered in *JSON* format.
* API data storage can be set in :ref:`config` of Mitum Currency.
* Mitum's main storage can be used, or a separate database is also possible.
* *TLS certificates* required for *HTTP/2* will randomly generate self signed certificates if the service host is localhost unless the path of the file is set separately.

| If an operation is not included in the block due to a specific problem, the cause can be checked through the response.

---------------------------------------------------
Summary
---------------------------------------------------

| This is the summary of REST API for Mitum Currency.

+----------------------------------------+-----------------------+------------------------------------+
| REQUEST URL                            | METHOD                | RESPONSE                           |
+========================================+=======================+====================================+
| /                                      | GET                   | Node information                   |
+----------------------------------------+-----------------------+------------------------------------+
| /block/manifests                       | GET                   | All block manifests                |
+----------------------------------------+-----------------------+------------------------------------+
| /block/{height}                        | GET                   | Block by block height              |
+----------------------------------------+-----------------------+------------------------------------+
| /block/{height}/manifest               | GET                   | Block manifest by block height     |
+----------------------------------------+-----------------------+------------------------------------+
| /block/{height}/operations             | GET                   | All operations of block            |
+----------------------------------------+-----------------------+------------------------------------+
| /block/{block_hash}                    | GET                   | Block by block hash                |
+----------------------------------------+-----------------------+------------------------------------+
| /block/{block_hash}/manifest           | GET                   | Block manifest by block hash       |
+----------------------------------------+-----------------------+------------------------------------+
| /block/operations                      | GET                   | All operations                     |
+----------------------------------------+-----------------------+------------------------------------+
| /block/operation/{fact_hash}           | GET                   | Operation by fact hash             |
+----------------------------------------+-----------------------+------------------------------------+
| /account/{address}                     | GET                   | Latest state of account            |
+----------------------------------------+-----------------------+------------------------------------+
| /account/{address}/operations          | GET                   | Operations related to account      |
+----------------------------------------+-----------------------+------------------------------------+
| /accounts?publickey={public_key}       | GET                   | Accounts related to public key     |
+----------------------------------------+-----------------------+------------------------------------+
| /builder/operation                     | GET                   | Available Operation types          |
+----------------------------------------+-----------------------+------------------------------------+
| /builder/operation/fact/template/{fact}| GET                   | Fact template                      |
+----------------------------------------+-----------------------+------------------------------------+
| /builder/operation/fact                | POST {fact}           | Operation message from fact        |
+----------------------------------------+-----------------------+------------------------------------+
| /builder/operation/sign                | POST {operation}      | Operation with hash from operation |
+----------------------------------------+-----------------------+------------------------------------+
| /builder/send                          | POST {operation/seal} | Broadcast seal                     |
+----------------------------------------+-----------------------+------------------------------------+
| /currency                              | GET                   | All currencies                     |
+----------------------------------------+-----------------------+------------------------------------+
| /currency/{currency_id}                | GET                   | Currency by currency id            |
+----------------------------------------+-----------------------+------------------------------------+

| Refer to `Mitum Currency Digest API Docs <https://rapidoc.test.protocon.network/>`_ for details.

| This document doesn't provide any information of APIs for Mitum Blocksign or Mitum Blockcity. See `Mitum Blocksign Digest API Docs <https://rapidoc.blocksign.protocon.network>`_ for Mitum Blocksign. 