===================================================
API List
===================================================

| This is the page of the explanation for each API Path.

| For details, visit `Mitum Currency Digest API Docs <https://rapidoc.test.protocon.network/>`_.

---------------------------------------------------
Node Info
---------------------------------------------------

/
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the node information of the network. 

+-------+--------+
| PATH  | METHOD |
+=======+========+
| /     | GET    |
+-------+--------+
 
| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "node-info-v0.0.1",
        "_embedded": {
            "_hint": "node-info-v0.0.1",
            "node": {
                "_hint": "base-node-v0.0.1",
                "address": "node4sas",
                "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu"
            },
            "network_id": "bWl0dW0=",
            "state": "CONSENSUS",
            "last_block": {
                "_hint": "block-manifest-v0.0.1",
                "hash": "7KjDLJMMzKw6RtfwjoZZ75rcab15mn6ASjQbGjotX1NW",
                "height": 1622504,
                "round": 0,
                "proposal": "H9ztzWj46ayufSvBXjdNXo7Xs3q2DK8nj9exaMPo1iyt",
                "previous_block": "JeT7t26J279p3EWq1S1yAEdXUqu4EYZa9tprT9nkMCy",
                "block_operations": null,
                "block_states": null,
                "confirmed_at": "2022-02-03T04:47:01.355363841Z",
                "created_at": "2022-02-03T04:47:01.361237906Z"
            },
            "version": "v0.0.1-stable-383cf0c-20211224",
            "policy": {
                "interval_broadcasting_accept_ballot": 1000000000,
                "timespan_valid_ballot": 60000000000,
                "max_operations_in_seal": 10,
                "interval_broadcasting_proposal": 1000000000,
                "wait_broadcasting_accept_ballot": 1000000000,
                "interval_broadcasting_init_ballot": 1000000000,
                "network_connection_timeout": 3000000000,
                "suffrage": "{\"cache_size\":10,\"number_of_acting\":1,\"type\":\"\"}",
                "threshold": 100,
                "max_operations_in_proposal": 100,
                "timeout_waiting_proposal": 5000000000
            },
            "suffrage": [
                {
                    "address": "node4sas",
                    "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu",
                    "conninfo": {
                        "_hint": "http-conninfo-v0.0.1",
                        "url": "https://3.35.171.179:54321",
                        "insecure": true
                    }
                }
            ],
            "conninfo": {
                "_hint": "http-conninfo-v0.0.1",
                "url": "https://3.35.171.179:54321",
                "insecure": true
            }
        },
        "_links": {
            "block:{hash}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "currency:{currencyid}": {
                "templated": true,
                "href": "/currency/{currencyid:.*}"
            },
            "block:current": {
                "href": "/block/1622504"
            },
            "block:current-manifest": {
                "href": "/block/1622504/manifest"
            },
            "block:manifest:{hash}": {
                "templated": true,
                "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
            },
            "block:next": {
                "href": "/block/1622505"
            },
            "block:prev": {
                "href": "/block/1622503"
            },
            "self": {
                "href": "/"
            },
            "currency": {
                "href": "/currency"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "block:manifest:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}/manifest"
            }
        }
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

---------------------------------------------------
Block
---------------------------------------------------

/block/manifests  
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all block manifests of the network.

+--------------------+--------+
| PATH               | METHOD |
+====================+========+
| /block/manifests   | GET    |
+--------------------+--------+

+---------+---------------------------------------+-----------------------+
| Query   |                                       | Example               |
+=========+=======================================+=======================+
| offset  | manifests after offset - block height | 2                     |
+---------+---------------------------------------+-----------------------+
| reverse | manifests by reverse order            | 1 (true)              |
+---------+---------------------------------------+-----------------------+

* offset: integer; block height
* reverse: boolean; use ``1`` for ``true``

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_embedded": [
            {
                "_hint": "mitum-currency-hal-v0.0.1",
                "hint": "block-manifest-v0.0.1",
                "_embedded": {
                    "_hint": "block-manifest-v0.0.1",
                    "hash": "F3qqMUDjofiLkftSSHn4N6uZYBppQzc48iKs8Aqupe9b",
                    "height": 1,
                    "round": 0,
                    "proposal": "34VNRjGW3TqrQ455dyqoKp1EDUeAvu3VfnyLu3aZDcur",
                    "previous_block": "6AMoeUTpDfF2Vs73HRWRCVfkqnVLs6gSwUpXYbJzDmAV",
                    "block_operations": null,
                    "block_states": null,
                    "confirmed_at": "2021-12-26T04:22:10.627Z",
                    "created_at": "2021-12-26T04:22:10.639Z"
                },
                "_links": {
                    "next": {
                        "href": "/block/2/manifest"
                    },
                    "block": {
                        "href": "/block/1"
                    },
                    "block:{height}": {
                        "templated": true,
                        "href": "/block/{height:[0-9]+}"
                    },
                    "block:{hash}": {
                        "href": "/block/{height:[0-9]+}",
                        "templated": true
                    },
                    "manifest:{height}": {
                        "templated": true,
                        "href": "/block/{height:[0-9]+}/manifest"
                    },
                    "manifest:{hash}": {
                        "templated": true,
                        "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
                    },
                    "self": {
                        "href": "/block/1/manifest"
                    },
                    "alternate": {
                        "href": "/block/F3qqMUDjofiLkftSSHn4N6uZYBppQzc48iKs8Aqupe9b/manifest"
                    }
                }
            },
            ...
        ],
        "_links": {
            "next": {
            "href": "/block/manifests?offset=10"
            },
            "reverse": {
            "href": "/block/manifests?reverse=1"
            },
            "self": {
            "href": "/block/manifests?offset=0"
            }
        }
    }

