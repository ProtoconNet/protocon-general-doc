.. _operation generation:

===================================================
Operation Generation
===================================================

| Each model can generate the following operations.

| For generation, you must use :ref:`seal command`.

.. code-block:: shell

    $ ./mitum seal <operation name> ...

.. _Currency CLIs:

---------------------------------------------------
currency
---------------------------------------------------

| This model supports the following operation generation commands:

+------------------------------------+------------------------------------+
| Operations for Currency                                                 | 
+====================================+====================================+
| currency-register                  | Register new currency id           |
+------------------------------------+------------------------------------+
| currency-policy-updater            | Update currency policy             |
+------------------------------------+------------------------------------+
| suffrage-infration                 | Increase amount of tokens          |
+------------------------------------+------------------------------------+

+------------------------------------+------------------------------------+
| Operations for Account                                                  |
+====================================+====================================+
| create-account                     | Create new account                 | 
+------------------------------------+------------------------------------+
| key-updater                        | Update account keys                | 
+------------------------------------+------------------------------------+
| transfer                           | Transfer amount of tokens          | 
+------------------------------------+------------------------------------+

* :ref:`create-account`
* :ref:`key-updater`
* :ref:`transfer`
* :ref:`currency-register`
* :ref:`currency-policy-updater`
* :ref:`suffrage-inflation`

.. _create-account:

create-account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``create-account`` command is used for creating an account.

.. code-block:: shell

    $ ./mitum seal create-account --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency,amount> --key=KEY@... --threshold=THRESHOLD

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

    $ ./mitum seal create-account --network-id=$NETWORK_ID $SENDER_PRV $SENDER_ADDR MCC,500 --key=$AC0_PUB,100 --threshold=100 --pretty
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

1. **Create Multi Sig Account**

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

    $ ./mitum seal create-account --network-id=$NETWORK_ID \
        $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC0_PUB,100 |
        mitum seal create-account --network-id=$NETWORK_ID \
            $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 \
            --key=$AC1_PUB,100 --seal=- | \
        mitum seal send --network-id="$NETWORK_ID" \
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

.. _transfer:

transfer
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``transfer`` command is used for transferring tokens between accounts.

.. code-block:: shell

    $ ./mitum seal transfer --network-id=NETWORK-ID-FLAG <privatekey> <sender> <receiver> <currency,amount> ...

| **EXAMPLE**

| This is an example of transferring the currency 10 *MCC* tokens from ``ac0`` to ``ac1``.

.. code-block:: shell

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ ./mitum seal transfer --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $AC1_ADDR $CURRENCY_ID,10 --pretty
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

.. _key-updater:

key-updater
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``key-updater`` command is used for updating the account keys.

| Updating account keys to new public keys does not change the address.

.. code-block:: shell

    $ ./mitum seal key-updater --network-id=NETWORK-ID-FLAG <privatekey> <target> <currency> --key=KEY@... --threshold=THRESHOLD

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

    $ ./mitum seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR --key $AC1_PUB,100 $CURRENCY_ID --pretty
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

    $ ./mitum seal key-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR \
        --key $AC1_PUB,100" $CURRENCY_ID \
        | mitum seal send --network-id=$NETWORK_ID \
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

.. _currency-register:

currency-register
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``currency-register`` command is used for registering a new currency token.

.. code-block:: shell

    $ ./mitum seal currency-register --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id> <genesis-amount> <genesis-account>

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

    $ ./mitum seal currency-register --network-id=$NETWORK_ID --feeer=fixed --feeer-fixed-receiver=$AC1_ADDR \
        --feeer-fixed-amount=3 --policy-new-account-min-balance=10 $N0_PRV MCC2 9999999999999 $AC1_ADDR \
        | mitum seal sign-fact $N1_PRV --network-id="$NETWORK_ID" --seal=- \
        | mitum seal sign-fact $N2_PRV --network-id="$NETWORK_ID" --seal=- \
        | mitum seal sign-fact $N3_PRV --network-id="$NETWORK_ID" --seal=- \
        | mitum seal send --network-id="$NETWORK_ID" $AC1_PRV --seal=-

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

.. _currency-policy-updater:

currency-policy-updater
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``currency-policy-updater`` command is used for updating the currency-related policy.

.. code-block:: shell

    $ ./mitum seal currency-policy-updater --network-id=NETWORK-ID-FLAG --feeer=STRING <privatekey> <currency-id>

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

    $ ./mitum seal currency-policy-updater --network-id=$NETWORK_ID --feeer="ratio" --feeer-ratio-receiver=$AC1_ADDR \
        --feeer-ratio-ratio=0.5 --feeer-ratio-min=3 --feeer-ratio-max=1000 --policy-new-account-min-balance=100 $N0_PRV MCC2 \
        | mitum seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal send --network-id=$NETWORK_ID $AC0_PRV --seal=-

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

.. _suffrage-inflation:

suffrage-inflation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``suffrage-inflation`` command is used for inflating the supply of an existing currency token.

.. code-block:: shell

    $ ./mitum seal suffrage-inflation --network-id=NETWORK-ID-FLAG <privatekey> <inflation item> ...

* ``inflation item``: <receiver-account>,<currency-id>,<inflation-amount>

| There are two processes of registering a currency in Mitum Currency.

* Through initial genesis currency generation 
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
    
    $ ./mitum seal suffrage-inflation --network-id=$NETWORK_ID $N0_PRV MCC 9999999999999 $AC2_ADDR \
        | mitum seal sign-fact $N1_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal sign-fact $N2_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal sign-fact $N3_PRV --network-id=$NETWORK_ID --seal=- \
        | mitum seal send --network-id=$NETWORK_ID $AC1_PRV --seal=-

.. _Currency Extension CLIs:

---------------------------------------------------
currency-extension
---------------------------------------------------

| This model supports the following operation generation commands:

+-----------------------------------------+-----------------------------------------+
| Operations for Contract Account                                                   |
+=========================================+=========================================+
| create-contract-account                 | Create new contract account             | 
+-----------------------------------------+-----------------------------------------+
| withdraw                                | Withdraw tokens from contract account   | 
+-----------------------------------------+-----------------------------------------+

* :ref:`create-contract-account`
* :ref:`withdraw`

.. _create-contract-account:

create-contract-account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``create-contract-account`` command is used for creating an account.

.. code-block:: shell

    $ ./mitum seal create-contract-account --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency,amount> --key=KEY@... --threshold=THRESHOLD

* KEY: <pub key, weight>

| The contract account address generation method is typically the same as :ref:`create-account`.

| However, the contract account cannot be an operation sender because it does not have public keys after it is created.

**EXAMPLE**

| The following example creates an operation that creates a new contract account.

