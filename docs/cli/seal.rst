.. _seal command:

===================================================
Seal Command
===================================================

| The ``seal`` command helps execute various operations contained in the seal.

| The subcommands related to **operation** are as follows.

* ``create-account``
* ``transfer``
* ``key-updater``
* ``currency-register``
* ``currency-policy-updater``
* ``suffrage-inflation``

| The ``seal`` command also helps create a signature and sends a seal.

| The subcommands related to **signature generation** and **transmission** are as follows.

* ``send``
* ``sign``
* ``sign-fact``

| Whether the operation has been successfully processed or not can be checked through the api.

| For more information, please refer to :ref:`confirm success`.

---------------------------------------------------
create-account
---------------------------------------------------

| The ``create-account`` command is used for creating an account.

.. code-block:: shell

    $ ./mc seal create-account --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency,amount> --key=KEY@... --threshold= 

* KEY: <pub key, weight>

| **EXAMPLE**

| We will proceed with the process of creating two accounts, ``ac0`` and ``ac1`` as an example.

| For how to create a keypair, please refer to :ref:`key command`.

| The operation that creates account ``ac0`` is as follows.

1. **Create Single Sig Account**

| We will create the account according to the following account information.

.. code-block:: none

    sender's account - who create new account
    private key: L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr
    address: Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    ac0
    public key - weight: cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu - 100
    threshold: 100
    initial balance: 500 MCC


.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ SENDER_PRV=L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr

    $ SENDER_ADDR=Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    $ AC0_PUB=cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu

    $ ./mc seal create-account --network-id=$NETWORK_ID $SENDER_PRV $SENDER_ADDR MCC,500 --key=$AC0_PUB,100 --threshold=100 | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "Xr7HS7rnbfxTrNbr6qRJ64on6KFuMzvJf5Z6BGqVZsX",
        "body_hash": "EJ93htxhUh2edJhBujMCHhpvGGHQoBic8KQ7VzggxKw1",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZUffVp3gmKD2WJA6756SeDy16d3PF6Ym15HBL89rs1YhT1cW4zVnWD17mhBdhfhutu3848GPd9zTMDqUFmkE8rUWmCs",
        "signed_at": "2021-06-10T14:06:17.60152Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "8ezjZDuC44U2ZFPDkebMyLEYNQBPUUnRjHyfSTeQs9gk",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "F1o51xXWnnQYUVV6JA44beJeKKxuJi3Tv8DzvREodHhA",
                    "token": "MjAyMS0wNi0xMFQxNDowNjoxNy41OTczMDNa",
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
                                    "amount": "500",
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
                        "signature": "381yXYyRo91cqu5gFp5GtHWCiYmsssbFxx95MaL8gH4koBCZ5AfnRqYEpWMxcxgKmeEWsRPVJ8zWytAMLiA9zQes9qGnbcj8",
                        "signed_at": "2021-06-10T14:06:17.601089Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| The above json messages are put in the seal and sent to the node.

2. **Create Multi Sig Account**

.. note::

    * In Mitum Currency, two or more operations signed by one account cannot be processed in one block.
    * For example, two respective operations that send 5 amount from ``ac0`` to ``ac1`` and ``ac2`` cannot be processed at the same time.
    * In this case, only the operation that arrived first is processed and the rest are ignored.

| Suppose that the sender is trying to create ``ac0`` and ``ac1`` at the same time using only one seal. Then the sender should include items for both ``ac0`` and ``ac1``.

| This means that for the operation to be processed successfully, the sender should create and send only one operation that creates two accounts in the seal. **Do not make multiple separate operations with the same sender.**

.. code-block:: none

    sender's account - who create new account
    private key: L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr
    address: Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    ac0
    public key - weight: cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu - 100
    threshold: 100
    initial balance: 50 MCC

    ac1
    public key - weight: sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu - 100
    threshold: 100
    initial balance: 50 MCC

| Then,