* 404 (No more manifests)

| If there are no more manifests, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "manifests not found",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/{height}  
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the block information of the block by *block height*.

+--------------------+--------+
| PATH               | METHOD |
+====================+========+
| /block/{height}    | GET    |
+--------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_links": {
            "self": {
                "href": "/block/5"
            },
            "manifest:{hash}": {
                "templated": true,
                "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
            },
            "prev": {
                "href": "/block/4"
            },
            "current": {
                "href": "/block/5"
            },
            "current-manifest": {
                "href": "/block/5/manifest"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "block:{hash}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "manifest:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}/manifest"
            },
            "next": {
                "href": "/block/6"
            }
        }
    }

* 400 (block not found)

| If the height you request with is wrong, it returns ``400``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "bad request; invalid height found for block by height: strconv.ParseInt: parsing \"...\": value out of range",
        "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/{height}/manifest
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the block manifest of the block by *block height*.

+-----------------------------+--------+
| PATH                        | METHOD |
+=============================+========+
| /block/{height}/manifest    | GET    |
+-----------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "block-manifest-v0.0.1",
        "_embedded": {
            "_hint": "block-manifest-v0.0.1",
            "hash": "9zVqaLhLngT8gmTUXfRNLo7WxGQBYoZkYw4NSDrKTrvX",
            "height": 222,
            "round": 0,
            "proposal": "66yixQwnHwBaJ4qDfpwsTa2tBgDGXYHGT1Nta7jD24S1",
            "previous_block": "CyPXbZUAhRb5dH2cJHJtfw51H73NwLSkyz1Ad7iWrpDc",
            "block_operations": null,
            "block_states": null,
            "confirmed_at": "2021-12-26T04:29:44.869Z",
            "created_at": "2021-12-26T04:29:44.877Z"
        },
        "_links": {
            "alternate": {
                "href": "/block/9zVqaLhLngT8gmTUXfRNLo7WxGQBYoZkYw4NSDrKTrvX/manifest"
            },
            "next": {
                "href": "/block/223/manifest"
            },
            "block": {
                "href": "/block/222"
            },
            "block:{hash}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "manifest:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}/manifest"
            },
            "manifest:{hash}": {
                "templated": true,
                "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "self": {
                "href": "/block/222/manifest"
            }
        }
    }

* 400 (manifest not found)

| If the height you request with is wrong, it returns ``400``.

.. code-block:: json

    {
        "title": "invalid height found for manifest by height",
        "detail": "...",
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others"
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }


/block/{height}/operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all operations of the block by *block height*.

+-----------------------------+--------+
| PATH                        | METHOD |
+=============================+========+
| /block/{height}/operations  | GET    |
+-----------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_embedded": [
            {
                "_hint": "mitum-currency-hal-v0.0.1",
                "hint": "mitum-currency-operation-value-v0.0.1",
                "_embedded": {
                    "_hint": "mitum-currency-operation-value-v0.0.1",
                    "hash": "FXRvh8ovbAJdmwdz66gtgb1EJSAaSZkA5TadV8KD1oGs",
                    "operation": {
                        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                        "hash": "EikTtWw8izGuaAWbu8dP7PRKpc5Ri6qYzPxaxaD7fr2r",
                        "fact": {
                            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                            "hash": "FXRvh8ovbAJdmwdz66gtgb1EJSAaSZkA5TadV8KD1oGs",
                            "token": "MjAyMS0xMi0yN1QwNzo1ODo1My4zMDE3NjcrMDA6MDA=",
                            "sender": "5om5ZuSsqjEj7CxoF1VyLLJYhQoCwBPjUciy9gu8dh8hmca",
                            "items": [
                                {
                                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                                    "keys": {
                                        "_hint": "mitum-currency-keys-v0.0.1",
                                        "hash": "C7ntk12BMkjpBoita2qf6USE45moRmLcrpUXn2FxCB31",
                                        "keys": [
                                            {
                                                "_hint": "mitum-currency-key-v0.0.1",
                                                "weight": 100,
                                                "key": "2BfVL17JezsZjsYx3PzXW9aRzERFA4F2Hnj1bFK7akXhAmpu"
                                            }
                                        ],
                                        "threshold": 100
                                    },
                                    "amounts": [
                                        {
                                            "_hint": "mitum-currency-amount-v0.0.1",
                                            "amount": "100",
                                            "currency": "MCC"
                                        }
                                    ]
                                }
                            ]
                        },
                        "fact_signs": [
                            {
                                "_hint": "base-fact-sign-v0.0.1",
                                "signer": "p4nHuxamW5HQZQd1mpkMqsHCbnnwdjWZ9c21eF2eKdLrmpu",
                                "signature": "AN1rKvtMWpbB3qLou12rkGVXxJxW4kYitEYkagNQJ4QWCYgNYSrLvsxDkLMxRfW2Do9KhkvzPVrr3r48YPN775yiJiMiyGx5m",
                                "signed_at": "2021-12-27T07:58:53.323Z"
                            }
                        ],
                        "memo": ""
                    },
                    "height": 48480,
                    "confirmed_at": "2021-12-27T08:01:54.507Z",
                    "reason": {
                        "_hint": "base-operation-reason-v0.0.1",
                        "msg": "; state, \"5om5ZuSsqjEj7CxoF1VyLLJYhQoCwBPjUciy9gu8dh8hmca:account\" does not exist",
                        "data": null
                    },
                    "in_state": false,
                    "index": 0
                },
                "_links": {
                    "block": {
                        "href": "/block/48480"
                    },
                    "manifest": {
                        "href": "/block/48480/manifest"
                    },
                    "self": {
                        "href": "/block/operation/FXRvh8ovbAJdmwdz66gtgb1EJSAaSZkA5TadV8KD1oGs"
                    }
                }
            }
        ],
        "_links": {
            "reverse": {
                "href": "/block/48480/operations?reverse=1"
            },
            "next": {
                "href": "/block/48480/operations?offset=0"
            },
            "self": {
                "href": "/block/48480/operations"
            }
        }
    }

