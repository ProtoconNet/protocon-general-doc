.. _Feefi CLIs:

===================================================
Feefi CLIs
===================================================

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

.. _pool-register:

---------------------------------------------------
pool-register
---------------------------------------------------

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

---------------------------------------------------
pool-policy-updater
---------------------------------------------------

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

---------------------------------------------------
deposit-pool
---------------------------------------------------

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

---------------------------------------------------
withdraw-pool
---------------------------------------------------

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

