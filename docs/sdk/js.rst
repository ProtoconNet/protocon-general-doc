===================================================
Javascript
===================================================

| This is SDK written in Javascript.

| Support models are,

* Mitum Currency
* Mitum Blocksign

| Note that this document introduces how to create operations only for Mitum Currency.

| If you would like to check the way to create operations for Mitum Blocksign and the detail explanation for Mitum Currency, please refer to README of `mitum-js-util <https://github.com/ProtoconNet/mitum-js-util>`_.

---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| To use **mitum-js-util** and build it, ``npm`` or ``yarn`` should be installed.

| Especially, this package has been developed by,

.. code-block:: shell

    $ npm --version
    v16.10.0

    $ node --version
    7.24.0

| ``npm version 16.10.0 or later`` is recommended.

Installation
'''''''''''''''''''''''''''''''''''''''''''''''''''

* Using **npm**,

.. code-block:: shell

    $ npm install mitumc

* Using **yarn**,

.. code-block:: shell

    $ yarn add mitumc

* Using **Git**,

.. code-block:: shell

    $ git clone https://github.com/ProtoconNet/mitum-js-util.git

    $ cd mitum-js-util

    $ sudo npm install -g

    $ cd YOUR_PACKAGE

    $ npm link mitumc

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| This tutorial explains how to ``create-account`` by **mitum-js-util**.

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

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

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

.. code-block:: javascript

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50) // key(pub, weight)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50) // key(pub, weight)
    
    const keys = currencyGenerator.createKeys([key1, key2], 100) // createKeys([key1, key2], threshold)

    const amount1 = currencyGenerator.amount("10000", "MCC") // amount(amount, currencyId)
    const amount2 = currencyGenerator.amount("20000", "PEN") // amount(amount, currencyId)
    const amounts = currencyGenerator.createAmounts([amount1, amount2]); // createAmounts([amount1, amount2])

    const createAccountsItem = currencyGenerator.createCreateAccountsItem(keys, amounts); // createCreateAccountsItem(keys, amounts)

* First, create each key by ``Generator.currency.key(public key, weight)``.
* Second, combine all keys with account threshold by ``Generator.currency.createKeys(key list, threshold)``.
* Third, create each amount by ``Generator.currency.amount(amount, currencyId)``.
* Forth, combine all amounts by ``Generator.currency.createAmounts(amount list)``.
* Finally, create an item by ``Generator.currency.createCreateAccountsItem(keys, amounts)``

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

.. code-block:: javascript

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca" // sender's account address; replace with your address
    const createAccountsFact = currencyGenerator.createCreateAccountsFact(senderAddress, [createAccountsItem]) // createCreateAccountsFact(sender's address, item list)

| If you want to create fact with multiple items, put them all in item list of ``Generator.currency.createCreateAccountsFact(sender's address, item list)`` as an array.

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

.. code-block:: javascript

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr" // sender's private key; replace with your private key
    
    const createAccounts = generator.createOperation(createAccountsFact, "") // createOperation(fact, memo)
    createAccounts.addSign(senderPrivateKey); // addSign(private key) add fact signature to fact_signs 

| Use just ``Generator.createOperation(fact, memo)`` for create operations, not ``Generator.currency.createOperation(fact, memo)``.

| Be sad, an operation can contain only one fact.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In fact, ``operation`` itself is enough to create an account.

| However, sometimes you may need to wrap multiple operations with a seal.

| Mentioned above, one seal can contain multiple operations.

| The maximum of the number of operations in a seal is decided by the policy of nodes.
| So check how many operations you can include in a seal before create seals.

| Anyway, it is simple to create a seal with **mitum-js-util**.

| What you have to prepare is *private key* from Mitum key package without any conditions.
| Any *btc compressed wif* with suffix *mpr* is okay.

.. code-block:: javascript

    const anyPrivateKey = "KyK7aMWCbMtCJcneyBZXGG6Dpy2jLRYfx3qp7kxXJjLFnppRYt7wmpr"

    const operations = [createAccounts]
    const seal = generator.createSeal(anyPrivateKey, operations)

| Like ``createOperation``, use ``Generator.createSeal(signer, operation list)``.

| Put all operations to wrap in *operation list*.

Support Operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

| This section will introduce code example for each operation.

| What Mitum Currency operations **mitum-js-util** supports are,

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

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50)
    
    const keys = currencyGenerator.createKeys([key1, key2], 100)

    const amount1 = currencyGenerator.amount("10000", "MCC")
    const amount2 = currencyGenerator.amount("20000", "PEN")
    const amounts = currencyGenerator.createAmounts([amount1, amount2]);

    const createAccountsItem = currencyGenerator.createCreateAccountsItem(keys, amounts);

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"
    const createAccountsFact = currencyGenerator.createCreateAccountsFact(senderAddress, [createAccountsItem])

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr"
    
    const createAccounts = generator.createOperation(createAccountsFact, "")
    createAccounts.addSign(senderPrivateKey);

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

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const targetAddress = "JDhSSB3CpRjwM8aF2XX23nTpauv9fLhxTjWsQRm9cJ7umca"
    const targetPrivateKey = "KzejtzpPZFdLUXo2hHouamwLoYoPtoffKo5zwoJXsBakKzSvTdbzmpr"

    const newPub1 = currencyGenerator.key("22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu", 100)
    const newPub2 = currencyGenerator.key("22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu", 100)
    const newKeys = currencyGenerator.createKeys([newPub1, newPub2], 100)

    const keyUpdaterFact = currencyGenerator.createKeyUpdaterFact(targetAddress, "MCC", newKeys) // createKeyUpdaterFact(target address, currency for fee, new keys)
    
    const keyUpdater = generator.createOperation(keyUpdaterFact, "")
    keyUpdater.addSign(targetPrivateKey) // only one signature since the account is single-sig

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
| Moreover, up to 10 item can be included in one fact. However, the receiver for each item should be different.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const senderPrivateKey = "KzdeJMr8e2fbquuZwr9SEd9e1ZWGmZEj96NuAwHnz7jnfJ7FqHQBmpr"
    const senderAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca"
    const receiverAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"

    const amount1 = currencyGenerator.amount("1000000", "MCC")
    const amount2 = currencyGenerator.amount("15000", "PEN")
    const amounts = currencyGenerator.createAmounts([amount1, amount2])

    const transfersItem = currencyGenerator.createTransfersItem(receiverAddress, amounts) // createTransfersItem(receiver address, amounts)
    const transfersFact = currencyGenerator.createTransfersFact(senderAddress, [transfersItem]) // createTransfersFact(sender address, item list)
    
    const transfers = generator.createOperation(transfersFact, "")
    transfers.addSign(senderPrivateKey) // suppose sender is single-sig    

| There are other operations that **mitum-js-util** supports, like operations of *Mitum Blocksign*, but this document doesn't provide examples of those operations.
| Refer to `README <https://github.com/ProtoconNet/mitum-js-util/blob/master/README.md>`_ if necessary.

---------------------------------------------------
Sign
---------------------------------------------------

| To allow an operation to store in blocks, whether signatures of the operation satisfy the **condition** should be checked.

| What you have to care about is,

* Is every signature is a signature signed by private key of the account?
* Is the sum of every weight for each signer greater than or equal to the account threshold?

| Of course, there are other conditions each operation must satisfy but we will focus on **signature** (especially about fact signature) in this section.

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

| Therefore, you must add multiple signature to each operation to satisfy the condition. (use ``Operation.addSign(private key)``)
| Like **CASE4**, it's okay to sign with all private keys as long as the sum of those weights >= threshold.

Add Fact Sign to Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Beside adding a fact signature when create the operation, there is another way to add new fact signature to the operation.

| To add new signature to the operation, you have to prepare,

* Private key to sign - it should be that of the sender of the operation.
* Operation as JS dictionary object, or external JSON file.
* Network ID

| First, create ``Signer`` with ``network id`` like ``Generator``.

.. code-block:: javascript

    import { Signer } from 'mitumc'
    
    const networkId = "mitum"
    const signKey = "L3CQHoKPJnK61LZhvvvfRouvAjVVabx2RQXHHhPHbBssgcewjgNimpr"
    const signer = new Signer(networkId, signKey)

| Then, sign now!

.. code-block:: javascript

    const operationJsonPath = "../createAccount.json" // it's an example; replace with your operation path
    const operationObject = createAccount.dict() // createAccount is the operation created by Generator.createOperation
    
    const signedFromPath = signer.signOperation(operationJsonPath)
    const signedFromObject = signer.signOperation(operationObject)

| ``signedFromPath`` and ``signedFromObject`` results in operation with same fact signatures.

| Note that the result operation is not ``Operation`` object of **mitum-js-util**. It's just a dictionary object.
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

| **mitum-js-util** will create random keypair for you!

| Use ``getNewKeypair()``.

.. code-block:: javascript

    import { getNewKeypair } from 'mitumc'

    const kp = getNewKeypair() // returns Keypair

    kp.getPrivateKey() // KzF4ia7G8in3hm7TzSr5k7cNtx46BdEFTzVdnh82vAopqxJG8rHompr
    kp.getPublicKey() // 25jrVNpKr59bYxrWH8eTkbG1iQ8hjvSFKVpfCcDT8oFf8mpu

    kp.getRawPrivateKey() // KzF4ia7G8in3hm7TzSr5k7cNtx46BdEFTzVdnh82vAopqxJG8rHo
    kp.getRawPublicKey() // 25jrVNpKr59bYxrWH8eTkbG1iQ8hjvSFKVpfCcDT8oFf8mpu

Get Keypair From Your Private Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| If you already have own private key, create keypair with it!

.. code-block:: javascript

    import { getKeypairFromPrivateKey } from 'mitumc'

    const kp = getKeypairFromPrivateKey("Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXimpr")

    kp.getPrivateKey() // Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXimpr
    kp.getPublicKey() // 239uA6z7MxkZfwp5zYKZ6eBbRWk38AvxeyzfHGQM8o2H8mpu

    kp.getRawPrivateKey() // Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXi
    kp.getRawPublicKey() //239uA6z7MxkZfwp5zYKZ6eBbRWk38AvxeyzfHGQM8o2H8

Get Keypair from your seed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| You can get keypair from your seed, too. Even if you don't remeber the private key of the keypair, the keypair can be recovered by it's seed.
| Note that string seed length >= 36.

.. code-block:: javascript

    import { getKeypairFromSeed } from 'mitumc'

    const kp = getKeypairFromSeed("Thelengthofseedshouldbelongerthan36characters.Thisisaseedfortheexample.")

    kp.getPrivateKey() // KynL1wNZjuXvZDboEugU4sWKZ6ck5GTMqtv6eod8Q7C4NaB4kfZPmpr
    kp.getPublicKey() // fyLbH5cUwNTihaW2YkJkAzeoLvTNTzf98r8dtCkjXbuqmpu

    kp.getRawPrivateKey() // KynL1wNZjuXvZDboEugU4sWKZ6ck5GTMqtv6eod8Q7C4NaB4kfZP
    kp.getRawPublicKey() // fyLbH5cUwNTihaW2YkJkAzeoLvTNTzf98r8dtCkjXbuq

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

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const gn = new Generator('mitum').currency

    const key1 = gn.key("vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu", 40)
    const key2 = gn.key("29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu", 30)
    const key3 = gn.key("uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu", 30)

    const keys = gn.createKeys([key1, key2, key3], 100)

    const address = keys.address // this is what you want to get!

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''

Generator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Generator`` is the class that helps generate operations for Mitum Currency.

| Before you use ``Generator``, ``network id`` must be set.

* For **Mitum Currency**, use ``Generator.currency``.
* For **Mitum Blocksign**, use ``Generator.blockSign``.

| For details of generating operations for **Mitum Blocksign**. refer to `README <https://github.com/ProtoconNet/mitum-js-util/blob/master/README.md>`_.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator(networkId)

    const currencyGenerator = generator.currency
    const blocksignGenerator = generator.blockSign

| All methods of ``Generator`` provides are,

.. code-block:: javascript

    /* For Mitum Currency */
    Generator.currency.key(key, weight) // 1 <= $weight <= 100
    Generator.currency.amount(big, cid) // typeof $big === "string" 
    Generator.currency.createKeys(keys, threshold) // 1 <= $threshold <= 100
    Generator.currency.createAmounts(amounts) 
    Generator.currency.createCreateAccountsItem(keys_o, amounts)
    Generator.currency.createTransfersItem(receiver, amounts)
    Generator.currency.createCreateAccountsFact(sender, items)
    Generator.currency.createKeyUpdaterFact(target, cid, keys_o)
    Generator.currency.createTransfersFact(sender, items)    

    /* For Mitum Blocksign */
    Generator.blockSign.createCreateDocumentsItem(fileHash, did, signcode, title, size, cid, signers, signcodes)
    Generator.blockSign.createSignDocumentsItem(owner, did, cid)
    Generator.blockSign.createTransferDocumentsItem(owner, receiver, did, cid)
    Generator.blockSign.createBlockSignFact(factType, sender, items)

    /* Common */
    Generator.createOperation(fact, memo)
    Generator.createSeal(signKey, operations)

Signer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Signer`` is the class for adding new fact signature to already create operations.

| Like ``Generator``, ``network id`` must be set.

| You have to prepare *private key* to sign, too.

| ``Signer`` provides only one method, that is,

.. code-block:: javascript

    Signer.signOperation(operation)

| To check the exact usage of ``Signer``, go back to **Make Your First Operation - Sign**.

JSONParser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This class is constructed just for convenience.
| If you would like to use other js package to export ``Operation`` to file or to print it in JSON format, you don't need to use ``JSONParser`` of **mitum-js-util**.

.. code-block:: javascript

    import { Generator, JSONParser } from 'mitumc'

    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    // ... omitted
    // ... create operations
    // ... refer to above `Make Your First Operation`
    // ... suppose you have already made operations - createAccount, keyUpdater, transfer and a seal - seal

    JSONParser.toJSONString(createAccount.dict()) // print operation createAccount in JSON
    JSONParser.toJSONString(keyUpdater.dict()) // print operation keyUpdater in JSON
    JSONParser.toJSONString(transfer.dict()) // print operation transfer in JSON
    JSONParser.toJSONString(seal) // print seal seal in JSON

    JSONParser.generateFile(createAccount.dict(), 'createAccount.json'); // generateFile(dict object, file path)
    JSONParser.generateFile(keyUpdater.dict(), 'keyUpdater.json');
    JSONParser.generateFile(transfer.dict(), 'transfer.json');
    JSONParser.generateFile(seal, 'seal.json');