* 404 (operations not found)

| If there are no more operations or there aren't any operations, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "operations not found",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/{block_hash}
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the block information of the block by *block hash*.

+-----------------------------+--------+
| PATH                        | METHOD |
+=============================+========+
| /block/{block_hash}         | GET    |
+-----------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_links": {
            "manifest": {
                "href": "/block/7tAfifVzxSz3kKzGq9RceKtuVAeFB7E9jvCUnojV3YfM/manifest"
            },
            "manifest:{height}": {
                "href": "/block/{height:[0-9]+}/manifest",
                "templated": true
            },
            "manifest:{hash}": {
                "templated": true,
                "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "block:{hash}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "self": {
                "href": "/block/7tAfifVzxSz3kKzGq9RceKtuVAeFB7E9jvCUnojV3YfM"
            }
        }
    }

* 400 (block not found)

| If the block hash is wrong, it returns ``400``.

.. code-block:: json

    {
        "detail": "..."
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "bad request; invalid hash for block by hash: invalid; empty hash"
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/{block_hash}/manifest
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the block manifest of the block by *block hash*.

+-------------------------------+--------+
| PATH                          | METHOD |
+===============================+========+
| /block/{block_hash}/manifest  | GET    |
+-------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "block-manifest-v0.0.1",
        "_embedded": {
            "_hint": "block-manifest-v0.0.1",
            "hash": "7tAfifVzxSz3kKzGq9RceKtuVAeFB7E9jvCUnojV3YfM",
            "height": 1594489,
            "round": 0,
            "proposal": "3uSAcktWpnrB31RBKS35WABEMZDvDTEtvkMCDVLYfjR8",
            "previous_block": "3FiMUXRZTkcPQCcLwN2fhEP8C8xr9QrB4cK4yTentG59",
            "block_operations": "EF2cQGmrzW4AfUeZyEym7UCSbrMkXCcjhUadk2oM5ME2",
            "block_states": "YMyRynoNP11HfX9aMBjbw8bPWR1MVcaczqeLWV4wve8",
            "confirmed_at": "2022-02-02T12:53:44.669Z",
            "created_at": "2022-02-02T12:53:44.684Z"
        },
        "_links": {
            "block": {
                "href": "/block/1594489"
            },
            "manifest:{hash}": {
                "templated": true,
                "href": "/block/{hash:(?i)[0-9a-z][0-9a-z]+}/manifest"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "block:{hash}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "manifest:{height}": {
                "href": "/block/{height:[0-9]+}/manifest",
                "templated": true
            },
            "self": {
                "href": "/block/1594489/manifest"
            },
            "alternate": {
                "href": "/block/7tAfifVzxSz3kKzGq9RceKtuVAeFB7E9jvCUnojV3YfM/manifest"
            },
            "next": {
                "href": "/block/1594490/manifest"
            }
        }
    }

* 404 (manifest not found)

| If the block hash is wrong, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "not found; manifest not found",
        "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all operations of the network.

+-----------------------------+--------+
| PATH                        | METHOD |
+=============================+========+
| /block/operations           | GET    |
+-----------------------------+--------+

+---------+---------------------------------------+-----------------------+
| Query   |                                       | Example               |
+=========+=======================================+=======================+
| offset  | manifests after offset - block height | 2                     |
+---------+---------------------------------------+-----------------------+
| reverse | manifests by reverse order            | 1 (true)              |
+---------+---------------------------------------+-----------------------+