.. code-block:: shell

    $ NETWORK_ID=mitum

    $ NODE=https://127.0.0.1:54321

    $ SENDER_PRV=L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr

    $ SENDER_ADDR=Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    $ CURRENCY_ID=MCC

    $ AC0_PUB=cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu

    $ AC1_PUB=sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu

    $ ./mc seal create-account --network-id=$NETWORK_ID \
        $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC0_PUB,100 |
        ./mc seal create-account --network-id=$NETWORK_ID \
            $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC1_PUB,100 --seal=- | \
        ./mc seal send --network-id="$NETWORK_ID" \
            $SENDER_PRV --seal=- --node=$NODE --tls-insecure | jq -R '. as $line | try fromjson catch $line'
    {
        "_hint": "seal-v0.0.1",
        "hash": "HV1tT3D639TiYe6bmamXtesvNjAN8tJ7AmgmeB6STrwz",
        "body_hash": "Gg5KQzzNPAt5PiLrcE5kjMbd4jB7Vk4ooBmN81yWDqYv",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZ1szjaYdxsznCpSvg19yS1tKUw1yPmgXBX6Ehf5ZcKNaMCRkJ8PaNS34rUwLSZ88EPh8vFq1FfRncHiTfo1v9adHCSH",
        "signed_at": "2021-06-10T15:01:13.080144Z",
        "operations": [
            {
                "memo": "",
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
                        "signed_at": "2021-06-10T15:01:13.053303Z"
                    }
                ]
            }
        ]
    }
    "2021-06-10T15:01:13.083634Z INF trying to send seal module=command-send-seal"
    "2021-06-10T15:01:13.171266Z INF sent seal module=command-send-seal"

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

---------------------------------------------------
transfer
---------------------------------------------------

| The ``transfer`` command is used for transferring tokens between accounts.

.. code-block:: shell

    $ ./mc seal transfer --network-id=NETWORK-ID-FLAG <privatekey> <sender> <receiver> <currency,amount> ...

| **EXAMPLE**

| This is an example of transferring the currency 10 *MCC* tokens from ``ac0`` to ``ac1``.

.. code-block:: shell

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ ./mc seal transfer --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $AC1_ADDR $CURRENCY_ID,10 | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "EJDzHbusvvcknN9NWaK1wjuvSTav2TVfnDmtRnqVjEVn",
        "body_hash": "FWLTyQePguo6CFxH8SgEHesoLL8ab3FofEw9nXHDDLMp",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZMbRqwMgfWwJNk4rWNuaJenJMHZU3HBufz7Uo4Yj3zo944oeJeGoKjUDyCJXuL4pZLt49gqW2FHV3YuB5zBR24h96ZH",
        "signed_at": "2021-06-14T03:42:11.969679Z",
        "operations": [
            {
                "_hint": "mitum-currency-transfers-operation-v0.0.1",
                "hash": "F3WZYRgcwwYENiVXx6J6zKPqkiDjSZcuF2vUUPiyR3n9",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "7xzioXfnkKU1qrFvgeWK1KrhR71RMHMSBZdpWRVK3MUD",
                    "token": "MjAyMS0wNi0xNFQwMzo0MjoxMS45NjUyNjNa",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "10",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtRQeMWcFQ9oPLqgakgW33fed4mCcxxfQwi3icWLyn19AKJ3XpYehA8njvAi7qzgGSVpv23JXBDcXbwiZvQkHBj6T8jw",
                        "signed_at": "2021-06-14T03:42:11.96891Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| If you want to send the operation to the network right away,

.. code-block:: shell

    $ ./mc seal transfer --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $AC1_ADDR $CURRENCY_ID,3 | jq \
        ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=-

.. _key updater:

---------------------------------------------------
key-updater
---------------------------------------------------

| The ``key-updater`` command is used for updating the account keys.

| Updating account keys to new public keys does not change the address.

.. code-block:: shell

    $ ./mc seal key-updater --network-id=NETWORK-ID-FLAG <privatekey> <target> <currency> --key=KEY@... --threshold=THRESHOLD

* KEY: <pub key, weight>

For more information about account keys, refer to :ref:`multi sig`.

| **EXAMPLE**

| This is an example of ``key-updater``. The example shows updating keys of ``ac0`` to another one.

.. code-block:: none

    ac0 - target account
    private key: KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr
    public key: 2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu
    address: FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    ac1 - new key
    public key: 247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ NODE=https://127.0.0.1:54321

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_PUB=2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_PUB=247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu

    $ CURRENCY_ID=MCC

    $ ./mc seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR --key $AC1_PUB,100 $CURRENCY_ID | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "GvuGxKCTKWqXzgzxk3iWVGkSPAMn1nBNbAu7qgzHB8y6",
        "body_hash": "8gyB4eE7yQvneA463ZnM8LEWKDCthm8mKEFcfvAmk2pg",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZWCaZy3G5VLse9NCBMmJg8bPWoY4rmyAWMTRVjLKZP9WkexgJfN8EP4G2P64MPchFKtsYZ2QsNyu31rrjKQN4THtEtz",
        "signed_at": "2021-06-14T03:45:21.821896Z",
        "operations": [
            {
                "_hint": "mitum-currency-keyupdater-operation-v0.0.1",
                "hash": "4fFKpjDBmSrka3C3Q62fz5JYGZstZmkQTe27vgyNj4A9",
                "fact": {
                    "_hint": "mitum-currency-keyupdater-operation-fact-v0.0.1",
                    "hash": "5yaMz2aSKS5H1wtd4YVcU4q5awbaxu7bhhswX3ss8XCb",
                    "token": "MjAyMS0wNi0xNFQwMzo0NToyMS44MTczNjNa",
                    "target": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "keys": {
                        "_hint": "mitum-currency-keys-v0.0.1",
                        "hash": "GmUiuEbsoTVLSirRWMZ2WcxT69enhEXNfskAnRJby8he",
                        "keys": [
                            {
                                "_hint": "mitum-currency-key-v0.0.1",
                                "weight": 100,
                                "key": "247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu"
                            }
                        ],
                        "threshold": 100
                    },
                    "currency": "MCC"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtPv6CuiW36Q4g1wtmsGNy2Fc3ierpHgfnjXjdqjDE3wvSH293FVDYy9Yf9VTNadfMGJ38WC39hthZuGkau3vBGq7ijP",
                        "signed_at": "2021-06-14T03:45:21.821399Z"
                    }
                ],
                "memo": ""
            }
        ]
    }    

