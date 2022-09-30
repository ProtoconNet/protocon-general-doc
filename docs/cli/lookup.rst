===================================================
Lookup Account
===================================================

---------------------------------------------------
Prerequisite
---------------------------------------------------

* curl

  * This is a command line tool for interacting with API.
  * https://curl.se

* jq

  * This is a command line tool for parsing json response.
  * https://stedolan.github.io/jq/

---------------------------------------------------
Genesis Account Lookup
---------------------------------------------------

1. You can look up genesis account from local blockdata.

.. code-block:: shell

    $ AC0_ACC_KEY=GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account

    $ find blockfs -name "*-states-*" -print | xargs -n 1 gzcat | grep '^{' | jq '. | select(.key == "'$AC0_ACC_KEY'") | [ "height: "+(.height|tostring), "state_key: " + .key, "address: " + .value.value.address, .operations, .value.value.keys.keys, .value.value.keys.threshold]'
    [
        "height: 2",
        "state_key: GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account",
        "address: FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
        [
            "9mc8YEFWC27WEF3VVee1wk4ib5kvWBk1iJ41pWf27Mrc"
        ],
        [
            {
                "_hint": "mitum-currency-key-v0.0.1",
                "weight": 100,
                "key": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu"
            }
        ],
        100
    ]

* 99999999999999999977 = 99999999999999999999 - (2 create account: 10 * 2) - (2 fee: 1 * 2)

2. You can also look up genesis account from digest api.

.. code-block:: shell

    $ curl --insecure https://localhost:54320/account/FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca | jq '{_embedded}'
    {
        "_embedded": {
            "_hint": "mitum-currency-account-value-v0.0.1",
            "hash": "AHQ4ohzwm7Y9T3f8vH5LTQ2rXKfVg3eazCdyihqsWv8F",
            "address": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
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
            "balance": [
                {
                    "_hint": "mitum-currency-amount-v0.0.1",
                    "amount": "9999999999999",
                    "currency": "MCC2"
                },
                {
                    "_hint": "mitum-currency-amount-v0.0.1",
                    "amount": "50",
                    "currency": "MCC"
                }
            ],
            "height": 10,
            "previous_height": -2
        }
    }

.. note::

    When you look up state by address from mongodb, remove the part after ``-`` of the address and use it as a key.

    * ``FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca`` → ``GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca``

---------------------------------------------------
Operation Lookup
---------------------------------------------------

| Whether the operation block is saved can be checked through the ``fact.hash`` of operation inquiry in the digest API.

.. code-block:: shell

    $ FACT_HASH=3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE

    $ DIGEST_API="https://127.0.0.1:54320"
    
    $ curl --insecure -v $DIGEST_API/block/operation/$FACT_HASH | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-operation-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-operation-value-v0.0.1",
            "hash": "3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE",
            "operation": {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "AhqQMGZHDCeJDp74aQJ8rEXMC6GgQtpxP3rXnjjP41ui",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE",
                    "token": "MjAyMS0wNi0xMFQxNTowMToxMy4wNDA0OTZa",
                    "sender": "Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
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
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
                                    "currency": "MCC"
                                }
                            ]
                        },
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "50",
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
                        "signature": "AN1rKvthtCymTu7gv2fSrMhGwqVuK3o24FrDe6GGLzRU8N5SWF62nPs3iKcEjuzwHya6P9JmrNLRF95ri8QTE4NBc66TxhCHm",
                        "signed_at": "2021-06-10T15:01:13.053Z"
                    }
                ],
                "memo": ""
            },
            "height": 13,
            "confirmed_at": "2021-06-10T15:01:13.354Z",
            "reason": null,
            "in_state": true,
            "index": 0
        },
        "_links": {
            "block": {
                "href": "/block/13"
            },
            "manifest": {
                "href": "/block/13/manifest"
            },
            "new_account:8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu": {
                "href": "/account/8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                "address": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
                "key": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLu"
            },
            "new_account:EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL": {
                "href": "/account/2S252hnemi1oA3UZqEA7dvMSvbd3RA9ut1mgJNxoGW1Pmca",
                "key": "EuCb6BVafkV1tBLsrMqkxojkanJCM4bvmG6JFUZ4s7XL",
                "address": "2S252hnemi1oA3UZqEA7dvMSvbd3RA9ut1mgJNxoGW1Pmca"
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
                "href": "/block/operation/3fDBD1i6V5VpGxB1di6JGgMPhyWZeWRML8FX4LnYXqJE"
            }
        }
    }