* offset: integer; block height
* reverse: boolean; use ``1`` for ``true``

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_embedded": [
            {
                "_hint": "mitum-currency-hal-v0.0.1",
                "hint": "mitum-currency-operation-value-v0.0.1",
                "_embedded": {
                    "_hint": "mitum-currency-operation-value-v0.0.1",
                    "hash": "7rSkwgF6BmLmid13jiBJKaaRtgYXS7rtDBFSuNdUNPeo",
                    "operation": {
                        "_hint": "mitum-currency-genesis-currencies-operation-v0.0.1",
                        "hash": "2rtWNNHP15pBcdmmzCjsg45D5KPsqs49YPMRC8AtTJbo",
                        "fact": {
                            "_hint": "mitum-currency-genesis-currencies-operation-fact-v0.0.1",
                            "hash": "7rSkwgF6BmLmid13jiBJKaaRtgYXS7rtDBFSuNdUNPeo",
                            "token": "bWl0dW0=",
                            "genesis_node_key": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "currencies": [
                                {
                                    "_hint": "mitum-currency-currency-design-v0.0.1",
                                    "amount": {
                                        "_hint": "mitum-currency-amount-v0.0.1",
                                        "amount": "1000000000000000000000000000",
                                        "currency": "PEN"
                                    },
                                    "genesis_account": null,
                                    "policy": {
                                        "_hint": "mitum-currency-currency-policy-v0.0.1",
                                        "new_account_min_balance": "10",
                                        "feeer": {
                                            "_hint": "mitum-currency-fixed-feeer-v0.0.1",
                                            "receiver": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                                            "amount": "1"
                                        }
                                    },
                                    "aggregate": "1000000000000000000000000000"
                                },
                                ...
                            ]
                        },
                        "fact_signs": [
                            {
                                "_hint": "base-fact-sign-v0.0.1",
                                "signer": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu",
                                "signature": "AN1rKvt3e9wPJjbGEvucwxr7ntUX4oNBvsGmU4QQBFdAv1ToxXdCBqtbpJ7TwuqY1DyTCcS8FBQjJDbYsWpWixTTtXA5y3R5y",
                                "signed_at": "2021-12-26T04:21:22.159Z"
                            }
                        ]
                    },
                    "height": 0,
                    "confirmed_at": "2021-12-26T04:21:23.013Z",
                    "reason": null,
                    "in_state": true,
                    "index": 0
                },
                "_links": {
                    "block": {
                        "href": "/block/0"
                    },
                    "manifest": {
                        "href": "/block/0/manifest"
                    },
                    "self": {
                        "href": "/block/operation/7rSkwgF6BmLmid13jiBJKaaRtgYXS7rtDBFSuNdUNPeo"
                    }
                }
            },
            ...
        ],
        "_links": {
            "reverse": {
                "href": "/block/operations?reverse=1"
            },
            "next": {
                "href": "/block/operations?offset=86472,0"
            },
            "self": {
                "href": "/block/operations"
            }
        }
    }

* 404 (operations not found)

| If there aren't any operations, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "operations not found",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/block/operation/{fact_hash}
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the operation information of the operation by *fact hash*.

+-------------------------------+--------+
| PATH                          | METHOD |
+===============================+========+
| /block/operation/{fact_hash}  | GET    |
+-------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-operation-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-operation-value-v0.0.1",
            "hash": "CtHUdBrLb5cbrkqorSfudS9o4iVMDNafxKiLHZBArHgU",
            "operation": {
                "_hint": "mitum-currency-fee-operation-v0.0.1",
                "hash": "ByDxnBzr116YvesYsFAdn9LR5bw94YWvVEaEFFZukh6H",
                "fact": {
                    "_hint": "mitum-currency-fee-operation-fact-v0.0.1",
                    "hash": "CtHUdBrLb5cbrkqorSfudS9o4iVMDNafxKiLHZBArHgU",
                    "token": "eVQYAAAAAAA=",
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "1",
                            "currency": "PEN"
                        }
                    ]
                }
            },
            "height": 1594489,
            "confirmed_at": "2022-02-02T12:53:44.669Z",
            "reason": null,
            "in_state": true,
            "index": 1
        },
        "_links": {
            "block": {
                "href": "/block/1594489"
            },
            "manifest": {
                "href": "/block/1594489/manifest"
            },
            "operation:{hash}": {
                "templated": true,
                "href": "/block/operation/{hash:(?i)[0-9a-z][0-9a-z]+}"
            },
            "block:{height}": {
                "href": "/block/{height:[0-9]+}",
                "templated": true
            },
            "self": {
                "href": "/block/operation/CtHUdBrLb5cbrkqorSfudS9o4iVMDNafxKiLHZBArHgU"
            }
        }
    }

* 400 (operation not found)

| If the fact hash is wrong, it returns ``400``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "invalid hash for operation by hash: invalid; empty hash",
        "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

---------------------------------------------------
Account
---------------------------------------------------

/account/{address}
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the latest state of the account by *account address*.

+-----------------------------+--------+
| PATH                        | METHOD |
+=============================+========+
| /account/{address}          | GET    |
+-----------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-account-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-account-value-v0.0.1",
            "hash": "YYWJs2ZEmqvuMHkKco9KwJZL9QUuD9j5QZng5KS4mVR",
            "address": "Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca",
            "keys": {
                "_hint": "mitum-currency-keys-v0.0.1",
                "hash": "Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71y",
                "keys": [
                    {
                        "_hint": "mitum-currency-key-v0.0.1",
                        "weight": 50,
                        "key": "kdfdUyAkiG88TVNZ28TV7LoRyLynFzH89btk1ctb9u1Ympu"
                    },
                    {
                        "_hint": "mitum-currency-key-v0.0.1",
                        "weight": 50,
                        "key": "toPtGPdHCsexeVJcJXykBM14gBEJqc487PmgGVjV3w4vmpu"
                    }
                ],
                "threshold": 100
            },
            "balance": [
                {
                    "_hint": "mitum-currency-amount-v0.0.1",
                    "amount": "10",
                    "currency": "CWC"
                }
            ],
            "height": 1198976,
            "previous_height": -2
        },
        "_links": {
            "operations:{offset,reverse}": {
                "templated": true,
                "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations?offset={offset}&reverse=1"
            },
            "block": {
                "href": "/block/1198976"
            },
            "self": {
                "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca"
            },
            "operations": {
                "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations"
            },
            "operations:{offset}": {
                "templated": true,
                "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations?offset={offset}"
            }
        }
    }

* 404 (account not found)