| If you want to send the operation right away,

.. code-block:: shell

    $ ./mc seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR \
        --key $AC1_PUB,100" $CURRENCY_ID \
        | ./mc seal send --network-id=$NETWORK_ID \
        $AC0_PRV --seal=- --node=$NODE --tls-insecure

| Also, you can check whether the account keys have really changed.

.. code-block:: shell

    $ find blockfs -name "*-states-*" -print | sort -g | xargs -n 1 gzcat |  grep '^{' | jq '. | select(.key == "'$AC0_ACC_KEY'") | [ "height: "+(.height|tostring),   "state_key: " + .key, "key.publickey: " + .value.value.keys.keys[0].key, "key.weight: " + (.value.value.keys.keys[0].weight|tostring), "threshold: " + (.value.value.keys.threshold|tostring)]'
    [
        "height: 3",
        "state_key: GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account",
        "key.publickey: 2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "key.weight: 100",
        "threshold: 100"
    ]
    [
        "height: 104",
        "state_key: GkswusUGC22R5wmrXWB5yqFm8UN22yHLihZMkMb3z623-mca:account",
        "key.publickey: 247KCJyus9NYJii9rkT4R3z6GxengcwYQHwRKA6DySbiUmpu",
        "key.weight: 100",
        "threshold: 100"
    ]

---------------------------------------------------
currency-register
---------------------------------------------------

| The ``currency-register`` command is used for registering a new currency token.

.. code-block:: shell

    $ ./mc seal currency-register --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id> <genesis-amount> <genesis-account>

| When registering a new currency, the items that need to be set are as follows.

* ``genesis account``: account where the issued token will be registered with new currency registration
* ``genesis amount``: amount of newly issued tokens
* ``–policy-new-account-min-balance=<amount>`` must be set.
* ``feeer``: The feeer can be selected from three policies; {nil, fixed, ratio}.

    * ``nil`` is a case where there is no fee payment.
    * ``fixed`` is a case where a fixed amount is paid.
    * ``ratio`` is a case where a payment is made in proportion to the operation amount.

    * If the fee policy is fixed, you must set ``–feeer-fixed-receiver=<fee receiver account address>`` and ``–feeer-fixed-amount=<fee amount>`` accordingly.
    * If the fee policy is ratio, then ``–feeer-ratio-receiver=<fee receiver account address>`` and ``–feeer-ratio-ratio=<fee ratio, multifly by operation amount>``,`` –feeer-ratio-min=<minimum fee>``,`` –feeer-ratio-max=<maximum fee>`` must be set.

