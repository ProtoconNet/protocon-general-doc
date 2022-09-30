===================================================
Seal and Operation
===================================================

.. _operation:

---------------------------------------------------
Operation
---------------------------------------------------

| In the Mitum blockchain network, an operation is **a unit of command that changes data**.

| For example, Mitum Currency has operations of ``create-account``, ``transfer``, ``key-updater``, ``currency-register``, ``currency-policy-updater``, and ``suffrage-infration``.

| Each operation requires a signature made with a private key according to its contents.

| The fact in the operation contains the contents to be executed and the hash value summarizing the body of the fact.

.. _fact:
.. _token:

---------------------------------------------------
Fact and token
---------------------------------------------------

| Every operation contains a *fact*. In other words, **the content of the operation is actually contained in the fact**.

| Facts play an important role in Mitum blockchain network.

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

| The following json file is an example of fact.

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
* In the example above, anyone can calculate the fact hash if they know: the sender, receiver, currencyID, amount of currency, and a specific token value used.
* Therefore, anyone can inquire whether the corresponding operation has been processed with the fact hash.

| A *fact hash* is like a **public proof** recorded in a blockchain. There can be various applications depending on how a user uses the evidence disclosed in the blockchain.
| For example, even an outsider who does not have a direct account in the blockchain can check the fact hash, the only value indicating whether the operation is processed or not, and make the implementation based on that.

| In addition, facts and tokens can practically be used in models that deal with various data including remittance.

.. _seal:

---------------------------------------------------
Seal
---------------------------------------------------

| *Seal* is **a collection of operations** transmitted to the network. In other words, the operation is contained in the seal and transmitted.

* To transmit the seal, a signature made with a private key is required.
* To create signature, you must use the private key created in Mitum's keypair package.
* Seal can contain up to 100 operations.

| The private key used for the signature has nothing to do with the blockchain account. In other words, it doesn't have to be the private key used by the account.

---------------------------------------------------
Send
---------------------------------------------------

| After creating an operation, the client creates and attaches a signature.

* Create as many operations as necessary within the maximum number able to be included in the seal, and put them in the seal.
* Create and put a signature on the seal.
* Send seal to Mitum node.

---------------------------------------------------
Stored in Block
---------------------------------------------------

| The operation transmitted to the Blockchain network changes the state of the account if it is normal and is finally saved in the block.
| Whether the operation is confirmed and saved in the block can be checked through :ref:`rest api`.