| It the account address is wrong, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "not found; account, ... not found",
        "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/account/{address}/operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all operations related to the account by *account address*.

+-------------------------------+--------+
| PATH                          | METHOD |
+===============================+========+
| /account/{address}/operations | GET    |
+-------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_embedded": [
            {
                "_hint": "mitum-currency-hal-v0.0.1",
                "hint": "mitum-currency-operation-value-v0.0.1",
                "_embedded": {
                    "_hint": "mitum-currency-operation-value-v0.0.1",
                    "hash": "G57ZwvuAxRA778JGTPz16HSHKhAR6Nb7NegRR2VHNwqd",
                    "operation": {
                    "fact": {
                        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                        "hash": "G57ZwvuAxRA778JGTPz16HSHKhAR6Nb7NegRR2VHNwqd",
                        "token": "MjAyMi0wMS0xN1QwNjoxOTo1MS44NTJa",
                        "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                        "items": [
                            {
                                "_hint": "mitum-currency-create-accounts-multiple-amounts-v0.0.1",
                                "keys": {
                                    "_hint": "mitum-currency-keys-v0.0.1",
                                    "hash": "CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkf",
                                    "keys": [
                                        {
                                            "_hint": "mitum-currency-key-v0.0.1",
                                            "weight": 100,
                                            "key": "kdfdUyAkiG88TVNZ28TV7LoRyLynFzH89btk1ctb9u1Ympu"
                                        }
                                    ],
                                    "threshold": 100
                                },
                                "amounts": [
                                    {
                                        "_hint": "mitum-currency-amount-v0.0.1",
                                        "amount": "100000000000000000000000000",
                                        "currency": "CWC"
                                    },
                                    ...
                                ]
                            }
                        ]
                    },
                    "fact_signs": [
                        {
                            "_hint": "base-fact-sign-v0.0.1",
                            "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                            "signature": "AN1rKvtUWK3qTmQKr613vW5eQm6qt3fRx4wZwEMdecudX8aP9w73KcbVBxuDPGHWLr9j8nL1MJfdSiMiYXNoM7qpsj59N2S14",
                            "signed_at": "2022-01-17T06:19:51.886Z"
                        }
                    ],
                    "memo": "",
                    "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                    "hash": "6XAxmTGfm8AxK9ey242FU3M1Y6pkzgtK2LoEYTjHrASh"
                },
                "height": 910536,
                "confirmed_at": "2022-01-17T06:19:53.617Z",
                "reason": null,
                "in_state": true,
                "index": 0
            },
            "_links": {
                "block": {
                    "href": "/block/910536"
                },
                "manifest": {
                    "href": "/block/910536/manifest"
                },
                "new_account:CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkf": {
                    "key": "CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkf",
                    "address": "CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca",
                    "href": "/account/CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca"
                    },
                    "self": {
                        "href": "/block/operation/G57ZwvuAxRA778JGTPz16HSHKhAR6Nb7NegRR2VHNwqd"
                    }
                }
            },
            ...
        ],
        "_links": {
            "next": {
                "href": "/account/CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca/operations?offset=1291226,0"
            },
            "reverse": {
                "href": "/account/CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca/operations?reverse=1"
            },
            "self": {
                "href": "/account/CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca/operations"
            },
            "account": {
                "href": "/account/CCxfWi1oErWX7vbxddAsLx8bXSwR1FUbwEkAJcb8Qmkfmca"
            }
        }
    }

* 404 (operations not found)

| If there are no more operations or there aren't any operations for the account, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "not found; operations not found",
        "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/accounts?publickey={public_key} 
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all accounts which keys contains the *public key* as a key.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /accounts?publickey={public_key} | GET    |
+----------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_embedded": [
            {
                "_hint": "mitum-currency-hal-v0.0.1",
                "hint": "mitum-currency-account-value-v0.0.1",
                "_embedded": {
                    "_hint": "mitum-currency-account-value-v0.0.1",
                    "hash": "YYWJs2ZEmqvuMHkKco9KwJZL9QUuD9j5QZng5KS4mVR",
                    "address": "Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71y",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 50,
                                "key": "kdfdUyAkiG88TVNZ28TV7LoRyLynFzH89btk1ctb9u1Ympu"
                            },
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 50,
                                "key": "toPtGPdHCsexeVJcJXykBM14gBEJqc487PmgGVjV3w4vmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "height": 1198976,
                    "previous_height": -2
                },
                "_links": {
                    "operations:{offset,reverse}": {
                        "templated": true,
                        "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations?offset={offset}&reverse=1"
                    },
                    "block": {
                        "href": "/block/1198976"
                    },
                    "self": {
                        "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca"
                    },
                    "operations": {
                        "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations"
                    },
                    "operations:{offset}": {
                        "href": "/account/Aqv9Gn15zM3j79WgzwVe73RVZ4RbSab7UK9vSpRbF71ymca/operations?offset={offset}",
                        "templated": true
                    }
                }
            },
            ...
        ],
        "_links": {
            "next": {
                "href": "/accounts?publickey=kdfdUyAkiG88TVNZ28TV7LoRyLynFzH89btk1ctb9u1Ympu&offset=1279558,JLni2ExjGn87UNUro8G7aeiM97M9LGFo8sQfdvGgxk1mca"
            },
            "self": {
                "href": "/accounts?publickey=kdfdUyAkiG88TVNZ28TV7LoRyLynFzH89btk1ctb9u1Ympu"
            }
        }
    }   