| When registering a new currency, **the signature of the suffrage nodes participating in consensus must exceed the consensus threshold (67%) to be executed**.

| **EXAMPLE**

| Suppose that we are going to register a new currency **MCC2** with the following conditions.

.. code-block:: none

    genesis-account : ac1
    genesis-amount : 9999999999999
    currency-id : MCC2
    feeer : fixed
    feeer-fixed-receiver : ac1
    feeer-fixed-amount : 3
    seal sender : ac1
    suffrage node : n0, n1, n2, n3

| Then,

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ AC1_ADDR="HWXPq5mBSneSsQis6BbrNT6nvpkafuBqE6F2vgaTYfAC-a000:0.0.1"

    $ AC1_PRV="792c971c801a8e45745938946a85b1089e61c1cdc310cf61370568bf260a29be-0114:0.0.1"

    $ N0_PRV=<n0 private key>

    $ N1_PRV=<n1 private key>

    $ N2_PRV=<n2 private key>

    $ N3_PRV=<n3 private key>

    $ ./mc seal currency-register --network-id=$NETWORK_ID --feeer=fixed --feeer-fixed-receiver=$AC1_ADDR \
        --feeer-fixed-amount=3 --policy-new-account-min-balance=10 $N0_PRV MCC2 9999999999999 $AC1_ADDR \
        | ./mc seal sign-fact $N1_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id="$NETWORK_ID" --seal=- \
        | ./mc seal send --network-id="$NETWORK_ID" $AC1_PRV --seal=-

| Each currency has a *zero account* for deposit only that is used to **burn tokens**. The *zero account* is deposit only because the public key is not registered.

| The address of the *zero account* has the same format as ``<currency id>-Xmca``. For example, the *zero account* address of PEN currency is ``PEN-Xmca``.

.. code-block:: shell

    $ curl --insecure http://localhost:54320/account/PEN-Xmca | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-account-value-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-account-value-v0.0.1",
            "hash": "EJvkxncxfVQNncdKZtjQTH2XuT5ECRiqSZA7LLE14zqi",
            "address": "PEN-Xmca",
            "keys": {
                "_hint": "mitum-currency-keys-v0.0.1",
                "hash": "",
                "keys": [],
                "threshold": 0
            },
            "balance": [
                {
                    "_hint": "mitum-currency-amount-v0.0.1",
                    "amount": "100000000000000000000000000",
                    "currency": "PEN"
                }
            ],
            "height": 41,
            "previous_height": 0
        },
        "_links": {
            "block": {
                "href": "/block/41"
            },
            "previous_block": {
                "href": "/block/0"
            },
            "self": {
                "href": "/account/PEN-Xmca"
            },
            "operations": {
                "href": "/account/PEN-Xmca/operations"
            },
            "operations:{offset}": {
                "href": "/account/PEN-Xmca/operations?offset={offset}",
                "templated": true
            },
            "operations:{offset,reverse}": {
                "templated": true,
                "href": "/account/PEN-Xmca/operations?offset={offset}&reverse=1"
            }
        }
    }

---------------------------------------------------
currency-policy-updater
---------------------------------------------------

| The ``currency-policy-updater`` command is used for updating the currency-related policy.

.. code-block:: shell

    $ ./mc seal currency-policy-updater --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id>

| First, get the info of the registered currency through API.

| When updating a currency policy, **the signature of the suffrage nodes participating in consensus must exceed the consensus threshold (67%) to be executed**.

.. code-block:: shell

    $ curl --insecure -v https://localhost:54320/currency/MCC2 | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-currency-design-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-currency-design-v0.0.1",
            "amount": {
                "_hint": "mitum-currency-amount-v0.0.1",
                "amount": "9999999999999",
                "currency": "MCC2"
            },
            "genesis_account": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
            "policy": {
                "_hint": "mitum-currency-currency-policy-v0.0.1",
                "new_account_min_balance": "10",
                "feeer": {
                    "_hint": "mitum-currency-fixed-feeer-v0.0.1",
                    "type": "fixed",
                    "receiver": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "amount": "3"
                }
            }
        },
        "_links": {
            "self": {
                "href": "/currency/MCC2"
            },
            "currency:{currencyid}": {
                "templated": true,
                "href": "/currency/{currencyid:.*}"
            },
            "block": {
                "href": "/block/10"
            },
            "operations": {
                "href": "/block/operation/goNANpmA1BcnXA6TVL6AKkoxsmiaT2F5ss5zoSh7Wdt"
            }
        }
    }

