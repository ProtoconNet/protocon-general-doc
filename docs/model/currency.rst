===================================================
Mitum Currency
===================================================

---------------------------------------------------
What is Mitum Currency
---------------------------------------------------

* **Mitum Currency** is a currency model that operates on the **Mitum** blockchain networks.
* The Mitum model is a solution that can provide various services as an extension layer that extends the Mitum main chain.
* In Mitum Currency, flexible policy settings related to currency issuance and operation are possible.
* Mitum Currency is implemented based on Mitum (blockchain core framework).


.. image:: ../images/model.currency/mitum_blockchain_layer.png
    :height: 570
    :scale: 50 
    :alt: Mitum Blockchain Layer


---------------------------------------------------
Features of Mitum Currency
---------------------------------------------------

* Mitum Currency provides core features to meet the business needs of various token-related fields.
* *Multiple keys* can be registered when creating an account, and related keys can be replaced through the key update operation.
* Mitum Currency can issue new currencies and related policies can be customized.
* Currency-related policy can be updated at any time as needed.
* Mitum Currency has no compensation for block generation and there is also no inflation.
* The node configuration for the Mitum Currency network follows the node operation policy of the Mitum blockchain, and details can be found at :ref:`build network`.

---------------------------------------------------
Digest Service
---------------------------------------------------

* **Digest Service** is an internal service that stores block data stored by Mitum separately to serve as *HTTP-based API*.
* For more information on Digest Service, please refer to :ref:`rest api`.

---------------------------------------------------
Seal and Operation
---------------------------------------------------

Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In the Mitum blockchain network, an operation is **a unit of command that changes data**.

| Mitum Currency has operations of,

* ``create-account``
* ``transfer``
* ``key-updater``
* ``currency-register``
* ``currency-policy-updater``
* ``suffrage-infration``

| Each operation requires a signature made with a private key according to its contents.

| The fact in the operation contains the contents to be executed and the hash value summarizing the body of the fact.

Fact and token
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Every operation contains a *fact*. In other words, **the content of the operation is actually contained in the fact**.

| Facts play an important role in Mitum Currency.

* The fact hash is a value representing the processed operation.
* The fact hash must have a unique value in the blockchain.
* So to check whether the operation is stored in the block, it can be retrieved using the fact hash.

| In fact, the contents of the facts can be duplicated. 

| For example, 

    .. code-block:: none
        
        - The contents that `sender A sends 100 to receiver B` must always have the same fact.
        - Fact hashes created using the same fact content can result in duplicate values.
        - If there are two or more operations that result in duplicate values of the fact hash, only the first operation is processed and the remaining operations are ignored.

| If so, does that mean that operations with the same fact content cannot be duplicated?

| Don't worry, in each fact, we use a value called *token* to make it unique.
| The token is a value added to the essential contents of the operation.