* 400 (accounts not found)

| If there are no more accounts or there aren't any accounts for the public key, it returns ``400``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "invalue accounts query: failed to decode publickey, \"...\": failed to decode key.BasePublickey: invalid key; pubkey string is empty",
        "detail": ""
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

---------------------------------------------------
Builder
---------------------------------------------------

/builder/operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all available operation types.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /builder/operation               | GET    |
+----------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_links": {
            "operation-fact:{create-accounts}": {
                "templated": true,
                "href": "/builder/operation/fact/template/create-accounts"
            },
            "operation-fact:{key-updater}": {
                "templated": true,
                "href": "/builder/operation/fact/template/key-updater"
            },
            "operation-fact:{transfers}": {
                "templated": true,
                "href": "/builder/operation/fact/template/transfers"
            },
            "operation-fact:{currency-register}": {
                "href": "/builder/operation/fact/template/currency-register",
                "templated": true
            },
            "self": {
                "href": "/builder/operation"
            }
        }
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/builder/operation/fact/template/{fact}
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the fact template for the requested operation type.

+-----------------------------------------+--------+
| PATH                                    | METHOD |
+=========================================+========+
| /builder/operation/fact/template/{fact} | GET    |
+-----------------------------------------+--------+

* Available types for ``{fact}`` can be found by ``/builder/operation``.

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "8iBXCwN3q8ZvJJ49iAJEN5ZNAhYAYxV69jDLTB9NyzQW",
            "token": "cmFpc2VkIGJ5",
            "sender": "mothermca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "DBa8N5of7LZkx8ngH4mVbQmQ2NHDd6gL2mScGfhAEqdd",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "zzeo6WAS4uqwCss4eRibtLnYHqJM21zhzPbKWQVPttxWmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "-333",
                            "currency": "xXx"
                        }
                    ]
                }
            ]
        },
        "_links": {
            "self": {
                "href": "/builder/operation/fact/template/create-accounts"
            }
        },
        "_extra": {
            "default": {
                "token": "cmFpc2VkIGJ5",
                "sender": "mothermca",
                "items.keys.keys.key": "zzeo6WAS4uqwCss4eRibtLnYHqJM21zhzPbKWQVPttxWmpu",
                "items.big": "-333",
                "currency": "xXx"
            }
        }
    }

* 404 (unknown operation)

| If the ``{fact}`` you requests is wrong, it returns ``404``.

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "title": "unknown operation, \"...\"",
        "detail": "..."
    }

* 500 

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/builder/operation/fact 
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the operation message with a fake fact_sign and an operation hash.
* It automatically fills ``hash`` of fact with a correct fact hash.
* Use a valid fact message as a request json.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /builder/operation/fact          | POST   |
+----------------------------------+--------+

| **Request Example**

* A request json must be a fact message.
* It is available not to fill the ``hash`` field.

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "hash": "",
        "token": "MjAyMS0wOC0yN1QwNjo1MDowNi41OTZa",
        "sender": "ETox5FKJFknprZv7iJk5KnKmqR9kz7fWTEWkHCaDkad3mca",
        "items": [
            {
                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                "keys": {
                    "_hint": "mitum-currency-keys-v0.0.1",
                    "hash": "yAbsevAtgHBT6BXoxJmL2nPveqd1B6kKp2dfAxnoVb1",
                    "keys": [
                        {
                            "_hint": "mitum-currency-key-v0.0.1",
                            "key": "bvdEGTsfaG6W3esdY9PjgjrsariGkhU1i3krVWzPaHtYmpu",
                            "weight": 100
                        }
                    ],
                    "threshold": 100
                },
                "amounts": [
                    {
                        "_hint": "mitum-currency-amount-v0.0.1",
                        "amount": "100",
                        "currency": "MCC"
                    }
                ]
            }
        ]
    }

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
            "hash": "DJ5eA3wYsE4TZiBM9NrPNVWM8cCuceoZpCUNrSpMNQLa",
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "2SehrkkFaqPDgjD6VyHtiAgBRS5Mc5BMFvK6auALP3Sa",
                "token": "MjAyMS0wOC0yN1QwNjo1MDowNi41OTZa",
                "sender": "ETox5FKJFknprZv7iJk5KnKmqR9kz7fWTEWkHCaDkad3mca",
                "items": [
                    {
                        "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                        "keys": {
                            "_hint": "mitum-currency-keys-v0.0.1",
                            "hash": "9dGHYkHV61Nob2UivFHSTrZSYNyjzbZyqvwd2XbQ3w2T",
                            "keys": [
                                {
                                    "_hint": "mitum-currency-key-v0.0.1",
                                    "weight": 100,
                                    "key": "bvdEGTsfaG6W3esdY9PjgjrsariGkhU1i3krVWzPaHtYmpu"
                                }
                            ],
                            "threshold": 100
                        },
                        "amounts": [
                            {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "100",
                                "currency": "MCC"
                            }
                        ]
                    }
                ]
            },
            "fact_signs": [
                {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "zzeo6WAS4uqwCss4eRibtLnYHqJM21zhzPbKWQVPttxWmpu",
                    "signature": "22UZo26eN",
                    "signed_at": "2020-10-08T07:53:26Z"
                }
            ],
            "memo": ""
        },
        "_links": {
            "self": {
                "href": "/builder/operation/fact"
            }
        },
        "_extra": {
            "default": {
                "fact_signs.signer": "zzeo6WAS4uqwCss4eRibtLnYHqJM21zhzPbKWQVPttxWmpu",
                "fact_signs.signature": "22UZo26eN"
            },
            "signature_base": "FW3W9vEA0DQwh6QoRxrjCaSPene+l8l1x7v9LUb59tNtaXR1bQ=="
        }
    }