| The policy that can be changed through ``currency-policy-updater`` is the **fee-related policy** and the **minimum balance value** when creating a new account.

| **EXAMPLE**

| Suppose that we are going to update policy for *MCC2* according to the following conditions.

.. code-block:: none

    currency-id : MCC2

    Policy to be updated
    - feeer : ratio
    - feeer-ratio-receiver : ac1
    - feeer-ratio-ratio : 0.5
    - feeer-ratio-min : 3
    - feeer-ratio-max : 1000
    - policy-new-account-min-balance : 100
    
    suffrage node : n0, n1, n2, n3

| Then,

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ AC1_ADDR="HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca"

    $ AC0_PRV="KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr"

    $ N0_PRV=<n0 private key>

    $ N1_PRV=<n1 private key>

    $ N2_PRV=<n2 private key>

    $ N3_PRV=<n3 private key>

    $ ./mc seal currency-policy-updater --network-id=$NETWORK_ID --feeer="ratio" --feeer-ratio-receiver=$AC1_ADDR \
        --feeer-ratio-ratio=0.5 --feeer-ratio-min=3 --feeer-ratio-max=1000 --policy-new-account-min-balance=100 $N0_PRV MCC2 \
        | ./mc seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=-

| Check,

.. code-block:: shell

    $ curl --insecure https://localhost:54320/currency/MCC2 | jq
    {
        "_hint": "mitum-currency-hal-v0.0.1",
        "hint": "mitum-currency-currency-design-v0.0.1",
        "_embedded": {
            "_hint": "mitum-currency-currency-design-v0.0.1",
            "amount": {
                "_hint": "mitum-currency-amount-v0.0.1",
                "amount": "9999999999999",
                "currency": "MCC2"
            },
            "genesis_account": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
            "policy": {
                "_hint": "mitum-currency-currency-policy-v0.0.1",
                "new_account_min_balance": "100",
                "feeer": {
                    "_hint": "mitum-currency-ratio-feeer-v0.0.1",
                    "type": "ratio",
                    "receiver": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "ratio": 0.5,
                    "min": "3",
                    "max": "1000"
                }
            }
        },
        "_links": {
            "currency:{currencyid}": {
                "href": "/currency/{currencyid:.*}",
                "templated": true
            },
            "block": {
                "href": "/block/13"
            },
            "operations": {
                "href": "/block/operation/3HxC5VP5Fjzent7uVVLsK44i1tp8ooH4f2Vh4c4uWM4e"
            },
            "self": {
                "href": "/currency/MCC2"
            }
        }
    }

---------------------------------------------------
suffrage-inflation
---------------------------------------------------

| The ``suffrage-inflation`` command is used for inflating the supply of an existing currency token.

.. code-block:: shell

    $ ./mc seal suffrage-inflation --network-id=NETWORK-ID-FLAG <privatekey> <inflation item> ...

* ``inflation item``: <receiver-account>,<currency-id>,<inflation-amount>

| There are two processes of registering a currency in Mitum Currency.

* Through initial genesis currency creation 
* By registering a new currency while the network is alive

| The registered currency has a total supply amount. The Mitum Currency may increase the amount of tokens in addition to the total supply amount.

| When generating new amounts, the items that need to be set are as follows.

* ``receiver-account`` which receives account of additionally generated tokens.

| When inflating a currency, **the signature of the suffrage nodes participating in consensus must exceed the consensus threshold (67%) to be executed**.

| **EXAMPLE**

| We are going to inflate the supply of ``MCC`` according to the following conditions.

.. code-block:: none

    operation-sender-account : ac1
    receiver-account : ac2
    inflation-amount : 9999999999999
    currency-id : MCC
    seal sender : ac1
    suffrage node : n0, n1, n2, n3

| Then,

