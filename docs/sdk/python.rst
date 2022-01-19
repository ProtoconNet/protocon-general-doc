===================================================
Python
===================================================

| This is SDK written in Python.

| Support models are,

* Mitum Currency
* Mitum Blocksign

| Note that this document introduces how to create operations only for Mitum Currency.

| If you would like to check the way to create operations for Mitum Blocksign and the detail explanation for Mitum Currency, please refer to README of `mitum-py-util <https://github.com/ProtoconNet/mitum-py-util>`_.

---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| This package has been developed by,

.. code-block:: shell

    $ python --version
    Python 3.9.2

| ``python 3.9 or later`` is recommended.

Installation
'''''''''''''''''''''''''''''''''''''''''''''''''''

* Using **Git**,

.. code-block:: shell

    $ git clone https://github.com/ProtoconNet/mitum-py-util.git

    $ cd mitum-py-util

    $ python setup.py install

| If setup.py doesn't work properly, please just install necessary packages with requirements.txt before running setup.py.

.. code-block:: shell

    $ pip install -r requirements.txt

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| This tutorial explains how to ``create-account`` by **mitum-py-util**.

| If you want to check how to create ``key-updater`` and ``transfer`` operation, go **Support Operations** at the end of this section.

Get Available Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Before start, you must hold the account registered in the network.

| In Mitum Currency, only already existing account can create operations able to be stored in block.

| An account consists of following factor.

.. code-block:: none

    1. pairs of (public key, weight); aka `keys`
    - public key has suffix `mpu`
    - The range of each weight should be in 1 <= weight <= 100
    - If an account have single public key, the account is called 'single-sig account', or it's called 'multi-sig account'
    
    2. threshold
    - The range of threshold should be in 1 <= threshold <= 100
    - The sum of all weights of the account should be over or equal to threshold

| If you haven't have any account yet, ask some other account to create your account first.
| You can get keypairs for your account in **Details - Get Mitum Keypair** section.
| Hand your (public key, weight) pairs and threshold to the account holder who helps make your new account.

| For signing, private keys corresponding each public key of the account must be remembered. **Don't let not allowed users to know your private key!**
| Of course, you must know your account address because you should use the address as ``sender``.

| You are able to create operations with unauthorized account(like fake keys and address) but those operations will be rejected after broadcasting.

| Now, go to next part to start to create your first operation!

Create Generator
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Most of elements and factors for an operation are created by ``Generator``.
| For Mitum Currency, use ``Generator.currency``.

| When declare a ``Generator``, ``network id`` should be provided.
| ``network id`` is up to each network.

| Let's suppose that the network id of the network is ``mitum``.

.. code-block:: python

    from mitumc import Generator

    networkId = 'mitum'
    generator = Generator('mitum')
    currencyGenerator = generator.currency

| For details about ``Generator``, go to **Details - Major Classes** and refer to **Generator**.

| In addition, you must have available account on the network.

| Now, it's done to create operations.

Create Operation Item
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Everything to do by an operation is contained in *operation fact*, not in *operation*.
| *Fact* have the basic information such that ``sender``, ``token``, etc...

| Actually, real constructions for the operation are contained in *Item*.
| That means you must create items for the operation.

| Let's suppose that you want to create an account following below conditions.

.. code-block:: none

    1. The keys and threshold of the account will be,
        - keys(public key, weight): (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 50), (pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu, 50) 
        - threshold: 100

    2. The initial balance of the account will be,
        - balance(currency id, amount): (MCC, 10000), (PEN, 20000)

| Since the number of keys contained in the account is 2, new account will be *multi-sig account*.

| If every factor of new account have been decided, create an item!

.. code-block:: python

    key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50) # key(public key, weight)
    key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50)
    keys = currencyGenerator.createKeys([key1, key2], 100) # createKeys(keyList, threshold)

    amount1 = currencyGenerator.amount(10000, 'MCC') # amount(amount, currency id)
    amount1 = currencyGenerator.amount(20000, 'PEN')
    amounts = currencyGenerator.createAmounts([amount]) # createAmounts(amountList)

    createAccountsItem = currencyGenerator.createCreateAccountsItem(keys, amounts)

* First, create each key by ``Generator.currency.key(public key, weight)``.
* Second, combine all keys with account threshold by ``Generator.currency.createKeys(key list, threshold)``.
* Third, create each amount by ``Generator.currency.amount(amount, currencyId)``.
* Forth, combine all amounts by ``Generator.currency.createAmounts(amount list)``.
* Finally, create an item by ``Generator.currency.createCreateAccountsItem(keys, amounts)

| Of course you can customize the content of items by following constrains.

.. code-block:: none

    - `Keys` created by `createKeys` can contain up to 10 key pairs.
    - `Amounts` created by `createAmounts` can contain up to 10 amount pairs.
    - Moreover, a `fact` can contain multiple items. The number of items in a fact is up to 10, either.

Create Operation Fact
'''''''''''''''''''''''''''''''''''''''''''''''''''

