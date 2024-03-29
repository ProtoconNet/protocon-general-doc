===================================================
Using Operation Builder
===================================================

| **Digest API** has **Operation Builder** to help write operation messages. **Operation Builder** makes it possible to generate operation messages through api without using SDK.

---------------------------------------------------
Get Operation Fact Template
---------------------------------------------------

| By requesting an *Operation Fact Template*, you can receive a template for each operation type. You can create a fact message by changing the field value of the template.

+---------------+-----------------------------------------------+
| METHOD        | GET                                           |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/fact/template/{fact_type}  |
+---------------+-----------------------------------------------+

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
            "token": "cmFpc2VkIGJ5",
            "sender": "mothermca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "2TQ8Xn5tdowqkJt8kHWcNj2QKhuNRnnCwiXxFbRbwBWY",
                        "keys": [
                            {
                                _hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
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
                "items.keys.keys.key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
                "items.big": "-333",
                "currency": "xXx",
                "token": "cmFpc2VkIGJ5",
                "sender": "mothermca",
            }
        }
    }

* The ``_embedded`` object among the contents of the template responded represents the *fact*. Edit the contents of the fact json object and use it in *Build Operation Message*.

| **create-accounts FACT EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
        "token": "cmFpc2VkIGJ5",
        "sender": "mothermca",
        "items": [
            {
                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                "keys": {
                    "_hint": "mitum-currency-keys-v0.0.1",
                    "hash": "2TQ8Xn5tdowqkJt8kHWcNj2QKhuNRnnCwiXxFbRbwBWY",
                    "keys": [
                        {
                            _hint": "mitum-currency-key-v0.0.1",
                            "weight": 100,
                            "key": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu"
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
    }

* There is no need to edit the ``hash`` value as the builder automatically completes it.
* ``token`` is a *base64* encoded value.
* Use the ``_hint`` item as it is.

| Check :ref:`key command` for the details of key registration of accounts related to ``keys``.

---------------------------------------------------
Build Operation Message
---------------------------------------------------

| The created fact message is sent to the request body in json format and the completed fact message is received.

| In the case of the example, you will receive a fact message with the ``keys hash``, ``token``, and ``fact hash`` changed.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/fact                       |
+---------------+-----------------------------------------------+

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "hash": "92FXbSdm46iuA7kQuC6ENfi5pd64G1Uiu49A3VmaA8Tu",
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "9ttqrz1bkFNCySVnrhYrxewcVB6mkZWWvBpSPS2fShip",
                "token": "MjAyMS0wNi0xNSAwODo0OTozOS45NDggKzAwMDAgVVRD",
                "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                "items": [
                    {
                        "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                        "keys": {
                            "_hint": "mitum-currency-keys-v0.0.1",
                            "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                            "keys": [
                                {
                                    "_hint": "mitum-currency-key-v0.0.1",
                                    "weight": 100,
                                    "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                                }
                            ],
                            "threshold": 100
                        },
                        "amounts": [
                            {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "333",
                                "currency": "MCC"
                            }
                        ]
                    }
                ]
            },
            "fact_signs": [
                {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu",
                    "signature": "22UZo26eN",
                    "signed_at": "2020-10-08T07:53:26Z"
                }
            ],
            "memo": "",
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1"
        },
        "_links": {
            "self": {
                "href": "/builder/operation/fact"
            }
        },
        "_extra": {
            "default": {
                "fact_signs.signer": "oRHdEPPrgbfNxUp6TWsC35DmWu1zbLCW9rp41Z8npF8Hmpu",
                "fact_signs.signature": "22UZo26eN"
            },
            "signature_base": "hCi8MFOChFusqKx6v0zrsJ8u3tppYUOewadYjwTvDUFtaXR1bQ=="
        }
    }

| Check the ``fact.hash`` value of the response data. ``fact.hash`` value is used as data to complete the value of the fact_sign object.

| In a *fact_sign* in ``fact_signs``,

* The ``signer`` is the *publickey* of the keypair used to create the signature.
* The ``signature`` is generated by the ``signer``.
* ``signed_at`` is the datetime at which the signature was generated.

---------------------------------------------------
Sign Operation Message
---------------------------------------------------

| A *signature* is created using the ``hash`` of the received *fact* then the *fact_sign* for it is added.

| When the generated fact message is sent to the request body in json format, the completed operation message with the *operation hash* added is received.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/operation/sign                       |
+---------------+-----------------------------------------------+

| **REQUEST BODY EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
            "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "333",
                            "currency": "MCC"
                        }
                    ]
                }
            ]
        },
        "fact_signs": [
            {
                "_hint": "base-fact-sign-v0.0.1",
                "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                "signed_at": "2021-06-16T01:56:14.124268Z"
            }
        ],
        "memo": "",
    }

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "_embedded": {
            "fact": {
                "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                "items": [
                    {
                        "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                        "keys": {
                            "_hint": "mitum-currency-keys-v0.0.1",
                            "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                            "keys": [
                                {
                                    "_hint": "mitum-currency-key-v0.0.1",
                                    "weight": 100,
                                    "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                                }
                            ],
                            "threshold": 100
                        },
                        "amounts": [
                            {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "333",
                                "currency": "MCC"
                            }
                        ]
                    }
                ]
            },
            "fact_signs": [
                {
                    "_hint": "base-fact-sign-v0.0.1",
                    "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                    "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                    "signed_at": "2021-06-16T01:56:14.124268Z"
                }
            ],
            "memo": "",
            "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
            "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT"
        },
        "_links": {
            "self": {
                "href": "/builder/operation/sign"
            }
        }
    }