.. code-block:: shell

    $ NETWORK_ID="mitum"
    
    $ AC1_PRV="L2Q4PqxrhgS39jgGoXsV92LaCHRF2SqTLRwMhCC6Q6in9Vb19aDLmpr"
    
    $ AC2_ADDR="HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca"
    
    $ N0_PRV=<n0 private key>
    
    $ N1_PRV=<n1 private key>
    
    $ N2_PRV=<n2 private key>
    
    $ N3_PRV=<n3 private key>
    
    $ ./mc seal suffrage-inflation --network-id=$NETWORK_ID $N0_PRV MCC 9999999999999 $AC2_ADDR \
        | ./mc seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | ./mc seal send --network-id=$NETWORK_ID $AC1_PRV --seal=-

---------------------------------------------------
send
---------------------------------------------------

| The ``send`` command is used for sending a seal.

.. code-block:: shell

    $ ./mc seal send  <sender privatekey> --network-id=<network id> --seal=<data file path> --node=<node https url>

| Operations created in Mitum Currency are **transmitted in units of seals**.

| Signature is required to transmit the seal. Refer to :ref:`seal` for the part related to the keypair used for signature creation.

| **EXAMPLE**

| ``data.json`` is a seal file written in json.

.. code-block:: shell

    $ NETWORK_ID="mitum" 

    $ NODE="https://127.0.0.1:54321"

    $ AC0_PRV=L1jPsE8Sjo5QerUHJUZNRqdH1ctxTWzc1ue8Zp2mtpieNwtCKsNZmpr

    $ ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --seal=data.json --node=$NODE jq -R '. as $line | try fromjson catch $line'
    {
        "_hint": "seal-v0.0.1",
        "hash": "6nLRWj5hGQ7va9gxpAJCBxNDKvgFnms9jaa913uWgsx1",
        "body_hash": "32ZEf8V9fV41JHVWbbqQdYWtrw5T255XN8fSXhBAhGFD",
        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
        "signature": "381yXZ4LFY5HnK211gpG3W22V52vMLqix4SysXEeMnqcXUk5eEYGM1JfFaX5UE86EF6qog5jUScPqZo6UkiaAFocUhwtSsjx",
        "signed_at": "2021-06-10T09:17:51.236729Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "7YvcA6WAcKEag1Z4Jv1bQ2wYxAZix5sNB6u8MUXDM44D",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3equMRJAVHk8WdVanffzEWkHfwnBDqF2cFwmmcv8MzDW",
                    "token": "MjAyMS0wNi0xMFQwOToxNzo1MS4yMDgwOTVa",
                    "sender": "8iRVFAPiHKaeznfN3CmNjtFtjYSPMPKLuL6qkaJz8RLumca",
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
                                    "amount": "100000",
                                    "currency": "MCC"
                                }
                            ]
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu",
                        "signature": "AN1rKvtPEX4MRu6kWRYDJ6WtsSnwxwJsYXiVi2Qujx8sF6SJzsZZKj7anCd9cmUZ175FSYLkkWkpDRj3fVgZFDxLFSnos3szz",
                        "signed_at": "2021-06-10T09:17:51.211816Z"
                    }
                ],
                "memo": ""
            }
        ]
    }
    2021-06-10T09:17:51.240066Z INF trying to send seal module=command-send-seal
    2021-06-10T09:17:51.345243Z INF sent seal module=command-send-seal

| When sending to a local node for testing, an error may occur related to tls authentication.

| In this case, give the option ``–tls-insecure=true`` when sending seals.

.. code-block:: shell

    $ ./mc seal send --network-id=$NETWORK_ID $AC0_PRV --tls-insecure=true --seal=data.json --node=$NODE

---------------------------------------------------
sign
---------------------------------------------------

| The ``sign`` command is used for creating a signature for a seal.

.. code-block:: shell

    $ ./mc seal sign --network-id=NETWORK-ID-FLAG <privatekey>

| **EXAMPLE**

| Before using the ``sign`` command, prepare a file containing a seal with operations. The file should be saved in json format for signature generation.