* 400 (problems in request)

| If the fact message you request with is wrong or not available, it returns ``400``.

.. code-block:: json

    {
    "_hint": "mitum-currency-problem-v0.0.1",
    "type": "https://github.com/spikeekips/mitum-currency/problems/others",
    "title": "...",
    "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/builder/operation/sign
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the operation message with new operation hash.
* It automatically fills ``hash`` of the operation with a newly generated operation hash.
* So the request operation message is available even though it doesn't have operation hash.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /builder/operation/sign          | POST   |
+----------------------------------+--------+

| **Request Example**

* A request json must be an operation message.
* It is available not to fill the ``hash`` field. (But fact hash must be correct.)

.. code-block:: json

    {
        "memo": "",
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "8yGWvxxQUGUd2tL2EEJSJyDTguXgrDrwwFVAgqnefWp5",
            "token": "MjAyMi0wMi0wM1QwNjoyMTozMi41Njla",
            "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GyCVt1JHwrjVmJo3Gjf1wpViDC1sCVjfCY8bEV5aHUrq",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "hTTVAEnZwaGzs12XLax2M7nY4MAnwykYLA6QpVVEbuuMmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "1000000000000000000000",
                            "currency": "PEN"
                        }
                    ]
                }
            ]
        },
        "hash": "",
        "fact_signs": [
            {
                "_hint": "base-fact-sign-v0.0.1",
                "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                "signature": "AN1rKvtB4BCAHibpYmUZsiPi2abRDJ91Y5qpYpuZuwS2MH1voVSjxCXHhfuTkqAMJCtgEzGtsFaGkjEt9SQucoCia2KDDqQhm",
                "signed_at": "2022-02-03T06:21:32.575Z"
            }
        ]
    }

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
            "hash": "2UimExSvg5YYywaTqzY69TgAYGFEnKvtU5eHCiptZPLP",
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "8yGWvxxQUGUd2tL2EEJSJyDTguXgrDrwwFVAgqnefWp5",
                "token": "MjAyMi0wMi0wM1QwNjoyMTozMi41Njla",
                "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                "items": [
                    {
                        "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                        "keys": {
                            "_hint": "mitum-currency-keys-v0.0.1",
                            "hash": "GyCVt1JHwrjVmJo3Gjf1wpViDC1sCVjfCY8bEV5aHUrq",
                            "keys": [
                                {
                                    "_hint": "mitum-currency-key-v0.0.1",
                                    "weight": 100,
                                    "key": "hTTVAEnZwaGzs12XLax2M7nY4MAnwykYLA6QpVVEbuuMmpu"
                                }
                            ],
                            "threshold": 100
                        },
                        "amounts": [
                            {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "1000000000000000000000",
                            "currency": "PEN"
                            }
                        ]
                    }
                ]
            },
            "fact_signs": [
                {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                    "signature": "AN1rKvtB4BCAHibpYmUZsiPi2abRDJ91Y5qpYpuZuwS2MH1voVSjxCXHhfuTkqAMJCtgEzGtsFaGkjEt9SQucoCia2KDDqQhm",
                    "signed_at": "2022-02-03T06:21:32.575Z"
                }
            ],
            "memo": ""
        },
        "_links": {
            "self": {
                "href": "/builder/operation/sign"
            }
        }
    }

* 400 (problems in request)

| If there is a problem with the request(for example, invalid operation message), it returns ``400``.

.. code-block:: json

    {
    "_hint": "mitum-currency-problem-v0.0.1",
    "type": "https://github.com/spikeekips/mitum-currency/problems/others",
    "title": "...",
    "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/builder/send 
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It broadcasts a seal or an operation to the network.
* If it successes to broadcast, it will returns ``200`` with the complete seal json.
* However, the success broadcasting doesn't asure the success of processing the operation.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /builder/send                    | POST   |
+----------------------------------+--------+

| **Request Example**

* This API allows to broadcast both operations and seals.
* If you request with an operation, it makes new seal containing the operation then broadcasts it.
* If you request with a seal, it newly signs to the seal and broadcasts it.

* operation

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "hash": "2UimExSvg5YYywaTqzY69TgAYGFEnKvtU5eHCiptZPLP",
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "8yGWvxxQUGUd2tL2EEJSJyDTguXgrDrwwFVAgqnefWp5",
            "token": "MjAyMi0wMi0wM1QwNjoyMTozMi41Njla",
            "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GyCVt1JHwrjVmJo3Gjf1wpViDC1sCVjfCY8bEV5aHUrq",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "hTTVAEnZwaGzs12XLax2M7nY4MAnwykYLA6QpVVEbuuMmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "1000000000000000000000",
                            "currency": "PEN"
                        }
                    ]
                }
            ]
        },
        "fact_signs": [
        {
            "_hint": "base-fact-sign-v0.0.1",
            "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
            "signature": "AN1rKvtB4BCAHibpYmUZsiPi2abRDJ91Y5qpYpuZuwS2MH1voVSjxCXHhfuTkqAMJCtgEzGtsFaGkjEt9SQucoCia2KDDqQhm",
            "signed_at": "2022-02-03T06:21:32.575Z"
        }
        ],
        "memo": ""
    }

