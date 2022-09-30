.. _Currency Extension CLIs:

===================================================
Currency Extension CLIs
===================================================

| This model supports the following operation generation commands:

+-----------------------------------------+-----------------------------------------+
| Operations for Contract Account                                                   |
+=========================================+=========================================+
| create-contract-account                 | Create new contract account             | 
+-----------------------------------------+-----------------------------------------+
| withdraw                                | Withdraw tokens from contract account   | 
+-----------------------------------------+-----------------------------------------+

.. _create-contract-account:

---------------------------------------------------
create-contract-account
---------------------------------------------------

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

---------------------------------------------------
withdraw
---------------------------------------------------

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