| For example,

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "5W39B2mmtc4KK9THiRdoF6F5UMZPSxjzedPePojVhqyV",
        "body_hash": "5yGtCzJiPRRbZkeLawQev4dvdYgYuKHXe6TP6x2VLSt4",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZHsyzbc8qTD7BJgmGoM8ncSrUcyDZiSNanARp9h84tvcj6HkGXzpFyck9arJTCQDmPGzT5UFq1coHv7wijusgynSfgr",
        "signed_at": "2021-06-10T06:50:26.903245Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9mFHaqd66pv7RjoAbKScUucJLKW7KVSkWqN1WXnzMrxQ",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3CpL1MgD1TPejUmVxPKSgiUu6LCR7FhFrDehSjSogavZ",
                    "token": "MjAyMS0wNi0xMFQwNjo1MDoyNi44NzQyNzVa",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "Dut3WiprEo1BRcx2xRvh6qbBgxaTLXQDris7SihDTET8",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "27tMvbSpajF1VSnrn3xRQESpPAsmA7KZEfUz9ZuTZEemumpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                    "_hint": "mitum-currency-amount-v0.0.1",
                                    "amount": "100000",
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
                        "signature": "AN1rKvtfRrgY15owfURsNyfWnYtZ7syuRafWa637tkWB1HyxDCD2tWZUhySTg6mnZWQKpP3i6Dmf96fw9TUWb8rrbsetHJciH",
                        "signed_at": "2021-06-10T06:50:26.877954Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

| Run ``seal sign`` with this json file.

| Then, you can get a seal with a new seal signature, as shown in the following.