.. code-block:: shell

    $ NETWORK_ID=mitum

    $ NODE=https://127.0.0.1:54321

    $ SENDER_PRV=L5GTSKkRs9NPsXwYgACZdodNUJqCAWjz2BccuR4cAgxJumEZWjokmpr

    $ SENDER_ADDR=Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca

    $ CURRENCY_ID=MCC

    $ CA_PUB=cnMJqt1Q7LXKqFAWprm6FBC7fRbWQeZhrymTavN11PKJmpu

    $ ./mitum seal create-contract-account --network-id=$NETWORK_ID $SENDER_PRV $SENDER_ADDR $CURRENCY_ID,50 --key=$CA_PUB,100 --threshold=100 --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "FesvoWab1rxiqThwa3NcatCYQjmsAHVdW3jhjgAvNUeH",
        "body_hash": "7VP1MkTMShuMkTFaVZ5NQfSc4znE8fBdBDJqNVpz9AQY",
        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
        "signature": "381yXZ35xwEQHrx29K9gxEByxCEfNjq4kk2RAc9R1pHxFvsb3ipBj6YATbcibNGmt9Qmjfk37Pj1dXEhUpxgpsAiomhiLdev",
        "signed_at": "2022-09-22T05:10:53.613948Z",
        "operations": [
            {
                "_hint": "mitum-currency-create-contract-accounts-operation-v0.0.1",
                "hash": "9CGe19v8J2vgtDzYYwrYDmdvSXuoDitRMW5yCLmt1wHS",
                "fact": {
                    "_hint": "mitum-currency-create-contract-accounts-operation-fact-v0.0.1",
                    "hash": "3TdxxmTqL8azYWT7jXJ964YsSVhd4D3fZbfK1a5Mcait",
                    "token": "MjAyMi0wOS0yMlQwNToxMDo1My42MTM4Wg==",
                    "sender": "Gu5xHjhos5WkjGo9jKmYMY7dwWWzbEGdQCs11QkyAhh8mca",
                    "items": [
                        {
                            "_hint": "mitum-currency-create-contract-accounts-multiple-amounts-v0.0.1",
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
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "rcrd3KA2wWNhKdAP8rHRzfRmgp91oR9mqopckyXRmCvGmpu",
                        "signature": "AN1rKvtLiUW7aMuUjm2VAgfprbHBZebQyhpJYHbSGG3wXVKe3w73LZQ59DE8tRVQkepDqiENZbU8GQyHQ7Jb9U8n7A3v9BZv6",
                        "signed_at": "2022-09-22T05:10:53.613936Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _withdraw:

withdraw
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``withdraw`` command is used to withdraw tokens from the contract account.

.. code-block:: shell

    $ ./mitum seal withdraw --network-id=NETWORK-ID-FLAG <privatekey> <sender> <target> <currency-amount> ...

| **EXAMPLE**

| This is an example of withdrawing the currency 10 *MCC* tokens from ``ca0``.

.. code-block:: shell

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ ./mitum seal withdraw --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CA1_ADDR $CURRENCY_ID,10 --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "3Cqw2bKvqRRscAT6DqACM9B4qtQPKi3nkSWV9emssvLH",
        "body_hash": "8onqhQvFNYTvAu5XeYpSx6GD1o6ybAoUsDR7bBs1M7NH",
        "signer": "2Aopgs1nSzNCWLvQx5fkBJCi2uxjYBfN8TqneqFd9DzGcmpu",
        "signature": "381yXZ4NQCLLjLbkc8oN3ZuDUt5Vix9QToVKRB5dyKsiWMyVZXA2EgvkX6fpsURdfuxLddj8yMD1JQWLLnB8xjjVHxr4FgqD",
        "signed_at": "2022-09-22T05:21:21.784792Z",
        "operations": [
            {
                "hash": "5GUZ7nCx1V1Dc4MW28cX3N59wqjjJ9DFWZ3aPUKHDuSe",
                "fact": {
                    "_hint": "mitum-currency-contract-account-withdraw-operation-fact-v0.0.1",
                    "hash": "J3mNeqrZwSSQZGorvXxDaAC2L88uF3akWDNnvQZzgCNP",
                    "token": "MjAyMi0wOS0yMlQwNToyMToyMS43ODQ1OTha",
                    "sender": "FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca",
                    "items": [
                        {
                            "_hint": "mitum-currency-withdraws-item-multi-amounts-v0.0.1",
                            "target": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
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
                        "signature": "381yXZHAgjXqDFJ38277rQFt8MamuhQCRdbqMuVah1TNYFEVg2cLihXCJBrGeUNzUiPpsGwAeHh2zaJG3mtKdc9VmJVU3dbF",
                        "signed_at": "2022-09-22T05:21:21.78478Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-currency-contract-account-withdraw-operation-v0.0.1"
            }
        ]
    }

.. _Document CLIs:

---------------------------------------------------
document
---------------------------------------------------

| This model supports the following operation generation commands:

+-----------------------------------------+-----------------------------------------+
| Operations for Document                                                           |
+=========================================+=========================================+
| create-document                         | Create new document                     | 
+-----------------------------------------+-----------------------------------------+
| update-document                         | Update the registered document          | 
+-----------------------------------------+-----------------------------------------+
| sign-document                           | Sign the registered document            | 
+-----------------------------------------+-----------------------------------------+

* :ref:`create-document`
* :ref:`update-document`
* :ref:`sign-document`

| In fact, in order to create a document using cli, you must use the appropriate command for each document type, not just ``create-document``.

| The document type is divided into **blockcity** and **blocksign**, and each command is as follows.

| For **blockcity**,

* ``document create-blockcity-user-document``
* ``document create-blockcity-land-document``
* ``document create-blockcity-voting-document``
* ``document create-blockcity-history-document``

* ``document update-blockcity-user-document``
* ``document update-blockcity-land-document``
* ``document update-blockcity-voting-document``
* ``document update-blockcity-history-document``

| For **blocksign**,

* ``document create-blocksign-document``
* ``sign-document``

| Also, there is a document id suffix corresponding to each document type.

| For **blockcity**,

* user doc: ``cui``
* land doc: ``cli``
* voting doc: ``cvi``
* history doc: ``chi``

| For **blocksign**,

* blocksign doc: ``sdi``

.. _create-document:

create-document
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``create-document`` command is used for creating an document.

| Use the appropriate command for each document type.

| The commands for each document type are as follows:

* ``create-blockcity-user-document``
* ``create-blockcity-land-document``
* ``create-blockcity-voting-document``
* ``create-blockcity-history-document``
* ``create-blocksign-document``

.. code-block:: shell

    $ ./mitum seal document <document-type-command> --network-id=NETWORK-ID-FLAG <privatekey> <sender> ...

| **EXAMPLE**

| For example, the process for creating a blocksign document is as follows:

.. code-block:: none

    ac0 - sender account
    private key:KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr
    address:BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca
    sign_code: signcode0

    target document
    title: example_doc
    file hash: 8y8eHdmPsxZZGPFrKaYaHCQnDvcVmCAgB1XsNm7KGSxF
    size: 1245
    document id: exampledocsdi

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ FILE_HASH=8y8eHdmPsxZZGPFrKaYaHCQnDvcVmCAgB1XsNm7KGSxF

    $ SIGN_CODE=signcode0

    $ TITLE=example_doc

    $ SIZE=1245

    $ DOCUMENT_ID=exampledocsdi

    $ ./mitum seal document create-blocksign-document --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $FILE_HASH $SIGN_CODE $DOCUMENT_ID $TITLE $SIZE $CURRENCY_ID --pretty 
    {
        "_hint": "seal-v0.0.1",
        "hash": "GF4e4c8Xxvhb5YFwEzXoZi4nV3XjkyPf4dQpu8VAbeEH",
        "body_hash": "43nopiEfz3Rjad1j9jvAjf36kbqw4Nwj6QKBL5vkymhD",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtaa6uDhZLd6okWV7PcEyDNoeVGDewMxfXSoBPiVj5pjkhT1nr3C5RWtF9B8YpGijSaZgKDR2HvozuLVAQhhn4h6dfmK",
        "signed_at": "2022-09-27T07:50:31.80218Z",
        "operations": [
            {
                "fact": {
                    "_hint": "mitum-create-documents-operation-fact-v0.0.1",
                    "hash": "69n9wHdnhowxPUu3ufZLPfZecnssDeky8wTykWq3M2Xj",
                    "token": "MjAyMi0wOS0yN1QwNzo1MDozMS44MDE5MTha",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-create-documents-item-v0.0.1",
                            "doc": {
                                "_hint": "mitum-blocksign-document-data-v0.0.1",
                                "info": {
                                    "_hint": "mitum-document-info-v0.0.1",
                                    "docid": {
                                        "_hint": "mitum-document-id-v0.0.1",
                                        "id": "exampledocsdi"
                                    },
                                    "doctype": "mitum-blocksign-document-data"
                                },
                                "owner": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                                "filehash": "8y8eHdmPsxZZGPFrKaYaHCQnDvcVmCAgB1XsNm7KGSxF",
                                "creator": {
                                    "_hint": "mitum-blocksign-docsign-v0.0.1",
                                    "address": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                                    "signcode": "signcode0",
                                    "signed": true
                                },
                                "title": "example_doc",
                                "size": "1245",
                                "signers": null
                            },
                            "currency": "MCC"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZVwDoasGFrT2TgcqrZ2JmzW31BZWpeAPaeePHdREhavsbuSoVYHM1va5etWXXeMeBwLp94WJ17iYtM2JjjkUkfnzq8e",
                        "signed_at": "2022-09-27T07:50:31.80216Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-create-documents-operation-v0.0.1",
                "hash": "AhwPxKWk9oRym6YwKQGRRqnxZQpSTY8i2RqZRZgPRTyM"
            }
        ]
    }

.. _update-document:

update-document
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``update-document`` command is used for updating documents.

| Use the appropriate command for each document type.

| The commands for each document type are as follows:

* ``update-blockcity-user-document``
* ``update-blockcity-land-document``
* ``update-blockcity-voting-document``
* ``update-blockcity-history-document``

| At this time, the **blocksign-document** cannot be updated.

.. code-block:: shell

    $ ./mitum seal document <document-type-command> --network-id=NETWORK-ID-FLAG <privatekey> <sender> ...

| **EXAMPLE**

| For example, the process for updating a blockcity-user document is as follows:

.. code-block:: none

    ac0 - sender account
    private key:KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr
    address:BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    target document
    document id: user0cui
    gold/bankgold: 10, 10
    hp/strength/agility/dexterity/charisma/intelligence/vital: 1, 1, 1, 1, 1, 1, 1

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CURRENCY_ID=MCC

    $ NETWORK_ID="mitum"

    $ DOCUMENT_ID=user0cui

    $ ./mitum seal document update-blockcity-user-document --network-id=mitum $AC0_PRV $AC0_ADDR 10 10 1 1 1 1 1 1 1 $DOCUMENT_ID $CURRENCY_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "5sddZRj6t3PZkgzz7LE3DzxtJmJwEp2BWiLiLQiZ9jHt",
        "body_hash": "4RMhiUA7d2izpkiJFp3VWF8bpQnNVwgrgGWYGgaHvHCu",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtnLuJ82DMvBs8D7RQPfLPJDNhHjxdgDozs6B7eWmeQpAm1t4EESx2RZPV9RQ4m7zaPMunG9L3dQWigWCMHquPZuECFC",
        "signed_at": "2022-09-27T08:17:52.012673Z",
        "operations": [
            {
                "memo": "",
                "_hint": "mitum-update-documents-operation-v0.0.1",
                "hash": "6DDHb7aTMbYMr4zmorLcuBaucgppQ5tgw34RqjjWJju8",
                "fact": {
                    "_hint": "mitum-update-documents-operation-fact-v0.0.1",
                    "hash": "Gf1uoLeSCg3n176iPvhqsmXF61PMqar4D7DK3ko2iZjY",
                    "token": "MjAyMi0wOS0yN1QwODoxNzo1Mi4wMTI0MTla",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-update-documents-item-v0.0.1",
                            "doc": {
                                "_hint": "mitum-blockcity-document-user-data-v0.0.1",
                                "info": {
                                    "_hint": "mitum-document-info-v0.0.1",
                                    "docid": {
                                        "_hint": "mitum-blockcity-user-document-id-v0.0.1",
                                        "id": "user0cui"
                                    },
                                    "doctype": "mitum-blockcity-document-user-data"
                                },
                                "owner": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                                "gold": 10,
                                "bankgold": 10,
                                "statistics": {
                                    "_hint": "mitum-blockcity-user-statistics-v0.0.1",
                                    "hp": 1,
                                    "strength": 1,
                                    "agility": 1,
                                    "dexterity": 1,
                                    "charisma": 1,
                                    "intelligence": 1,
                                    "vital": 1
                                }
                            },
                            "currency": "MCC"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZLrGDmhoL5htvF2qwjX4TXssgms5opqmXAgC2BybG47DG5Y2ZW5r57S1WT6qh2dXx6PY6d2DFZxhfnAWCpD1d79Btvz",
                        "signed_at": "2022-09-27T08:17:52.012653Z"
                    }
                ]
            }
        ]
    }

.. _sign-document:

sign-document
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The ``sign-document`` command is used for signing documents.

| At this time, the **blockcity-document** cannot be signed.

.. code-block:: shell

    $ ./mitum seal sign-document --network-id=NETWORK-ID-FLAG <privatekey> <sender> <documentid> <owner> <currency>

| **EXAMPLE**

| For example, the process for signing a blocksign document is as follows:

.. code-block:: none

    ac0 - signer account
    private key:KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr
    address:BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    ac1 - owner account
    address: J1MbU4AaYnkGtvTJ2i8VpoPBY2rqP8GXqetQ41T8ZQKamca

.. code-block:: shell

    $ NETWORK_ID="mitum"

    $ AC0_PRV=KzUYFHNzxvUnZfm1ePJJ4gnLcLtMv1Tvod7Fib2sRuFmGwzm1GVbmpr

    $ AC0_ADDR=FnuHC5HkFMpr4QABukchEeT63612gGKus3cRK3KAqK7Bmca

    $ AC1_ADDR=J1MbU4AaYnkGtvTJ2i8VpoPBY2rqP8GXqetQ41T8ZQKamca

    $ CURRENCY_ID=MCC

    $ DOCUMENT_ID=exampledocsdi

    $ ./mitum seal sign-document --network-id=mitum $AC0_PRV $AC0_ADDR $DOCUMENT_ID $AC1_ADDR $CURRENCY_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "3FuuEGb7C8SmYEQC2Ykv3DmNc91CC1JHacTzt5dv6fCK",
        "body_hash": "DWh3hCPjz3BKxLAAARRvLDKHrFpGsbrhayNyf5pkfoEk",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXZ1bHmxG5xEzaLNtqbTo35zYamL5B3GyhbmKJiShEej4v56dW1D16meJAzSZxqmwoiY8YmHsxj6yYbT9ddsUmJEf5Sa1",
        "signed_at": "2022-09-27T08:32:18.78323Z",
        "operations": [
            {
                "hash": "12nBfHCUVvvsKn7AZjL6DuSub8fzppTWshtcEWhvoBeC",
                "fact": {
                    "_hint": "mitum-blocksign-sign-documents-operation-fact-v0.0.1",
                    "hash": "A7rP6Rxp4LqRpirYP5T6zcGNxePUp7gJ9C37JQzL7tte",
                    "token": "MjAyMi0wOS0yN1QwODozMjoxOC43ODI5ODNa",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-blocksign-sign-item-single-document-v0.0.1",
                            "documentid": "exampledocsdi",
                            "owner": "J1MbU4AaYnkGtvTJ2i8VpoPBY2rqP8GXqetQ41T8ZQKamca",
                            "currency": "MCC"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZAiPWdPHkEK6yHUKoiLCENiZQn7i2uUEFJFc6G2sPJfxrVYw6Tps9sU6TFEKKx948VyrNACtYM8decamVjE4Y6ZuZU8",
                        "signed_at": "2022-09-27T08:32:18.783211Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-blocksign-sign-documents-operation-v0.0.1"
            }
        ]
    }  

.. _Feefi CLIs:

---------------------------------------------------
feefi
---------------------------------------------------

| This model supports the following operation generation commands:

+-----------------------------------------+-----------------------------------------+
| Operations for Feefi Pool                                                         |
+=========================================+=========================================+
| pool-register                           | Register new feefi pool                 | 
+-----------------------------------------+-----------------------------------------+
| pool-policy-updater                     | Update pool policy                      | 
+-----------------------------------------+-----------------------------------------+
| pool-deposit                            | Deposit tokens to pool                  | 
+-----------------------------------------+-----------------------------------------+
| pool-withdraw                           | Withdraw tokens from pool               | 
+-----------------------------------------+-----------------------------------------+

* :ref:`pool-register`
* :ref:`pool-policy-updater`
* :ref:`deposit-pool`
* :ref:`withdraw-pool`

.. _pool-register:

pool-register
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``pool-register`` is a command to register a pool of new token pairs in the contract account.

| In order to execute this command correctly, you must also prepare a contract account along with a general account.

.. code-block:: shell

    $ ./mitum seal pool-register --network-id=NETWORK-ID-FLAG <privatekey> <sender> <pool> <feefipool-income-cid> <feefipool-outlay-cid> <initial-fee> <currency-id>

| **EXAMPLE**

| For example, the process for registering a new pool is as follows:

.. code-block:: none

    ac0: pool owner
    ca1: target contract account
    income cid: PEN
    outlay cid: MCC
    pool fee: 1000

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ INCOME_ID=PEN

    $ OUTLAY_ID=MCC

    $ CURRENCY_ID=PEN

    $ ./mn seal pool-register --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CA1_ADDR $INCOME_ID $OUTLAY_ID 1000 $CURRENCY_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "CNF4tXBZYBN165R4TJBD9fU1eioSM6RkcpP4GXz8yWvg",
        "body_hash": "CmY9uTmSRdbA55vhUeQHfmTB9JoVXqxmDYMTLRJmGx9j",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXZCkYhagFJf8cwNiU1x5C4G9pq6J7WKAaGLamd2ctdrKZ2Rmw76q48wFxRu28dmwtJjcoxdgGcRPzhxrHYrAnJcwnDiU",
        "signed_at": "2022-09-29T03:34:37.900423Z",
        "operations": [
            {
                "fact": {
                    "_hint": "mitum-feefi-pool-register-operation-fact-v0.0.1",
                    "hash": "64rjFMjZLMrUc5xzqUSSjZAA8wtdBLaEHfCLL5DmXZnX",
                    "token": "MjAyMi0wOS0yOVQwMzozNDozNy44OTk4NTha",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "target": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "initialfee": "1000",
                    "incomecid": "PEN",
                    "outlaycid": "MCC",
                    "currency": "PEN"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZAFwTeKWD7USrhfrnEEkULmD2nFRuuGuU663STypsFoKBNPoffk7bExDFCStx7SU9uUgB6iWue8VU7a7XUFdSjWRKKn",
                        "signed_at": "2022-09-29T03:34:37.90014Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-feefi-pool-register-operation-v0.0.1",
                "hash": "EspLXHipsoVpsBg43hGyHjtPHxDxEXUph45ThrpKFcrL"
            }
        ]
    }

.. _pool-policy-updater:

pool-policy-updater
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``pool-policy-updater`` is literally a command to update a pool policy.

.. code-block:: shell

    $ ./mitum seal pool-policy-updater --network-id=NETWORK-ID-FLAG <privatekey> <sender> <pool> <feefipool-income-cid> <feefipool-outlay-cid> <fee> <currency-id>

| **EXAMPLE**

| For example, the process of updating a policy in a pool is as follows:

.. code-block:: none

    ac0: pool owner
    ca1: target contract account (pool)
    income cid: PEN
    outlay cid: MCC
    pool fee: 1000

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ INCOME_ID=PEN

    $ OUTLAY_ID=MCC

    $ CURRENCY_ID=PEN

    $ ./mn seal pool-policy-updater --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CA1_ADDR $INCOME_ID $OUTLAY_ID 100 $CURRENCY_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "2JifrJrATSeZ4DLR93SASMRfYPaBtzRDTKTDnMBo7n2o",
        "body_hash": "GTARF3Aa5N2udRryex6mrNQaFGo8PmTvE9jASZXzKJab",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXYtNVGfFErRKJptsxMyus1XZw7gfp4kbFKdUeruacsdWHmRaFzGVcVNunyNmj3GKsgqccSWvWg9vJWfWGCFcpPJFfKmA",
        "signed_at": "2022-09-29T03:43:37.455156Z",
        "operations": [
            {
                "_hint": "mitum-feefi-pool-policy-updater-operation-v0.0.1",
                "hash": "3HW64V3dkRVUvYHFt9p5aokKb3hThZvmZyDuHvFqCCzC",
                "fact": {
                    "_hint": "mitum-feefi-pool-policy-updater-operation-fact-v0.0.1",
                    "hash": "3rzZZYGHBpFAt4ERPCDbWcZpnLTfDUam9Squ5vwpmwMU",
                    "token": "MjAyMi0wOS0yOVQwMzo0MzozNy40NTQ4MDda",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "target": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "fee": "100",
                    "incomecid": "PEN",
                    "outlaycid": "MCC",
                    "currency": "PEN"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZFzjsGsEWraLdWR3ypikpfBjZnPXwoetcnN1jiuzNCC8RVRbmzATeymQQzfdzg2NUHFV4s9B7MjSKZGH7DU8cZ9Eeaa",
                        "signed_at": "2022-09-29T03:43:37.454903Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _deposit-pool:

deposit-pool
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``deposit-pool`` is a command for depositing tokens into a pool.

.. code-block:: shell

    $ ./mitum seal deposit-pool --network-id=NETWORK-ID-FLAG <privatekey> <sender> <pool-address> <income-cid> <outlay-cid> <currency-amount>

| **EXAMPLE**

| For example, the process of depositing tokens into a pool is as follows:

.. code-block:: none

    ac0: general account
    ca1: target contract account (pool)
    income cid: PEN
    outlay cid: MCC
    deposit amount: 1000

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ INCOME_ID=PEN

    $ OUTLAY_ID=MCC

    $ ./mn seal deposit-pool --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CA1_ADDR $INCOME_ID $OUTLAY_ID 1000 --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "62g4Lm6g5trSKMgX69h6x3uWVrecX5nxuSCDoRrZMDvN",
        "body_hash": "7Nre3WrUrbz34THfeD5sfxXYuNaQt15YEJUswfM2N2Kc",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtcm39tLWjZvdero5eucr2rHN36UCKxuvjcJ2BFBVBEfD2szo8igaCRP5v8hQeM85zLPEtsTzmreVLjSRNRPYr7sBdAL",
        "signed_at": "2022-09-29T05:19:17.776578Z",
        "operations": [
            {
                "_hint": "mitum-feefi-pool-deposits-operation-v0.0.1",
                "hash": "BfnEsBGrCSvy16mPWBmuSHdphUwJJM4RZ22F6TBKQwmy",
                "fact": {
                    "_hint": "mitum-feefi-pool-deposits-operation-fact-v0.0.1",
                    "hash": "99UQkedTVajjdK3nvTaxpSyiWbqBXadzNagoQVVcmUcH",
                    "token": "MjAyMi0wOS0yOVQwNToxOToxNy43NzY0Nlo=",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "pool": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "incomecid": "PEN",
                    "outlaycid": "MCC",
                    "amount": "1000"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZUCwW43whDh8e2t1SEMt2Ug8CjQq2CfgJmuKRoNWZz4M2beUYNkJYR6mdemhjh8M7JNrTTedrWvuZnqkXnaHGxix2nZ",
                        "signed_at": "2022-09-29T05:19:17.776566Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _withdraw-pool:

withdraw-pool
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``withdraw-pool`` is a command to withdraw tokens deposited in the pool.

.. code-block:: shell

    $ ./mitum seal withdraw-pool --network-id=NETWORK-ID-FLAG <privatekey> <sender> <pool> <income-cid> <outlay-cid> <currency-amount> ...

| **EXAMPLE**

| For example, the process of withdrawing a token from a pool is as follows:

.. code-block:: none

    ac0: general account
    ca1: target contract account (pool)
    income cid: PEN
    outlay cid: MCC
    withdraw amount: PEN,1000

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ INCOME_ID=PEN

    $ OUTLAY_ID=MCC

    $ ./mn seal withdraw-pool --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CA1_ADDR $INCOME_ID $OUTLAY_ID $INCOME_ID,1000 --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "CH1UGmJXnFSrAvTb6gwUutXmDVveZanUVfaHawoanNDc",
        "body_hash": "52Hd9Cw6oQRCzuPB84P4BQ99oC8NcKJWrWuWnLrDLWte",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXYfZEv6t8nQUKsA2GEZ6Q23xy7YjHHSf41tv5xN4yuukXnErjrHQHjrniUhKKRmxnLFFfK98yNqgKarLNvHFFvpdhinA",
        "signed_at": "2022-09-29T05:26:19.42738Z",
        "operations": [
            {
                "_hint": "mitum-feefi-pool-withdraw-operation-v0.0.1",
                "hash": "2J6vKTXc4y5hSbw2XQYFLfzRoydRA5VA34DSKTDX9pWH",
                "fact": {
                    "_hint": "mitum-feefi-pool-withdraw-operation-fact-v0.0.1",
                    "hash": "7bmHTxhZieuGFo5LDg7dVjz1bcov5BWoZpvLVtU4ktb2",
                    "token": "MjAyMi0wOS0yOVQwNToyNjoxOS40MjcyNTZa",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "pool": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                    "incomecid": "PEN",
                    "outlaycid": "MCC",
                    "amounts": [
                        {
                            "_hint": "mitum-currency-amount-v0.0.1",
                            "amount": "1000",
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZUfYx8mVMa8HQUqL6GiZn6xszPaxCSVg71vKgEPQvq5ZBH4oevwhtrAxcN2Wb5xYZeYtF8k54wbepTxYMg3YTXHyuHB",
                        "signed_at": "2022-09-29T05:26:19.427363Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _NFT CLIs:

---------------------------------------------------
nft
---------------------------------------------------

| This model supports the following operation generation commands:

+-----------------------------------------+-----------------------------------------+
| Operations for NFT Collection                                                     |
+=========================================+=========================================+
| collection-register                     | Register new nft collection             | 
+-----------------------------------------+-----------------------------------------+
| collection-policy-updater               | Update nft collection                   | 
+-----------------------------------------+-----------------------------------------+

+-----------------------------------------+-----------------------------------------+
| Operations for NFT                                                                |
+=========================================+=========================================+
| mint                                    | Mint new nft                            | 
+-----------------------------------------+-----------------------------------------+
| sign                                    | Sign nft as creator or copyrighter      | 
+-----------------------------------------+-----------------------------------------+
| transfer                                | Transfer nft                            | 
+-----------------------------------------+-----------------------------------------+
| burn                                    | Burn(Deactivate) nft                    | 
+-----------------------------------------+-----------------------------------------+

+-----------------------------------------+-----------------------------------------------+
| Operations for Delegation of Authority                                                  |
+=========================================+===============================================+
| delegate                                | Delegation of authority to nfts of collection | 
+-----------------------------------------+-----------------------------------------------+
| approve                                 | Delegation of authority to any one nft        | 
+-----------------------------------------+-----------------------------------------------+

* :ref:`collection-register`
* :ref:`collection-policy-updater`
* :ref:`mint`
* :ref:`transfer-nft`
* :ref:`burn`
* :ref:`sign-nft`
* :ref:`delegate`
* :ref:`approve`

.. _collection-register:

collection-register
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``collection-register`` is a command for registering a new collection design in a contract account.

| In order to execute this command correctly, you must also prepare a contract account along with a general account.

.. code-block:: shell

    $ ./mitum seal collection-register --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <target> <symbol> <name> <royalty>

| **EXAMPLE**

| For example, the process of registering a collection design is as follows:

.. code-block:: none

    ac0: collection owner
    ca1: target contract account
    collection: Crazy Protocon / CPRT / https://protocon.io/api/collection/CPRT
    collection royalty: 10
    whitelist: [ ac0 ]

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ COLLECTION_SYMBOL="CPRT"

    $ COLLECTION_NAME="Crazy Protocon"

    $ COLLECTION_URI=https://protocon.io/api/collection/CPRT

    $ ./mn seal collection-register --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $CA1_ADDR $COLLECTION_SYMBOL $COLLECTION_NAME 10 --white=$AC0_ADDR --uri=$COLLECTION_URI --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "5CQ1o3w8N8pDcYHzSDSkiZ7UwQohUDB16Vvuos6UqMna",
        "body_hash": "FCa3xzPeDeqnQex2JEFFXsMGx4SGsfCKWTJFEbvkeZev",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtLv7bJDvNoQhWiafxhJf5vqz3fSuYbQ5zvajpVnKfcEcmBW1YpmuqS7JrZUmUaDhy6dH3gMirQVrTwpZxTR8qYiwV25",
        "signed_at": "2022-09-29T05:40:13.457989Z",
        "operations": [
            {
                "_hint": "mitum-nft-collection-register-operation-v0.0.1",
                "hash": "Dd7H7EMeXroow4GqJPUJpgzU8e37c28zBut9RigCrm9c",
                "fact": {
                        "_hint": "mitum-nft-collection-register-operation-fact-v0.0.1",
                        "hash": "FaFoitzYgXGxNcSqMvqnjaqe5csAjTS4STubM9xNZKJk",
                        "token": "MjAyMi0wOS0yOVQwNTo0MDoxMy40NTc4NDFa",
                        "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "form": {
                        "_hint": "mitum-nft-collection-register-form-v0.0.1",
                        "target": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                        "symbol": "CPRT",
                        "name": "Crazy Protocon",
                        "royalty": 10,
                        "uri": "https://protocon.io/api/collection/CPRT",
                        "whites": [
                            "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca"
                        ]
                    },
                    "currency": "PEN"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZ369TJvHz9SqgnPquJEhN6gLv5vLoxXem1hUKYkqJRh6qoKAPRsj1GVQm6YZn3HPegvHdnFqo1D1Qe7sR5eXdTVVqr3",
                        "signed_at": "2022-09-29T05:40:13.457979Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _collection-policy-updater:

collection-policy-updater
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``collection-policy-updater`` is a command to update the policy of the registered collection design.

.. code-block:: shell

    $ ./mitum seal collection-register --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <target> <symbol> <name> <royalty>

| **EXAMPLE**

| For example, the process of registering a collection design is as follows:

.. code-block:: none

    ac0: collection owner
    ca1: target contract account
    collection: Crazy Protocon / CPRT / https://protocon.io/api/collection/CPRT
    collection royalty: 10
    whitelist: [ ac0 ]

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ CA1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ COLLECTION_SYMBOL="CPRT"

    $ COLLECTION_NAME="Crazy Protocon"

    $ COLLECTION_URI=https://protocon.io/api/collection/CPRT

    $ ./mn seal collection-register --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $CA1_ADDR $COLLECTION_SYMBOL $COLLECTION_NAME 10 --white=$AC0_ADDR --uri=$COLLECTION_URI --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "5CQ1o3w8N8pDcYHzSDSkiZ7UwQohUDB16Vvuos6UqMna",
        "body_hash": "FCa3xzPeDeqnQex2JEFFXsMGx4SGsfCKWTJFEbvkeZev",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtLv7bJDvNoQhWiafxhJf5vqz3fSuYbQ5zvajpVnKfcEcmBW1YpmuqS7JrZUmUaDhy6dH3gMirQVrTwpZxTR8qYiwV25",
        "signed_at": "2022-09-29T05:40:13.457989Z",
        "operations": [
            {
                "_hint": "mitum-nft-collection-register-operation-v0.0.1",
                "hash": "Dd7H7EMeXroow4GqJPUJpgzU8e37c28zBut9RigCrm9c",
                "fact": {
                        "_hint": "mitum-nft-collection-register-operation-fact-v0.0.1",
                        "hash": "FaFoitzYgXGxNcSqMvqnjaqe5csAjTS4STubM9xNZKJk",
                        "token": "MjAyMi0wOS0yOVQwNTo0MDoxMy40NTc4NDFa",
                        "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "form": {
                        "_hint": "mitum-nft-collection-register-form-v0.0.1",
                        "target": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                        "symbol": "CPRT",
                        "name": "Crazy Protocon",
                        "royalty": 10,
                        "uri": "https://protocon.io/api/collection/CPRT",
                        "whites": [
                            "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca"
                        ]
                    },
                    "currency": "PEN"
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZ369TJvHz9SqgnPquJEhN6gLv5vLoxXem1hUKYkqJRh6qoKAPRsj1GVQm6YZn3HPegvHdnFqo1D1Qe7sR5eXdTVVqr3",
                        "signed_at": "2022-09-29T05:40:13.457979Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _mint:

mint
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``mint`` is a command to mint nft to collection.

| Only accounts registered in the whitelist of the collection can mint nfts to the collection.

.. code-block:: shell

    $ ./mitum seal mint --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <collection> <hash> <uri>

| **EXAMPLE**

| For example, the process of minting nft is as follows:

.. code-block:: none

    ac0: whitelisted account
    collection symbol: target collection
    nft hash: 4nM1L2Z44YztaL
    nft uri: https://protocon.io/api/nft/CPRT-00001
    creator: ac0
    copyrighter: none

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ COLLECTION_SYMBOL=CPRT

    $ NFT_HASH=4nM1L2Z44YztaL

    $ NFT_URI=https://protocon.io/api/nft/CPRT-00001

    $ ./mn seal mint --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $COLLECTION_SYMBOL $NFT_HASH $NFT_URI --creator=$AC0_ADDR,100 --creator-total=100 --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "6RhSU1dnYuvT3VXbo7ihpeyE9jW89RZhq7WShWxUSH7S",
        "body_hash": "BQEXFyqkXYcd4N4FViHeNTFRRiP3M1nxLykFzBQn9pmx",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXYoAAhGbKADEwBbRx63JuER3Cp6zFXmHeiHiq4bLc9BvuCBBckAekDPQQghQ3TmBEsk2xebwoaSctJTgGK7iTuVnQR66",
        "signed_at": "2022-09-29T06:09:15.659013Z",
        "operations": [
            {
                "_hint": "mitum-nft-mint-operation-v0.0.1",
                "hash": "CMmCnrd8r7hoUgKRbvSNXhuA2nhpc9vvr3BXcQ3pWUm2",
                "fact": {
                    "_hint": "mitum-nft-mint-operation-fact-v0.0.1",
                    "hash": "2TkApoGoQ6ws886g7M92MWrrZcH39xkDBkuRDcT6vLdu",
                    "token": "MjAyMi0wOS0yOVQwNjowOToxNS42NTg4NzVa",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-nft-mint-item-v0.0.1",
                            "collection": "CPRT",
                            "form": {
                                "_hint": "mitum-nft-mint-form-v0.0.1",
                                "hash": "4nM1L2Z44YztaL",
                                "uri": "https://protocon.io/api/nft/CPRT-00001",
                                "creators": {
                                    "_hint": "mitum-nft-signers-v0.0.1",
                                    "total": 100,
                                    "signers": [
                                        {
                                            "_hint": "mitum-nft-signer-v0.0.1",
                                            "account": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                                            "share": 100,
                                            "signed": false
                                        }
                                    ]
                                },
                                "copyrighters": {
                                    "_hint": "mitum-nft-signers-v0.0.1",
                                    "total": 0,
                                    "signers": []
                                }
                            },
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "AN1rKvtjKr13MgqBKC3VTEFL2QhUYU94zCLo3eshV1N9oGH1KxxrsMMGefuKJYZAbcsDBr2kV5HdMVjY1ThXbDsGV6Zwxdbtd",
                        "signed_at": "2022-09-29T06:09:15.659002Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _sign-nfts:

sign-nfts
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``sign-nfts`` is a command to sign as an account related to the minted nft.

| The related account here refers to the creator and copywriter registered together when nft minting.

.. code-block:: shell

    $ ./mitum seal sign-nfts --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <nft>

| **EXAMPLE**

| For example, the process of signing nft is as follows:

.. code-block:: none

    ac0: related account
    target nft: CPRT-00001
    option: creator

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ NFT_ID=CPRT,00001

    $ ./mn seal sign-nfts --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $NFT_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "8PkkoofAguZsRc8pLfj7hX7GCeoQgDLDLdjfk8ZsViPF",
        "body_hash": "Fs8MKQs1gkoNLK8ZFMnAMko6PyMhpQ2Tk4EuS9fuh6Yr",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXYuqprX8pDANqerFniRf3yjhSxbBxxwxiYaYyjJLsD8QA33erAaMVZrRijx4er2deJdtRHguARzdCaoikPkdFSqE8d1w",
        "signed_at": "2022-09-29T06:18:03.485899Z",
        "operations": [
            {
                "_hint": "mitum-nft-sign-operation-v0.0.1",
                "hash": "5mSy9YJnSu6MAu69vBrFoGDveQcN4KqjbGFLa2E2mYSm",
                "fact": {
                    "_hint": "mitum-nft-sign-operation-fact-v0.0.1",
                    "hash": "ENR1r1vgDpRNUj7JioenAUtCbrRBphMDqK9yA7VFAUo4",
                    "token": "MjAyMi0wOS0yOVQwNjoxODowMy40ODU3NzVa",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                        "items": [
                        {
                            "_hint": "mitum-nft-sign-item-v0.0.1",
                            "qualification": "creator",
                            "nft": {
                                "_hint": "mitum-nft-nft-id-v0.0.1",
                                "collection": "CPRT",
                                "idx": 1
                            },
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXYh1acYk32rVqeim4owg7icKb2qy7V9Sq2dv9f5fC7SXdQMJWr9K8vGdk3WuywoQ81PDeCYfFVdvt86W9GdwGwmENZhL",
                        "signed_at": "2022-09-29T06:18:03.485888Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

If you want to sign as a copyrighter, use the option ``--qualification=copyrighter``.

.. _transfer-nfts:

transfer-nfts
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``transfer-nfts`` is a command to transfer nft from the nft owner to another account.

| Agent, approved accounts, as well as nft owner, are eligible to send this operation.

.. code-block:: shell

    $ ./mitum seal transfer-nfts --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <receiver> <nft>

| **EXAMPLE**

| For example, the process of transmitting nft is as follows:

.. code-block:: none

    ac0: nft owner
    ac1: receiver
    target nft: CPRT-00001

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ NFT_ID=CPRT,00001

    $ ./mn seal transfer-nfts --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $AC1_ADDR $NFT_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "7v6FZR3s4mGBHvD4TFV2JufL8vfo7offBsapNNw6FGz1",
        "body_hash": "8Zvs6uBc8zLpgdGayyGgyFkQfD8avqBWoKm1ZsUTiZe1",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXYj9S5Va9K4nd3BcCAug4aBaev1ftzZye5KFf9MGMCJucCUZpSNGMtpP4a9kZcyjatH5GfP7kAZYCt4N9EsexrfAnPzM",
        "signed_at": "2022-09-29T06:25:32.460976Z",
        "operations": [
            {
                "_hint": "mitum-nft-transfer-operation-v0.0.1",
                "hash": "2jyBBECZJG8aEUFusUg1SgW37XnMZUH1PkJM1gEuiEa5",
                "fact": {
                    "_hint": "mitum-nft-transfer-operation-fact-v0.0.1",
                    "hash": "9UWSpSmomhkHzaVGvykTKweGSvMxYGJkSQni2yujfsCp",
                    "token": "MjAyMi0wOS0yOVQwNjoyNTozMi40NjA4NjZa",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-nft-transfer-item-v0.0.1",
                            "receiver": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                            "nft": {
                                "_hint": "mitum-nft-nft-id-v0.0.1",
                                "collection": "CPRT",
                                "idx": 1
                            },
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "AN1rKvtC5RugSHM3YUb4NHkkrnVpAz8Wgv7BurQG2nepYXcmdshyZ89KFHrxC9vppditkhKYMz3jYvuyNZPg1TwtJuSoApLpZ",
                        "signed_at": "2022-09-29T06:25:32.460966Z"
                    }
                ],
                "memo": ""
            }
        ]
    }

.. _burn:

burn
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``burn`` is a command to incinerate nft.

| Agent, approved accounts, as well as nft owner, are eligible to send this operation.

.. code-block:: shell

    $ ./mitum seal burn --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <nft>

| **EXAMPLE**

| For example, the process of incinerating nft is as follows:

.. code-block:: none

    ac0: nft owner
    target nft: CPRT-00001

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ NFT_ID=CPRT,00001

    $ ./mn seal burn --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $NFT_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "2cbjue66H6EuaupEPEccGoJcsTuv3D96zDmFcaXSQZAr",
        "body_hash": "34GWZf6YqivGExAjc2tY4sYvxQXg5JQCnnvNxNdxHt8F",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "381yXZSNiYzfQtswgxP6TJgRK9ZFPLhrFbDSi8nFF2MfFpMtP2EUbycxMFPk3yvkCT7cT9YChK8QmgXu64yxJXdhUcSq4VNg",
        "signed_at": "2022-09-29T06:30:44.431107Z",
        "operations": [
            {
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXYrNL1wZqEcSkwBdxuPz922sdVh9T3gb2DhsDHLMN4MVkE1L9JGN8SXuYYXHGG8Vgm1fHh15X5E5sg1f6cXBuyZ3NvS1",
                        "signed_at": "2022-09-29T06:30:44.431097Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-nft-burn-operation-v0.0.1",
                "hash": "B7y5eABzoqzaRW1D16f4a3b7YLNRCgnmiYYauTeMtDqJ",
                "fact": {
                    "_hint": "mitum-nft-burn-operation-fact-v0.0.1",
                    "hash": "CxRKhLGYoUVJGPA8Bg2G86KVUQXBpm5YYGqKAby1mkGh",
                    "token": "MjAyMi0wOS0yOVQwNjozMDo0NC40MzEwMDda",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-nft-burn-item-v0.0.1",
                            "nft": {
                                "_hint": "mitum-nft-nft-id-v0.0.1",
                                "collection": "CPRT",
                                "idx": 1
                            },
                            "currency": "PEN"
                        }
                    ]
                }
            }
        ]
    }

.. _delegate:

delegate
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``delegate`` is a command that delegates the transfer and incineration rights for each nft of one collection to another account.

| At this time, the authorized account will be **Agent Account**.
| Even though an account does not own any NFTs of the collection, it can register an agent in advance by sending this operation.

.. code-block:: shell

    $ ./mitum seal delegate --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <collection> <agent>

| **EXAMPLE**

| For example, the process of delegating agent rights is as follows:

.. code-block:: none

    ac0: general account
    ac1: general/contract account (agent)
    collection symbol: CPRT

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ COLLECTION_SYMBOL=CPRT

    $ ./mn seal delegate --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $COLLECTION_SYMBOL $AC1_ADDR --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "78bKwrFZiodwFxT29Nk3oUsJbeh38Pk1pQj6qYEvpnGC",
        "body_hash": "468Sb3PGoNKANYUhbUPp5W9xy8LXyQWDqrCxQJeWUsmS",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvsxWoVUzURqLxy47XegwGESDpWW4EZv484ZHz45NuZPNbX479jWsF8sByfEXU4wSAdmF7kqzhHwtEFPX7jfycDQYjnJx",
        "signed_at": "2022-09-29T08:40:15.381075Z",
        "operations": [
            {
                "_hint": "mitum-nft-delegate-operation-v0.0.1",
                "hash": "CXjXqfmbDtDaEvrnRkodBCd33jdWpG4q43SKBy8qm6uD",
                "fact": {
                    "_hint": "mitum-nft-delegate-operation-fact-v0.0.1",
                    "hash": "D2Re8uEsoan37UypSbkCxWk4jT6y2YqRKjCgnzcAVe5k",
                    "token": "MjAyMi0wOS0yOVQwODo0MDoxNS4zODA5Nzha",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-nft-delegate-item-v0.0.1",
                            "collection": "CPRT",
                            "agent": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                            "mode": "allow",
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "381yXZRNLYMpnRziEWDJamTNCiUFQKCBtgwJ5gGubkhHWkGxtBAgzXQAdXvHnKAZoAHb9f6fAbrMuSWj2EZtNZejnXUujSSV",
                        "signed_at": "2022-09-29T08:40:15.381066Z"
                    }
                ],
                "memo": ""
            }
        ]
    }



| If you want to withdraw your delegation, use the option ``--mode=cancel``.


.. _approve:

approve
'''''''''''''''''''''''''''''''''''''''''''''''''''

| ``approve`` is a command that grants the right to transfer and incinerate certain nfts to other accounts.

| At this time, the authorized account will be **Approved Account**.
| Only the owner or agent of the owner of the NFT can send this operation.

.. code-block:: shell

    $ ./mitum seal approve --network-id=NETWORK-ID-FLAG <privatekey> <sender> <currency> <approved> <nft>

| **EXAMPLE**

| For example, the process of delegating agent rights is as follows:

.. code-block:: none

    ac0: general account
    ac1: general/contract account (approved)
    target nft: CPRT-00001

.. code-block:: shell

    $ AC0_PRV=KwejqURNWCqao3MZZcuchZXotsg7LzcvxBYPdL9XA2V9w44Vf4ZDmpr

    $ AC0_ADDR=BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca

    $ AC1_ADDR=HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca

    $ NETWORK_ID=mitum

    $ CURRENCY_ID=PEN

    $ NFT_ID=CPRT,1

    $ ./mn seal approve --network-id=$NETWORK_ID $AC0_PRV $AC0_ADDR $CURRENCY_ID $AC1_ADDR $NFT_ID --pretty
    {
        "_hint": "seal-v0.0.1",
        "hash": "HPyk7y6BNba63nd1uwmg9nmyPZLAjG8NxJwG9jCi9Uu1",
        "body_hash": "EoSmooTisCXHgjSvRuhGw2Y9eXY7SzCpwyyPfWVRmVfq",
        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
        "signature": "AN1rKvtRJTx9GUGUD9BXBBJJfXmD9Z6Lpsjo1sq8D1uRCn4Asc4sEHJ3JPZx39nvBgfHcbymNBgZGbwTez21HHtFb8S7Hd8KN",
        "signed_at": "2022-09-29T08:49:00.776655Z",
        "operations": [
            {
                "hash": "6szCeHzng8KZEApzwemEcGA5jwziW8AEbA7w4eHyis3s",
                "fact": {
                    "_hint": "mitum-nft-approve-operation-fact-v0.0.1",
                    "hash": "HM8WU5MnShZbdXcwozN4Zn87yvkL1bqpE8zkRqSFghp2",
                    "token": "MjAyMi0wOS0yOVQwODo0OTowMC43NzY1Mjla",
                    "sender": "BQafCTAUdwbgzoHfPcZf6gMBBnJ5h1vXB8oJ7aHz9gQcmca",
                    "items": [
                        {
                            "_hint": "mitum-nft-approve-item-v0.0.1",
                            "approved": "HjyXhhuHAZBGaEw2S5cKZhDwqVc1StbkJMtdgGm3F1dnmca",
                            "nft": {
                                "_hint": "mitum-nft-nft-id-v0.0.1",
                                "collection": "CPRT",
                                "idx": 1
                            },
                            "currency": "PEN"
                        }
                    ]
                },
                "fact_signs": [
                    {
                        "_hint": "base-fact-sign-v0.0.1",
                        "signer": "tT9K5Mf22vtaB71VryiZDMj2hhijM7JAhXRHSFg3H2nGmpu",
                        "signature": "AN1rKvsyGJc6hdiZv5JbQyWR7Bf4tDUyCVLWNK2fFbLLYEUUKaxfcv97nNkgdZnypuoRPJsFb46GBqkYUb8QKGxvSJwkbKNAL",
                        "signed_at": "2022-09-29T08:49:00.776643Z"
                    }
                ],
                "memo": "",
                "_hint": "mitum-nft-approve-operation-v0.0.1"
            }
        ]
    }

| To initialize the approved account of the NFT, please re-send the operation by filling ``approved`` with the nft owner's account address.
