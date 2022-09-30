.. _Document CLIs:

===================================================
Document CLIs
===================================================

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

---------------------------------------------------
create-document
---------------------------------------------------

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

---------------------------------------------------
update-document
---------------------------------------------------

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

---------------------------------------------------
sign-document
---------------------------------------------------

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