.. code-block:: shell

    $ SIGNER_PRV=KxmWM4Zj5Ln8bbDwVZEKrYQY8N51Uk3UVq5GNQAeb2KW8JqHmsgmmpr
    $ ./mc seal sign --seal=data.json  --network-id=mitum $SIGNER_PRV | jq
    {
        "_hint": "seal-v0.0.1",
        "hash": "5dLCySkPrFtc8SnbjzELBK5GR7VQocrK7cXswEnhEa1S",
        "body_hash": "3Ah7J2q4HhFXSgV3c4EQWeZtpi1nFY7be2nmL4X6qDxa",
        "signer": "224ekkhrax6EpekzfLTv9See1hNDZW3LAjWBRuzTMpgnrmpu",
        "signature": "AN1rKvtFhZfDzyLLXtK3PtZ8P1jSTqZy6gC8WooBjWRhzwLrXjCcVTeo4juzdMg83he2emJ3SVkCNZssiB1pTtAPtx753P5CT",
        "signed_at": "2021-06-10T07:12:41.992205Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-accounts-operation-v0.0.1",
                "hash": "9mFHaqd66pv7RjoAbKScUucJLKW7KVSkWqN1WXnzMrxQ",
                "fact": {
                    "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
                    "hash": "3CpL1MgD1TPejUmVxPKSgiUu6LCR7FhFrDehSjSogavZ",
                    "token": "MjAyMS0wNi0xMFQwNjo1MDoyNi44NzQyNzVa",
                    "sender": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                            "keys": {
                                "_hint": "mitum-currency-keys-v0.0.1",
                                "hash": "Dut3WiprEo1BRcx2xRvh6qbBgxaTLXQDris7SihDTET8",
                                "keys": [
                                    {
                                        "_hint": "mitum-currency-key-v0.0.1",
                                        "weight": 100,
                                        "key": "27tMvbSpajF1VSnrn3xRQESpPAsmA7KZEfUz9ZuTZEemumpu"
                                    }
                                ],
                                "threshold": 100
                            },
                            "amounts": [
                                {
                                "_hint": "mitum-currency-amount-v0.0.1",
                                "amount": "100000",
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
                        "signature": "AN1rKvtfRrgY15owfURsNyfWnYtZ7syuRafWa637tkWB1HyxDCD2tWZUhySTg6mnZWQKpP3i6Dmf96fw9TUWb8rrbsetHJciH",
                        "signed_at": "2021-06-10T06:50:26.877954Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

---------------------------------------------------
sign-fact
---------------------------------------------------

| The ``sign-fact`` command is used for creating signatures for operation facts.

| This command is used to add a fact signature to the operation contained in the seal. You must pass the seal data containing the operation to this command.

| This command is mainly used when an operation is created by an account with multi sig or when signing multiple nodes is required, such as in currency registration.

.. code-block:: shell

    $ ./mc seal sign-fact --network-id=NETWORK-ID-FLAG <privatekey>

| **EXAMPLE**

| Here is an example where a seal contains a transfer operation for transferring tokens from the multi sig account. It requires two fact signatures, but only has one.

.. code-block:: json

    {
        "_hint": "seal-v0.0.1",
        "hash": "CgFaHkJEP966xRQjzPtXBUwzqgQYWB53RHwjBqyvmKHs",
        "body_hash": "Akjx1kJZKzyYMo2eVbqcUvtEfivDEGsK4yeUUuNwbGmu",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZ8qZBYQXDBaGr1KyAcsMJyB9HZLo1aQQRsxhx854aMYm5n7nh3NXzsJHpEhiYHgWUYnCtbAZaVsQ8pe6nEnLaHCXizY",
        "signed_at": "2021-06-10T09:54:35.868873Z",
        "operations": [
            {
                "hash": "Eep8SJH7Vkqft3BcvKYd9NY14Zgzmhyp7Uts2GmpaS5N",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "Eu1b4gr528Xy4u2sg97DsEo5uj9BuQEMjHzJxdsLgH48",
                    "token": "MjAyMS0wNi0xMFQwOTo1NDozNS44NjQwOTha",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtZFkx5e4NexvBSjjJkuzUj45UKau8DL2JZx5d1htnbnkmPmHnNbgwqfvUnz8KHpUR72Z9YxD4JVQhdh4JCzGv9zMDDG",
                        "signed_at": "2021-06-10T09:54:35.868223Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-currency-transfers-operation-v0.0.1"
            }
        ]
    }

| After using ``sign-fact`` to add a fact signature, the above json becomes,

.. code-block:: shell

    $ SIGNER1_PUB_KEY=2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu
    $ SIGNER2_PUB_KEY=sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu
    $ SIGNER2_PRV_KEY=L5AAoEqwnHCp7WfkPcUmtUX61ppZQww345rEDCwB33jVPud4hzKJmpr
    $ NETWORK_ID=mitum
    $ ./mc seal sign-fact $SIGNER2_PRV_KEY --seal data.json --network-id=$NETWORK_ID | jq

    {
        "_hint": "seal-v0.0.1",
        "hash": "GiADUurx7qVwyeu8XUNQgmNpqmtN9UDzockhLNKXzYN6",
        "body_hash": "Ci7yzpahGtXqpWs3EGfoqnmUhTgbRhdkgb2GupsJRvgB",
        "signer": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu",
        "signature": "381yXYnDDMYrZ4asLpAYgD7AHDAGMsVih11S3V2jCwNdvJJxeA96whPnth4DxXoJ3RiK8vBpvVKRvXJsPpDpZZ2GMagAmaBi",
        "signed_at": "2021-06-10T10:01:27.690429Z",
        "operations": [
            {
                "_hint": "mitum-currency-transfers-operation-v0.0.1",
                "hash": "AduowWC9mHTCeRp8aqN4dQxHjKGH8xdm8vqxcMj7SfUZ",
                "fact": {
                    "_hint": "mitum-currency-transfers-operation-fact-v0.0.1",
                    "hash": "Eu1b4gr528Xy4u2sg97DsEo5uj9BuQEMjHzJxdsLgH48",
                    "token": "MjAyMS0wNi0xMFQwOTo1NDozNS44NjQwOTha",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-transfers-item-single-amount-v0.0.1",
                            "receiver": "CoXPgSxcad3fRAbp2JBEeGcYGEQ7dQhdZGWXLbTHpwuGmca",
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
                        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
                        "signature": "AN1rKvtZFkx5e4NexvBSjjJkuzUj45UKau8DL2JZx5d1htnbnkmPmHnNbgwqfvUnz8KHpUR72Z9YxD4JVQhdh4JCzGv9zMDDG",
                        "signed_at": "2021-06-10T09:54:35.868223Z"
                    },
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "sdjgo1jJ2kxAxMyBj6qZDb8okZpwzHYE8ZACgePYW4eTmpu",
                        "signature": "381yXZ9yqzCSzUZZUuQvU3ZMHgM9Pa5MQUo2hKGhPFW4ZuMCC3eK2iGYvx3gwQD3LCfELuUXejAQiMmeKaNAEoZVPDf1gpkE",
                        "signed_at": "2021-06-10T10:01:27.690034Z"
                    }
                ],
                "memo": ""
            }
        ]
    }