.. code-block:: json
    
    {
        "_hint": "mitum-currency-create-accounts-operation-fact-v0.0.1",
        "hash": "3Zdg5ZVdNFRbwX5WU7Nada3Wnx5VEgkHrDLVLkE8FMs1",
        "token": "cmFpc2VkIGJ5",
        "sender": "8PdeEpvqfyL3uZFHRZG5PS3JngYUzFFUGPvCg29C2dBnmca",
        "items": [
            {
                "_hint": "mitum-currency-create-accounts-single-amount-v0.0.1",
                "keys": {
                    "_hint": "mitum-currency-keys-v0.0.1",
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
    }

| The role of the token resembles that of a memo, but is capable of making a fact unique by **giving different token values** for the same fact content.

| Making the fact that is essential for every operation unique expands usability in several ways.

* The biggest advantage is that you can simply check whether the operation is processed or not as you exactly know the contents of the fact along with the token.
* Anyone can calculate the fact hash if they know: the sender, receiver, currencyID, amount of currency, and a specific token value used.
* Therefore, anyone can inquire whether the corresponding operation has been processed with the fact hash.

| A *fact hash* is like a **public proof** recorded in a blockchain. There can be various applications depending on how a user uses the evidence disclosed in the blockchain.
| For example, even an outsider who does not have a direct account in the blockchain can check the fact hash, the only value indicating whether the operation is processed or not, and make the implementation based on that.

| In addition, facts and tokens can practically be used in models that deal with various data including remittance.

.. _seal:

Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| *Seal* is **a collection of operations** transmitted to the network. In other words, the operation is contained in the seal and transmitted.

* To transmit the seal, a signature made with a private key is required.
* To create signature, you must use the private key created in Mitum's keypair package.
* Seal can contain up to 100 operations.

| The private key used for the signature has nothing to do with the blockchain account. In other words, it doesn't have to be the private key used by the account.

Send
'''''''''''''''''''''''''''''''''''''''''''''''''''

| After creating an operation, the client creates and attaches a signature.

* Create as many operations as necessary within the maximum number able to be included in the seal, and put them in the seal.
* Create and put a signature on the seal.
* Send seal to Mitum node.

Stored in Block
'''''''''''''''''''''''''''''''''''''''''''''''''''

| The operation transmitted to the Blockchain network changes the state of the account if it is normal and is finally saved in the block.
| Whether the operation is confirmed and saved in the block can be checked through :ref:`rest api`.

.. _block data:

---------------------------------------------------
Block Data
---------------------------------------------------

Block data in Mitum Currency Node
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In the **Mitum Currency Node**, block data is stored in two spaces: **Database** and **File System**.

* The **database** stores the information used for consensus, such as,

.. code-block:: none

    blockdata_map
    info
    manifest: block header
    operation: operation fact
    operation
    proposal
    seal
    state: state data by each block
    voteproof

* The **file system** stores all block data, such as,

.. code-block:: none

    manifest
    operations of block
    states of block
    proposal
    suffrage information
    voteproofs(and init and accept ballots)

* Block data stored in the **database** is required to run the mitum currency node and participate in the network normally.
* Block data in the **file system** is not used at runtime, but is used to provide block data to syncing nodes.

| An intact node must support block data for other nodes which want to synchronize block data.

BlockDataMap
'''''''''''''''''''''''''''''''''''''''''''''''''''

| By default, block data is stored in the local file system.

| *blockdatamap* contains the information about where the actual block data is located.

.. code-block:: json

    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
        },
        "writer": "blockdata-writer-v0.0.1"
    }

| In this BlockDataMap example, the data of ``operation_tree`` is located at ``file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz``

BlockDataMap for block data stored in external storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Mitum Currency supports storing block data in external storage rather than the node's local file system.

| After going through a certain process to store block data externally, *blockdatamap* is modified as follows.

.. code-block:: json
    
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
        },
        "writer": "blockdata-writer-v0.0.1"
    }

| As you can see, the ``url`` is replaced with the external storage server.

How to update BlockDataMap for external Storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| For example, suppose that block data with a block height of 10 is moved to an external storage.

| Here, we will do this using the node's *deploy key*.
| This *deploy key* of the node is a key that can be used instead of the private key.

| See ``deploy key`` command in :ref:`deploy command` for how to create a deploy key.

| The process of **moving block data** and **updating blockdatamap** is as follows.

* Get the new *deploy key* of mitum currency node.
* Download the current *blockdatamap* by using the ``storage download map`` command.
* Upload all the block data files of height 10 to external storage(example : AWS S3)
* Update the ``url`` field value of the downloaded BlockDataMap with the new url of external storage.
* Update the node's *blockdatamap* by running the ``storage set-blockdatamaps`` command.
* Check the newly updated *blockdatamap* with ``storage download map`` command

| After updating blockdatamap successfully, the mitum currency node will remove all the files with the height of 10 automatically after 30 minutes.

.. code-block:: shell

    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ NODE=https://127.0.0.1:54321
    
    $ ./mc storage download map 10 --tls-insecure --node=$NODE > mapData
    
    $ cat mapData | jq
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
            "manifest": {
                "type": "manifest",
                "checksum": "6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42",
                "url": "file:///000/000/000/000/000/000/002/2-manifest-6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42.jsonld.gz"
            },
            "suffrage_info": {
                "type": "suffrage_info",
                "checksum": "e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02",
                "url": "file:///000/000/000/000/000/000/002/2-suffrage_info-e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02.jsonld.gz"
            },
            "states": {
                "type": "states",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "file:///000/000/000/000/000/000/002/2-states-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "operations": {
                "type": "operations",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "file:///000/000/000/000/000/000/002/2-operations-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "proposal": {
                "type": "proposal",
                "checksum": "dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe",
                "url": "file:///000/000/000/000/000/000/002/2-proposal-dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe.jsonld.gz"
            },
            "init_voteproof": {
                "type": "init_voteproof",
                "checksum": "705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64",
                "url": "file:///000/000/000/000/000/000/002/2-init_voteproof-705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64.jsonld.gz"
            },
            "accept_voteproof": {
                "type": "accept_voteproof",
                "checksum": "0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34",
                "url": "file:///000/000/000/000/000/000/002/2-accept_voteproof-0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34.jsonld.gz"
            },
            "states_tree": {
                "type": "states_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "file:///000/000/000/000/000/000/002/2-states_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            }
        },
        "writer": "blockdata-writer-v0.0.1"
    }

    $ aws s3 cp ./blockdata/000/000/000/000/000/000/002 s3://destbucket/blockdata/000/000/000/000/000/000/002 --recursive
    # update mapData blockdata url from "file:///000/000/000/000/000/000/002/" to https://aws/"

    $ ./mc storage set-blockdatamaps $DEPLOY_KEY mapData $NODE --tls-insecure

    $ ./mc storage download map 2 --tls-insecure --node=$NODE
    {
        "_hint": "base-blockdatamap-v0.0.1",
        "hash": "2ojLCZwG5J7xmfoxiBbhvJsc6dDTxDFDsw1nfPneT2xr",
        "height": 2,
        "block": "BcXqCKG5MbQcfuFpPtjvHcNBGeK6Pz3aG2cMcp4MUy9C",
        "created_at": "2021-06-14T03:20:24.887Z",
        "items": {
            "operations_tree": {
                "type": "operations_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-operations_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            },
            "manifest": {
                "type": "manifest",
                "checksum": "6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42",
                "url": "fhttps://aws/2-manifest-6e53950e3ab87008b2bcb9841461588456c3e1069458eb8b150f1bfb97d22d42.jsonld.gz"
            },
            "suffrage_info": {
                "type": "suffrage_info",
                "checksum": "e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02",
                "url": "fhttps://aws/2-suffrage_info-e7584f9b5324566d4c5319db33ece980000f9c29eaf4d17befcc239743788f02.jsonld.gz"
            },
            "states": {
                "type": "states",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "fhttps://aws/2-states-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "operations": {
                "type": "operations",
                "checksum": "d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59",
                "url": "fhttps://aws/2-operations-d890f3ba40375a6b2d331883907dc0a9ca980ce45f7d5dcaca9087278c0b6d59.jsonld.gz"
            },
            "proposal": {
                "type": "proposal",
                "checksum": "dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe",
                "url": "fhttps://aws/2-proposal-dbbce4aaa6aece06596ecd45068008d35a41f592339d8898501b55f5843dbefe.jsonld.gz"
            },
            "init_voteproof": {
                "type": "init_voteproof",
                "checksum": "705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64",
                "url": "fhttps://aws/2-init_voteproof-705af3bd660070813354b572288204d787a949fc5411f3e2bc28e86f07bc1e64.jsonld.gz"
            },
            "accept_voteproof": {
                "type": "accept_voteproof",
                "checksum": "0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34",
                "url": "fhttps://aws/2-accept_voteproof-0d4296d44f96a3de216a90f99d77bf77a00ecd5102d7bbba612b13a57bdf2f34.jsonld.gz"
            },
            "states_tree": {
                "type": "states_tree",
                "checksum": "1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26",
                "url": "fhttps://aws/2-states_tree-1f9877aebf8854fd42154c6e6479ff6a3e379b2762c65995c80f3dff2a357a26.jsonld.gz"
            }
        },
        "writer": "blockdata-writer-v0.0.1"
    }

---------------------------------------------------
Support Operations
---------------------------------------------------

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

| Refer to :ref:`seal command` to check how to create those operations by commands.