| *Fact* must have not empty ``items``, ``sender``, ``token``, and ``fact hash``.

| Don't worry about ``token`` and ``fact hash`` because they will be filled automatically by SDK.
| The information you must provide is about ``items`` and ``sender``.

| The way to create items has been introduced above section.

| Just be careful that only the account under below conditions can be used as ``sender``.

.. code-block:: none

    1. The account which has been created already.
    2. The account which has sufficient balance of currencies in items.
    3. The account that you(or owners of the account) know its private keys corresponding account public keys.

| Then, create *fact*!

.. code-block:: python

    senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca" # sender's account address; replace with your address
    createAccountsFact = currencyGenerator.createCreateAccountsFact(senderAddress, [createAccountsItem]) # createCreateAccountsFact(sender's address, item list)

| If you want to create fact with multiple items, put them all in item list as an array of ``Generator.currency.createCreateAccountsFact(sender's address, item list)``

Create Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Finally, you are in the step to create operation!

| Only thing you need to prepare is **sender's private key**. It is used for signing fact.
| The signature of a private key is included to ``fact_signs`` as a **fact signature**.
| The sum of weights of all signers in ``fact_signs`` should exceeds or be equal to ``sender``'s threshold.

| **Only the signatures of sender account's keys are available to fact_signs!**

| There is ``memo`` in operation but it is not necessary. You can enter something if you need, but be careful because that ``memo`` also affect to ``operation hash``.

| In this example, supposed that ``sender`` is *single-sig account*. That means, only one key exist in the sender's account.
| If ``sender`` is *multi-sig account*, you may add multiple signatures to ``fact_signs``.
| What key must sign is decided by the account's threshold and keys' weights.

.. code-block:: python

    senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr" # sender's private key; replace with your private key
    
    createAccounts = generator.createOperation(createAccountsFact, "") # createOperation(fact, memo)
    createAccounts.addFactSign(senderPrivateKey); # addFactSign(private key) add fact signature to fact_signs 

| Use just ``Generator.createOperation(fact, memo)`` for create operations, not ``Generator.currency.createOperation(fact, memo)``.

| Be sad, an operation can contain only one fact.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In fact, ``operation`` itself is enough to create an account.

| However, sometimes you may need to wrap multiple operations with a seal.

| Mentioned above, one seal can contain multiple operations.

| The maximum of the number of operations in a seal is decided by the policy of nodes.
| So check how many operations you can include in a seal before create seals.

| Anyway, it is simple to create a seal with **mitum-py-util**.

| What you have to prepare is *private key* from Mitum key package without any conditions.
| Any *btc compressed wif* with suffix *mpr* is okay.

.. code-block:: python

    signKey = "L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr"

    operations = [createAccounts]
    seal = generator.createSeal(signKey, operations)

| Like ``createOperation``, use ``Generator.createSeal(signer, operation list)``.

| Put all operations to wrap in *operation list*.

Support Operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

| This section will introduce code example for each operation.

| What Mitum Currency operations **mitum-py-util** supports are,

* Create Account
* Key Updater
* Transfer

Create Account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| The tutorial for ``create-account`` have been already explained but it'll be re-introduced in one code-block.

| To create new account you have to prepare,

* The information of new account: account keys as pairs of (public key, weight), threshold, initial balance as pairs of (currency id, amount)
* Sender's account that has existed already - especially sender's account address and private keys.

| Mentioned before, what private keys must sign the fact is up to the threshold and composition of weights.

.. code-block:: python

    from mitumc import Generator

    senderPrivateKey = "L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr"
    senderAddress = "5fbQg8K856KfvzPiGhzmBMb6WaL5AsugUnfutgmWECPbmca"

    generator = Generator('mitum')
    gn = generator.currency

    key = gn.key("2177RF13ZZXpdE1wf7wu5f9CHKaA2zSyLW5dk18ExyJ84mpu", 100)
    keys = gn.createKeys([key], 100)

    amount = gn.amount(100, 'MCC')
    amounts = gn.createAmounts([amount])

    createAccountsItem = gn.createCreateAccountsItem(keys, amounts)
    createAccountsFact = gn.createCreateAccountsFact(srcAddr, [createAccountsItem])

    createAccounts = generator.createOperation(createAccountsFact, "")
    createAccounts.addFactSign(srcPriv)

| The detailed explanation was omitted. See at the start of 'Make Your First Operation'.

Key Updater
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This operation is literally to update keys of the account.

| For example,

.. code-block:: none

    - I have an single sig account with keys: (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 100), threshold: 100
    - But I want to replace keys of the account with keys: (22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu, 50), (22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu, 50), threshold: 100
    - Then you can use key-updater operation to reach the goal!

| *Can I change my account from single-sig to multi-sig? or from multi-sig to single-sig?*

| Fortunately, of course, you can!

| To update keys of the account, you have to prepare,

* The account(target) information you want to change the keys - account address and private keys; what private keys are need is up to threshold and key weights.
* New keys: pairs of (public key, weights) and threshold
* Sufficient balance of a currency id to pay some fee.

| ``create-account`` and ``transfer`` need ``item`` to create an operation but ``key-updater`` don't need any item for it.
| Just create *fact* right now.

.. code-block:: python

    from mitumc import Generator

    targetPrivateKey = "KzejtzpPZFdLUXo2hHouamwLoYoPtoffKo5zwoJXsBakKzSvTdbzmpr"
    targetAddress = "JDhSSB3CpRjwM8aF2XX23nTpauv9fLhxTjWsQRm9cJ7umca"

    generator = Generator('mitum')
    gn = generator.currency

    key1 = gn.key("22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu", 50)
    key2 = gn.key("22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu", 50)
    keys = gn.createKeys([key1, key2], 100)

    keyUpdaterFact = gn.createKeyUpdaterFact(targetAddress, keys, "MCC") # createKeyUpdaterFact(target address, new keys, currency id for fee)

    keyUpdater = generator.createOperation(keyUpdaterFact, "")
    keyUpdater.addFactSign(targetPrivateKey)

* **After updating keys of the account, the keys used before becomes useless. You should sign operation with private keys of new keypairs of the account.**
* **So record new private keys somewhere before send key-updater operation to the network.**

Transfer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Finally, you can transfer your tokens to another account.

| As other operations, you have to prepare,

* Sender's account information - account address, and private keys
* Pairs of (currency id, amount) to transfer

| Like ``create-account``, you must create *item* before making *fact*.

| Check whether you hold sufficient balance for each currency id to transfer before sending operation.

| Before start, suppose that you want to transfer,

* 1000000 MCC token
* 15000 PEN token

| And receiver is,

* CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca

| Note that up to 10 (currency id, amount) pairs can be included in one item.
| Moreover, up to 10 item can be included in one item. However, the receiver for each item should be different.

.. code-block:: python

    from mitumc import Generator

    generator = Generator('mitum')
    gn = generator.currency

    senderPrivateKey = "KzdeJMr8e2fbquuZwr9SEd9e1ZWGmZEj96NuAwHnz7jnfJ7FqHQBmpr"
    senderAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca"
    receiverAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"

    amount = gn.amount(1000000, 'MCC')
    amount = gn.amount(15000, 'PEN')
    amounts = gn.createAmounts([amount1, amount2])

    transfersItem = gn.createTransfersItem(receiverAddress, amounts) # createTransfersItem(receiver address, amounts)
    transfersFact = gn.createTransfersFact(senderAddress, [transfersItem]) # createTransfersFact(sender addrewss, item list)

    transfers = generator.createOperation(transfersFact, "")
    transfers.addFactSign(senderPrivateKey)  

| There are other operations that **mitum-py-util** supports, like operations of *Mitum Blocksign*, but this document doesn't provide examples of those operations.
| Refer to `README <https://github.com/ProtoconNet/mitum-py-util/blob/master/README.md>`_ if necessary.

---------------------------------------------------
Sign
---------------------------------------------------

| To allow an operation to store in blocks, whether signatures of the operation satisfy the **condition** should be checked.

| What you have to care about is,

* Is every signature is a signature signed by private key of the account?
* Is the sum of every weight for each signer greater than or equal to the account threshold?

| Of course, there are other conditions each operation must satisfy but we will focus on **signature**(especially about fact signature) in this section.

| Let's suppose there is an multi-sig account with 3 keys s.t each weight is 30 and threshold is 50.

| That means, 

* (pub1, 30)
* (pub2, 30)
* (pub3, 30)
* threshold: 50

| When this account want to send an operation, the operation should include at least two fact signatures of different signers.

1. CASE1: fact signatures signed by pub1's private key and pub2's private key

   1. the sum of pub1's weight and pub2's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

2. CASE2: fact signatures signed by pub2's private key and pub3's private key

   1. the sum of pub2's weight and pub3's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

3. CASE3: fact signatures signed by pub1's private key and pub3's private key

   1. the sum of pub1's weight and pub3's weight: 60
   2. the sum of weights = 60 > threshold = 50
   3. So the operation with these two fact signatures is available

4. CASE4: fact signatures signed by pub1's private key, pub2's private key, pub3's private key

   1. the sum of pub1's weight, pub2's weight and pub3's weight: 90
   2. the sum of weights = 90 > threshold = 50
   3. So the operation with these two fact signatures is available

| Therefore, you must add multiple signature to each operation to satisfy the condition. (use ``Operation.addFactSign(private key)``)
| Like **CASE4**, it's okay to sign with all private keys as long as the sum of those weights >= threshold.

Add Fact Sign to Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Beside adding a fact signature when create the operation, there is another way to add new fact signature to the operation.

| To add new signature to the operation, you have to prepare,

* Private key to sign - it should be that of the sender of the operation.
* Operation as JS dictionary object, or external JSON file.
* Network ID

| First, create ``Signer`` with ``network id`` like ``Generator``.

.. code-block:: python

    from mitumc import Signer

    networkId = 'mitum'
    signKey = 'L1V19fBjhnxNyfuXLWw6Y5mjFSixzdsZP4obkXEERskGQNwSgdm1mpr'
    signer = Signer(networkId, signKey)

| Then, sign now!

.. code-block:: python

    signed = signer.signOperation('operation.json') # signOperation(filePath)

| Note that the result operation is not ``Operation`` object of **mitum-py-util**. It's just a dictionary object.
| If you want to add multiple signature at once, you must create another different JSON file then re-sign it with other private keys using ``Signer``.

---------------------------------------------------
Details
---------------------------------------------------

Get Mitum Keypair
'''''''''''''''''''''''''''''''''''''''''''''''''''

| We will introduce how to create Mitum keypairs!

| Before start, we want to let you know something important; About type suffix.

| *Address*, *private key*, and *public key* in Mitum have specific type suffixes. They are,

* Account Address: ``mca``
* Private Key: ``mpr``
* Public Key: ``mpu``

| For example, an single-sig account looks like,

* Account Address: ``9XyYKpjad2MSPxR4wfQHvdWrZnk9f5s2zc9Rkdy2KT1gmca``
* Private Key: ``L11mKUECzKouwvXwh3eyECsCnvQx5REureuujGBjRuYXbMswFkMxmpr``
* Public Key: ``28Hhy6jwkEHx75bNLmG66RQu1LWiZ1vodwRTURtBJhtPWmpu``

| There are three methods to create a keypair.

Just Create New Keypair
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| **mitum-py-util** will create random keypair for you!

| Use ``getNewKeypair()``.

.. code-block:: python

    from mitumc.key import getNewKeypair

    # get new Keypair
    kp = getNewKeypair() # returns BTCKeyPair
    kp.privateKey # KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr
    kp.publicKey # 24TbbrNYVngpPEdq6Zc5rD1PQSTGQpqwabB9nVmmonXjqmpu

Get Keypair From Your Private Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| If you already have own private key, create keypair with it!

.. code-block:: python

    from mitumc.key import getKeypairFromPrivateKey

    # get Keypair from your private key
    pkp = getKeypairFromPrivateKey("L2ddEkdgYVBkhtdN8HVXLZk5eAcdqXxecd17FDTobVeFfZNPk2ZDmpr")

Get Keypair From Your Seed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| You can get keypair from your seed, too. Even if you don't remeber the private key of the keypair, the keypair can be recovered by it's seed.
| Note that string seed length >= 36.

.. code-block:: python

    from mitumc.key import getKeypairFromSeed

    # get Keypair from your seed
    skp = getKeypairFromSeed("Thisisaseedforthisexample.len(seed)>=36.")

Get Account Address with Keys
'''''''''''''''''''''''''''''''''''''''''''''''''''

| You can calcualte address from threshold, and every (public key, weight) pair of the account.

| However, it is not available to get address if keys or threshold of the account have changed.
| This method is available only for the account that have not changed yet.

| The account information for the example is,

* key1: (vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu, 40)
* key2: (29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu, 30)
* key3: (uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu, 30)
* threshold: 100

.. code-block:: python

    from mitumc import Generator

    gn = Generator('mitum').currency

    pub1 = "vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu"
    pub2 = "29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu"
    pub3 = "uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu"

    key1 = gn.key(pub1, 40)
    key2 = gn.key(pub2, 30)
    key3 = gn.key(pub3, 30)

    keys = gn.createKeys([key1, key2, key3], 100)
    address = keys.address # your address

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''

Generator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Generator`` is the class that helps generate operations for Mitum Currency.

| Before you use ``Generator``, ``network id`` must be set.

* For **Mitum Currency**, use ``Generator.currency``.
* For **Mitum Blocksign**, use ``Generator.blockSign``.

| For details of generating operations for **Mitum Blocksign**. refer to `README <https://github.com/ProtoconNet/mitum-py-util/blob/master/README.md>`_.

.. code-block:: python

    from mitumc import Generator

    generator = Generator('mitum')
    currencyGenerator = generator.currency
    blockSignGenerator = generator.blockSign

| All methods of ``Generator`` provides are,

.. code-block:: python

    /* For Mitum Currency */
    Generator.currency.key(key, weight)
    Generator.currency.amount(amount, currencyId)
    Generator.currency.createKeys(keys, threshold)
    Generator.currency.createAmounts(amounts) 
    Generator.currency.createCreateAccountsItem(keys, amounts)
    Generator.currency.createTransfersItem(receiver, amoutns)
    Generator.currency.createCreateAccountsFact(sender, items)
    Generator.currency.createKeyUpdaterFact(target, cid, keys)
    Generator.currency.createTransfersFact(sender, items)

    /* For Mitum Blocksign */
    Generator.blockSign.createCreateDocumentsItem(filehash, did, signcode, title, size, cid, signers, signcodes)
    Generator.blockSign.createSignDocumentsItem(owner, documentid, cid)
    Generator.blockSign.createTransferDocumentsItem(owner, receiver, documentid, cid)
    Generator.blockSign.createBlockSignFact(operationType, sender, itemList)

    /* Common */
    Generator.createOperation(fact, memo)
    Generator.createSeal(signKey, operations)

Signer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Signer`` is the class for adding new fact signature to already create operations.

| Like ``Generator``, ``network id`` must be set.

| You have to prepare *private key* to sign, too.

| ``Signer`` provides only one method, that is,

.. code-block:: python

    Signer.signOperation(operation)

| To check the exact usage of ``Signer``, go back to **Make Your First Operation - Sign**.

JSONParser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This class is constructed just for convenience.
| If you would like to use other js package to export ``Operation`` to file or to print it in JSON format, you don't need to use ``JSONParser`` of **mitum-py-util**.

.. code-block:: python

    from mitumc import JSONParser

    # ... omitted
    # ... create operations
    # ... refer to above `Make Your First Operation`
    # ... suppose you have already made operations - createAccount, keyUpdater, transfer and a seal - seal

    JSONParser.toJSONString(createAccount.dict()) # print operation createAccount in JSON
    JSONParser.toJSONString(keyUpdater.dict()) # print operation keyUpdater in JSON
    JSONParser.toJSONString(transfer.dict()) # print operation transfer in JSON
    JSONParser.toJSONString(seal) # print seal seal in JSON

    JSONParser.generateFile(createAccount.dict(), 'createAccount.json') # generateFile(dict object, file path)
    JSONParser.generateFile(keyUpdater.dict(), 'keyUpdater.json')
    JSONParser.generateFile(transfer.dict(), 'transfer.json')
    JSONParser.generateFile(seal, 'seal.json')