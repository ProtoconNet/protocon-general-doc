.. _NFT CLIs:

===================================================
NFT CLIs
===================================================

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

.. _collection-register:

---------------------------------------------------
collection-register
---------------------------------------------------

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

---------------------------------------------------
collection-policy-updater
---------------------------------------------------

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

---------------------------------------------------
mint
---------------------------------------------------

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

---------------------------------------------------
sign-nfts
---------------------------------------------------

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

---------------------------------------------------
transfer-nfts
---------------------------------------------------

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

---------------------------------------------------
burn
---------------------------------------------------

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

---------------------------------------------------
delegate
---------------------------------------------------

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

---------------------------------------------------
approve
---------------------------------------------------

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