---------------------------------------------------
Broadcast Message to Network
---------------------------------------------------

| By requesting an *Operation* or *Seal* message as the request body, you can broadcast it to the network.

| In this case, the *signer* of the seal becomes the digest node.

* If the request body is an **operation**, a new seal is created and the digest node signs.
* If the request body is a **seal**, the seal is signed by the digest node.

+---------------+-----------------------------------------------+
| METHOD        | POST                                          |
+---------------+-----------------------------------------------+
| PATH          | /builder/send                                 |
+---------------+-----------------------------------------------+

| **REQUEST BODY EXAMPLE**

.. code-block:: json

    {
        "fact": {
            "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
            "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
            "items": [
                {
                    "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "333",
                            "currency": "MCC"
                        }
                    ]
                }
            ]
        },
        "fact_signs": [
            {
                "_hint": "base-fact-sign-v0.0.1",
                "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                "signed_at": "2021-06-16T01:56:14.124268Z"
            }
        ],
        "memo": "",
        "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
        "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT"
    }

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "seal-v0.0.1",
        "_embedded": {
            "_hint": "seal-v0.0.1",
            "hash": "4UvusVw9RYdqxHQz2EzDb6gW6CgoZGPayD1yZBcdSSHW",
            "body_hash": "9AFx2gAqeMveV6ojwUi6HKx19GfbZZggPTGhTS3dDih5",
            "signer": "uGnKHNfh8EtNVXsL4Qu1a655oQuzibK8Tc41TZUHzHqkmpu",
            "signature": "381yXZAzT6LcYUXfTG9Fifc6neDfXDqpjzuGzfqr1LXPMvvtseJKzGSRwdL6jvkHBaVRdGPD4YfrHnp2rbpZEEWRNAePiJBt",
            "signed_at": "2021-06-16T03:06:33.649190888Z",
            "operations": [
                {
                    "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                    "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT",
                    "fact": {
                        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                        "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                        "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                        "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                        "items": [
                            {
                                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                                "keys": {
                                    "_hint": "mitum-currency-keys-v0.0.1",
                                    "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                                    "keys": [
                                        {
                                            "_hint": "mitum-currency-key-v0.0.1",
                                            "weight": 100,
                                            "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                                        }
                                    ],
                                    "threshold": 100
                                },
                                "amounts": [
                                    {
                                        "_hint": "mitum-currency-amount-v0.0.1",
                                        "amount": "333",
                                        "currency": "MCC"
                                    }
                                ]
                            }
                        ]
                    },
                    "fact_signs": [
                        {
                            "_hint": "base-fact-sign-v0.0.1",
                            "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                            "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                            "signed_at": "2021-06-16T01:56:14.124268Z"
                        }
                    ],
                    "memo": ""
                }
            ]
        },
        "_links": {
            "self": {
                "href": ""
            },
            "operation:0": {
                "href": "/block/operation/CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE"
            }
        }
    }

.. _confirm success:

---------------------------------------------------
Confirming the Success of the Operation
---------------------------------------------------

| By querying the operation with the *fact hash* value in the api, you can check whether the operation is successfully processed or not.

+---------------+-----------------------------------------------+
| METHOD        | GET                                           |
+---------------+-----------------------------------------------+
| PATH          | /block/operation/{operation_fact_hash}        |
+---------------+-----------------------------------------------+

* If ``_embedded.in_state`` is true in the response message, it means the operation has successfully been saved in the block.
* If ``_embedded.in_state`` is false, it means the operation hasn't been saved in the block.

* If **the operation fails**, the reason may be as follows.
    
    1. *insufficient balance of sender* when sending money
    2. *incorrect signature*
    3. *create-account amount less than new-account-min-balance*
    4. etc...

| You can check the reason for failure in ``_embedded.reason.msg`` in the response message.

| **RESPONSE EXAMPLE**

.. code-block:: json

    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-operation-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-operation-value-v0.0.1",
            "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
            "operation": {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9pNsg6gkQJoVsB7iqY3udeLVti2Yxgbe4mFkGqzds2AT",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE",
                    "token": "MjAyMS0wMy0yNCAwMjozNzozNC4xNzQgKzAwMDAgVVRD",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "333",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvtVhunuSdS8g3KWQ1PFBEP9bzz4sU4Vb3B4JrYyVUF79XwNUrG6AzoVfq6mHsK8W4S5hu7LKjDARfAQeDWwit1GnKXcN",
                        "signed_at": "2021-06-16T01:56:14.124Z"
                    }
                ],
                "memo": ""
            },
            "height": 108674,
            "confirmed_at": "2021-06-16T02:26:55.75Z",
            "reason": {
                "_hint": "base-operation-reason-v0.0.1",
                "msg": "state, \"9g4BAB8nZdzWmrsAomwdvNJU2hA2psvkfTQ5XdLn4F4r-mca:account\" does not exist",
                "data": null
            },
            "in_state": false,
            "index": 0
        },
        "_links": {
            "manifest": {
                "href": "/block/108674/manifest"
            },
            "operation:{hash}": {
                "templated": true,
                "href": "/block/operation/{hash:(?i)[0-9a-z][0-9a-z]+}"
            },
            "block:{height}": {
                "templated": true,
                "href": "/block/{height:[0-9]+}"
            },
            "self": {
                "href": "/block/operation/CDUkHDJB4aC8552QvVCAPk8ZtohSuow67cPZZxqZG7RE"
            },
            "block": {
                "href": "/block/108674"
            }
        }
    }