* seal

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "6DrH1RbJHBoKBRFUo33m8foBNti7gSjKg31pgs8L1Cdz",
        "body_hash": "CjjV3HTbTonfkGZWMeXq6rWgBcf8sgRj74i6YdTjNabn",
        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
        "signature": "AN1rKvszFHPHZVahb17DCx5dzby8c3UBBeV8R2kzPGMiX8e2dceW8n3LifAaPJAHrTs47hF2xiVeyGcqW99j4rwMR1oH4DNeZ",
        "signed_at": "2022-02-03T06:32:28.022166729Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "GiFDqiwh9j6eqar1yhGGKiT7m8nRaiCW2KqjiAtJQeuu",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "J2Kr6rXvZmj2ooTcmvDCba2y2QCZ8dJikSwGpkH5gJBv",
                    "token": "MjAyMi0wMi0wM1QwNjozMjoyOC4wMjE4MDg2NzNa",
                    "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-multiple-amounts-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "9Myzqxx5mHxy8oZL1uhvBFQaqwk3Egejh5AaBKsARZka",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "fGZAe2skLHoQ4rhPxbPvjNSjfcPY9292NVyJWX5m4cGYmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "10000",
                                    "currency": "PEN"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                    "signature": "AN1rKvtYoYJafJUim5BB5sjid8bxNszGB8kuDgbpARnbeGTgUwp2VpVjXS8kbArUVw4axKNb92ZZ4RXmjZn2enHbEAkb6soGL",
                    "signed_at": "2022-02-03T06:32:28.022141041Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "8xNFCxZ6mwgVLXntD7oXapxDLfVXpPDdcjS8Xb4aFQ6m",
        "body_hash": "A1PWmw93mqYd1VXY2ALQFB6qEB7tKQv8AJp1bkAK75QL",
        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
        "signature": "AN1rKvsxoy63ZDBRqJpz9ps79HHvZMz8jd4yfeTE4v3YFQT5ajoqsZqUF5sTWmACV9R2naBbtXVXamgtw7pPmpSRbkck6NcdF",
        "signed_at": "2022-02-03T06:35:14.742926621Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "GiFDqiwh9j6eqar1yhGGKiT7m8nRaiCW2KqjiAtJQeuu",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "J2Kr6rXvZmj2ooTcmvDCba2y2QCZ8dJikSwGpkH5gJBv",
                    "token": "MjAyMi0wMi0wM1QwNjozMjoyOC4wMjE4MDg2NzNa",
                    "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-multiple-amounts-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "9Myzqxx5mHxy8oZL1uhvBFQaqwk3Egejh5AaBKsARZka",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "fGZAe2skLHoQ4rhPxbPvjNSjfcPY9292NVyJWX5m4cGYmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "10000",
                                    "currency": "PEN"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                    "signature": "AN1rKvtYoYJafJUim5BB5sjid8bxNszGB8kuDgbpARnbeGTgUwp2VpVjXS8kbArUVw4axKNb92ZZ4RXmjZn2enHbEAkb6soGL",
                    "signed_at": "2022-02-03T06:32:28.022141041Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

* 400 (problems in request)

| If there is a problem with your request(for example, wrong operation or seal), it returns ``400``.

.. code-block:: json

    {
    "_hint": "mitum-currency-problem-v0.0.1",
    "type": "https://github.com/spikeekips/mitum-currency/problems/others",
    "title": "...",
    "detail": "..."
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

---------------------------------------------------
Currency
---------------------------------------------------

/currency
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns all currency id's in the network.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /currency                        | GET    |
+----------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "",
        "_links": {
            "currency:{currencyid}": {
                "href": "/currency/{currencyid:.*}",
                "templated": true
            },
            "self": {
                "href": "/currency"
            },
            "currency:PEN": {
                "href": "/currency/PEN"
            }
        }
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }

/currency/{currency_id}
'''''''''''''''''''''''''''''''''''''''''''''''''''

* It returns the currency information of the currency by *currency id*.

+----------------------------------+--------+
| PATH                             | METHOD |
+==================================+========+
| /currency/{currency_id}          | GET    |
+----------------------------------+--------+

| **Response Example**

* 200

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-currency-design-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-currency-design-v0.0.1",
            "amount": {
                "_hint": "mitum-currency-amount-v0.0.1",
                "amount": "1000000000000000000000000000",
                "currency": "PEN"
            },
            "genesis_account": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
            "policy": {
                "_hint": "mitum-currency-currency-policy-v0.0.1",
                "new_account_min_balance": "10",
                "feeer": {
                    "_hint": "mitum-currency-fixed-feeer-v0.0.1",
                    "receiver": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                    "amount": "1"
                }
            },
            "aggregate": "1000000000000000000000000000"
        },
        "_links": {
            "currency:{currencyid}": {
                "templated": true,
                "href": "/currency/{currencyid:.*}"
            },
            "block": {
                "href": "/block/0"
            },500
            "operations": {
                "href": "/block/operation/7rSkwgF6BmLmid13jiBJKaaRtgYXS7rtDBFSuNdUNPeo"
            },
            "self": {
                "href": "/currency/PEN"
            }
        }
    }

* 500

.. code-block:: json

    {
        "_hint": "mitum-currency-problem-v0.0.1",
        "title": "....",
        "type": "https://github.com/spikeekips/mitum-currency/problems/others",
        "detail": "...."
    }
