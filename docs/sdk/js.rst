===================================================
Javascript
===================================================

| This is Mitum SDK written in Javascript.

| For more information, please refer to README of `mitum-js-util <https://github.com/ProtoconNet/mitum-js-util>`_.

---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| To use **mitum-js-util** and build it, ``npm`` or ``yarn`` should be installed.

| The development environment is as follows:

.. code-block:: shell

    $ npm --version
    v16.10.0

    $ node --version
    7.24.0

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

.. _js - Make Your First Operation:

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| This tutorial explains how to ``create-account`` by **mitum-js-util**.

| If you want to check how to create other operations, go to :ref:`js - support operations`.

Get Available Account
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Before start, you must hold the account registered in the network.

| Mitum handles only operations sent by accounts that already exist on the network normally.

| An account consists of the following factors.

.. code-block:: none

    1. pairs of (public key, weight); aka `keys`
    - public key has suffix `mpu`
    - The range of each weight should be in 1 <= weight <= 100
    - If an account have single public key, the account is called 'single-sig account', or it's called 'multi-sig account'
    
    1. threshold
    - The range of threshold should be in 1 <= threshold <= 100
    - The sum of all weights of the account should be over or equal to threshold

| If you haven't made an account yet, ask other accounts to create your account first.
| You can get keypairs for your account in :ref:`js - Get Mitum Keypair`` section.
| Hand your (public key, weight) pairs and threshold to the account holder who helped create your new account.

| For signing, you must remember private keys corresponding each public key of the account. **Don't let not allowed users to know your private key!**
| Of course, you must know your account address because you should use the address as ``sender``.

| You are able to create operations with unauthorized account(like fake keys and address) but those operations will be rejected after broadcasting.

| Now, go to the next part to start creating your first operation!

Create Generator
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Most of the elements and factors for an operation are created by ``Generator``.
| For Mitum Currency, use ``Generator.currency``.

| When declaring a ``Generator``, ``network id`` should be provided.
| ``network id`` is up to each network.

| Let's suppose that the network id of the network is ``mitum``.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

| For details about ``Generator``, go to :ref:`js - Major Classes` and refer to **Generator**.

| In addition, you must have an available account on the network.

| Now, you are ready to create operations.

Create Operation Item
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Everything to do by an operation is contained in *operation fact*, not in *operation*.
| *Fact* has the basic information such that ``sender``, ``token``, etc…

| Actually, real constructions for the operation are contained in *Item*.
| That means you must create items for the operation.

| Let's suppose that you want to create an account following conditions below.

.. code-block:: none

    1. The keys and threshold of the account will be,
        - keys(public key, weight): (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 50), (pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu, 50) 
        - threshold: 100

    2. The initial balance of the account will be,
        - balance(currency id, amount): (MCC, 10000), (PEN, 20000)

| Since the number of keys contained in the account is 2, new account will be a *multi-sig account*.

| If every factor of the new account has been decided, create an item!

.. code-block:: javascript

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50) // key(pub, weight)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50) // key(pub, weight)
    
    const keys = currencyGenerator.keys([key1, key2], 100) // createKeys([key1, key2], threshold)

    const amount1 = currencyGenerator.amount("MCC", "10000") // amount(currencyId, amount)
    const amount2 = currencyGenerator.amount("PEN", "20000") // amount(currencyId, amount)
    const amounts = currencyGenerator.amounts([amount1, amount2]); // createAmounts([amount1, amount2])

    const createAccountsItem = currencyGenerator.getCreateAccountsItem(keys, amounts); // createCreateAccountsItem(keys, amounts)

* First, create each key by ``Generator.currency.key(public key, weight)``.
* Second, combine all keys with account threshold by ``Generator.currency.keys(key list, threshold)``.
* Third, create each amount by ``Generator.currency.amount(currencyId, amount)``.
* Forth, combine all amounts by ``Generator.currency.amounts(amount list)``.
* Finally, create an item by ``Generator.currency.getCreateAccountsItem(keys, amounts)``

| Of course, you can customize the content of items by following constraints.

.. code-block:: none

    - `Keys` created by `keys` can contain up to 10 key pairs.
    - `Amounts` created by `amounts` can contain up to 10 amount pairs.
    - Moreover, a `fact` can contain multiple items. The number of items in a fact is up to 10, either.

Create Operation Fact
'''''''''''''''''''''''''''''''''''''''''''''''''''

| *Fact* must have not empty ``items``, ``sender``, ``token``, and ``fact hash``.

| Don't worry about ``token`` and ``fact hash`` because they will be filled automatically by SDK.
| The information you must provide is about ``items`` and ``sender``.

| The way to create items has been introduced in the section above.

| Just be careful that only the account under below conditions can be used as ``sender``.

.. code-block:: none

    1. The account which has been created already.
    2. The account which has sufficient balance of currencies in items.
    3. The account that you(or owners of the account) know its private keys corresponding account public keys.

| Then, create *fact*!

.. code-block:: javascript

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca" // sender's account address; replace with your address
    const createAccountsFact = currencyGenerator.getCreateAccountsFact(senderAddress, [createAccountsItem]) // getCreateAccountsFact(sender's address, item list)

| If you want to create fact with multiple items, put them all in item list of ``Generator.currency.getCreateAccountsFact(sender's address, item list)`` as an array.

Create Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Finally, you are in the step to create operation!

| Only thing you need to prepare is **sender's private key**. It is used for signing fact.
| The signature of a private key is included in ``fact_signs`` as a **fact signature**.
| The sum of weights of all signers in ``fact_signs`` should exceed or be equal to the ``sender``’s threshold.

| **Only the signatures of the sender account’s keys are available to fact_signs!**

| There is ``memo`` in operation but it is not necessary. You can enter something if you need, but be careful because that ``memo`` also affects the ``operation hash``.

| In this example, suppose that ``sender`` is a *single-sig account* which means only a single key exists in the sender’s account.
| If ``sender`` is a *multi-sig account*, you may add multiple signatures to ``fact_signs``.
| What key must sign is decided by the account's threshold and keys' weights.

.. code-block:: javascript

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr" // sender's private key; replace with your private key
    
    const createAccounts = generator.getOperation(createAccountsFact, "") // getOperation(fact, memo)
    createAccounts.addSign(senderPrivateKey); // addSign(private key) add fact signature to fact_signs 

| Use just ``Generator.getOperation(fact, memo)`` for create operations, not ``Generator.currency.getOperation(fact, memo)``.

| Unfortunately, an operation can contain only one fact.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In fact, ``operation`` itself is enough to create an account.

| However, sometimes you may need to wrap multiple operations with a seal.

| As mentioned above, one seal can contain multiple operations.

| The maximum number of operations in a seal is decided by the policy of nodes.
| So check how many operations you can include in a seal before creating seals.

| Anyway, it is simple to create a seal with **mitum-js-util**.

| What you have to prepare is *private key* from Mitum key package without any conditions.
| Any *btc compressed wif* with suffix *mpr* is okay.

.. code-block:: javascript

    const anyPrivateKey = "KyK7aMWCbMtCJcneyBZXGG6Dpy2jLRYfx3qp7kxXJjLFnppRYt7wmpr"

    const operations = [createAccounts]
    const seal = generator.getSeal(anyPrivateKey, operations)

| Like ``getOperation``, use ``Generator.getSeal(signer, operation list)``.

| Put all operations to wrap in *operation list*.

.. _js - support operations:

---------------------------------------------------
Support Operations
---------------------------------------------------

| This section will introduce code example for each operation.

| The following is a list of operations supported by each Mitum model.

+----------------------------+-----------------------------------------------------------------------------------------------+
| Model                      | Support Operations                                                                            |
+============================+===============================================================================================+
| Currency                   | create account, key updater, transfer                                                         |
+----------------------------+-----------------------------------------------------------------------------------------------+
| Currency Extension         | create contract account, withdraw                                                             |
+----------------------------+-----------------------------------------------------------------------------------------------+
| Document                   | create document, update document, (sign document)                                             |
+----------------------------+-----------------------------------------------------------------------------------------------+
| Feefi                      | pool register, pool policy updater, pool deposit, pool withdraw                               |
+----------------------------+-----------------------------------------------------------------------------------------------+
| NFT                        | collection register, collection policy updater, mint, transfer, burn, sign, approve, delegate |
+----------------------------+-----------------------------------------------------------------------------------------------+

Currency
'''''''''''''''''''''''''''''''''''''''''''''''''''

Create Account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| The tutorial for ``create-account`` have been already explained but it'll be re-introduced in one code-block.

| To create a new account you have to prepare,

* The information of the new account: account keys as pairs of (public key, weight), threshold, initial balance as pairs of (currency id, amount).
* Sender's account that has existed already - especially sender's account address and private keys.

| As mentioned before, what private keys must sign the fact is up to the threshold and composition of weights.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50)
    
    const keys = currencyGenerator.keys([key1, key2], 100)

    const amount1 = currencyGenerator.amount("MCC", "10000")
    const amount2 = currencyGenerator.amount("PEN", "20000")
    const amounts = currencyGenerator.amounts([amount1, amount2]);

    const createAccountsItem = currencyGenerator.getCreateAccountsItem(keys, amounts);

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"
    const createAccountsFact = currencyGenerator.getCreateAccountsFact(senderAddress, [createAccountsItem])

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr"
    
    const createAccounts = generator.getOperation(createAccountsFact, "")
    createAccounts.addSign(senderPrivateKey);

| The detailed explanation was omitted. Refer to the beginning part of :ref:`js - Make Your First Operation`.

Key Updater
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This operation is to update keys of the account as its name implies.

| For example,

.. code-block:: none

    - I have an single sig account with keys: (kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu, 100), threshold: 100
    - But I want to replace keys of the account with keys: (22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu, 50), (22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu, 50), threshold: 100
    - Then you can use key-updater operation to reach the goal!

| *Can I change my account from single-sig to multi-sig? or from multi-sig to single-sig?*

| Fortunately, of course, you can!

| To update keys of the account, you have to prepare,

* The account(target) information you want to change the keys - account address and private keys; what private keys need is up to threshold and key weights.
* New keys: pairs of (public key, weights) and threshold
* Sufficient balance in a currency id to pay a fee.

| ``create-account`` and ``transfer`` need ``item`` to create an operation but ``key-updater`` don't need any item for it.
| Just create *fact* right now.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const targetAddress = "JDhSSB3CpRjwM8aF2XX23nTpauv9fLhxTjWsQRm9cJ7umca"
    const targetPrivateKey = "KzejtzpPZFdLUXo2hHouamwLoYoPtoffKo5zwoJXsBakKzSvTdbzmpr"

    const newPub1 = currencyGenerator.key("22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu", 100)
    const newPub2 = currencyGenerator.key("22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu", 100)
    const newKeys = currencyGenerator.keys([newPub1, newPub2], 100)

    const keyUpdaterFact = currencyGenerator.getKeyUpdaterFact(targetAddress, "MCC", newKeys) // getKeyUpdaterFact(target address, currency for fee, new keys)
    
    const keyUpdater = generator.getOperation(keyUpdaterFact, "")
    keyUpdater.addSign(targetPrivateKey) // only one signature since the account is single-sig

* **After updating keys of the account, the keys used before become useless. You should sign operation with private keys of new keypairs of the account.**
* **So record new private keysthreshold somewhere else before sending a key-updater operation to the network.**

Transfer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Finally, you can transfer your tokens to another account.

| As other operations, you have to prepare,

* Sender's account information - account address, and private keys
* Pairs of (currency id, amount) to transfer

| Like ``create-account``, you must create *item* before making *fact*.

| Check whether you hold sufficient balance for each currency id to transfer before sending the operation.

| Before start, suppose that you want to transfer,

* 1000000 MCC token
* 15000 PEN token

| And the receiver is,

* CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca

| Note that up to 10 (currency id, amount) pairs can be included in one item.
| Moreover, up to 10 items can be included in one fact. However, the receiver for each item should be different.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const senderPrivateKey = "KzdeJMr8e2fbquuZwr9SEd9e1ZWGmZEj96NuAwHnz7jnfJ7FqHQBmpr"
    const senderAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca"
    const receiverAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"

    const amount1 = currencyGenerator.amount("MCC", "1000000")
    const amount2 = currencyGenerator.amount("PEN", "15000")
    const amounts = currencyGenerator.amounts([amount1, amount2])

    const transfersItem = currencyGenerator.getTransfersItem(receiverAddress, amounts) // getTransfersItem(receiver address, amounts)
    const transfersFact = currencyGenerator.getTransfersFact(senderAddress, [transfersItem]) // getTransfersFact(sender address, item list)
    
    const transfers = generator.getOperation(transfersFact, "")
    transfers.addSign(senderPrivateKey) // suppose sender is single-sig    

Currency Extension
'''''''''''''''''''''''''''''''''''''''''''''''''''

Create Contract Account
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| You can create a contract account by sending this operation.

| The steps for creating a create-contract-account operation are the same as for create-account.

| However, the difference between contract account and general account is that in the case of contract account, there are no public keys in the account information.

| Therefore, the contract account cannot send or start an operation as an operation sender, and it cannot arbitrarily send tokens from the account to another account.

| Only the owner of the contract account can withdraw tokens sent to it to his account through withdraw operation.

| Below is an example for creating a create-contract-account operation, and the description of the example is omitted because it is very similar to the case of create-account.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator(networkId)
    const currencyGenerator = generator.currency

    const key1 = currencyGenerator.key("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50)
    const key2 = currencyGenerator.key("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50)
    
    const keys = currencyGenerator.keys([key1, key2], 100)

    const amount1 = currencyGenerator.amount("MCC", "10000")
    const amount2 = currencyGenerator.amount("PEN", "20000")
    const amounts = currencyGenerator.amounts([amount1, amount2]);

    const createAccountsItem = currencyGenerator.extension.getCreateContractAccountsItem(keys, amounts);

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"
    const createAccountsFact = currencyGenerator.extension.getCreateContractAccountsFact(senderAddress, [createAccountsItem])

    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr"
    
    const createContractAccounts = generator.getOperation(createContractAccounts, "")
    createContractAccounts.addSign(senderPrivateKey);

Withdraw
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| The token deposited in the contract account can be withdrawn by its owner through the withdraw operation.

.. code-block:: javascript

    import { Generator } from 'mitumc';
    
    const generator = new Generator('mitum')
    const currencyGenerator = generator.currency

    const amount = currencyGenerator.amount("MCC", "100");
    const amounts = currencyGenerator.amounts([amount]);

    const targetAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca";
    const withdrawsItem = currencyGenerator.extension.getWithdrawsItem(targetAddress,  amounts);

    const senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca";
    const withdrawsFact = currencyGenerator.extension.getWithdrawsFact(senderAddress, [withdrawsItem])
   
    const senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr";

    const withdraws = generator.getOperation(withdrawsFact, "")
    withdraws.addSign(senderPrivateKey)

| How to create an operation for document, feefi, and NFT can be found in `README <https://github.com/ProtoconNet/mitum-js-util#readme>`_ in Github.

---------------------------------------------------
Sign
---------------------------------------------------

| To allow an operation to be stored in blocks, whether signatures of the operation satisfy the **condition** should be checked.

| What you have to care about is,

* Has every signature been signed by the private key of the account?
* Is the sum of every weight for each signer greater than or equal to the account threshold?

| Of course, there are other conditions each operation must satisfy but we will focus on **signature** (especially about fact signature) in this section.

| Let's suppose there is a multi-sig account with 3 keys s.t each weight is 30 and threshold is 50.

| That means, 

* (pub1, 30)
* (pub2, 30)
* (pub3, 30)
* threshold: 50

| When this account wants to send an operation, the operation should include at least two fact signatures of different signers.

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

| Therefore, you must add multiple signatures to each operation to satisfy the condition. (use ``Operation.addSign(private key)``)
| Like **CASE4**, it's okay to sign with every private key as long as the sum of their weight >= threshold.

Add Fact Sign to Operation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| Besides adding a fact signature when creating the operation, there is another way to add a new fact signature to the operation.

| To add a new signature to the operation, you have to prepare,

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

| ``signedFromPath`` and ``signedFromObject`` results in operation with the same fact signatures.

| Note that the result operation is not ``Operation`` object of **mitum-js-util**. It's just a dictionary object.
| If you want to add multiple signature at once, you must create another different JSON file then re-sign it with other private keys using ``Signer``.

---------------------------------------------------
Details
---------------------------------------------------

.. _js - Get Mitum Keypair:

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

| **mitum-js-util** will create a random keypair for you!

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

| If you already have your own private key, create keypair with it!

.. code-block:: javascript

    import { getKeypairFromPrivateKey } from 'mitumc'

    const kp = getKeypairFromPrivateKey("Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXimpr")

    kp.getPrivateKey() // Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXimpr
    kp.getPublicKey() // 239uA6z7MxkZfwp5zYKZ6eBbRWk38AvxeyzfHGQM8o2H8mpu

    kp.getRawPrivateKey() // Kz5b6UMxnRvgL91UvNMuRoTfUEAUw7htW2z4kV2PEZUCVPFmdbXi
    kp.getRawPublicKey() //239uA6z7MxkZfwp5zYKZ6eBbRWk38AvxeyzfHGQM8o2H8

Get Keypair from your seed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| You can get a keypair from your seed, too. Even if you don't remember the private key of the keypair, the keypair can be recovered by its seed.
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

| However, it is not available to get an address if the keys or threshold of the account have changed.
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

    const keys = gn.keys([key1, key2, key3], 100)

    const address = keys.address // this is what you want to get!

.. _js - Major Classes:

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''

Generator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Generator`` is the class that helps generate operations for Mitum Currency.

| Before you use ``Generator``, ``network id`` must be set.

* For **Mitum Currency**, use ``Generator.currency``.
* For **Mitum Currency Extension**, use ``Generator.currency.extension``.
* For **Mitum Document**, use ``Generator.document``.
* For **Mitum Feefi**, use ``Generator.feefi``.
* For **Mitum NFT**, use ``Generator.nft``.

.. code-block:: javascript

    import { Generator } from 'mitumc'

    const networkId = 'mitum'
    const generator = new Generator(networkId)

    const currencyGenerator = generator.currency
    const extensionGenerator = generator.currency.extension
    const documentGenerator = generator.document
    const feefiGenerator = generator.feefi
    const nftGenerator = generator.nft

| All methods of ``Generator`` provides are,

.. code-block:: javascript

    /* For Mitum Currency */
    Generator.currency.key(key, weight) // 1 <= $weight <= 100
    Generator.currency.amount(currencyId, amount) // typeof $amount === "string" 
    Generator.currency.keys(keys, threshold) // 1 <= $threshold <= 100
    Generator.currency.amounts(amounts) 
    Generator.currency.getCreateAccountsItem(keys, amounts)
    Generator.currency.getTransfersItem(receiver, amounts)
    Generator.currency.getCreateAccountsFact(sender, items)
    Generator.currency.getKeyUpdaterFact(target, currencyId, keys)
    Generator.currency.getTransfersFact(sender, items)    

    /* For Mitum Currency Extension */
    Generator.currency.extension.getCreateContractAccountsItem(keys, amounts)
    Generator.currency.extension.getWithdrawsItem(target, amounts)
    Generator.currency.extension.getCreateContractAccountsFact(sender, items)
    Generator.currency.extension.getWithdrawsFact(sender, items)

    /* For Mitum Document */
    Generator.document.getCreateDocumentsItem(document, currencyId)
    Generator.document.getUpdateDocumentsItem(document, currencyId)
    Generator.document.getCreateDocumentsFact(sender, items)
    Generator.document.getUpdateDocumentsFact(sender, items)

    /* For Blocksign*/
    Generator.document.blocksign.user(address, signcode, signed)
    Generator.document.blocksign.document(documentId, owner, fileHash, creator, title, size, signers)
    Generator.document.blocksign.getSignDocumentsItem(documentId, owner, currencyId)
    Generator.document.blocksign.getSignDocumentsFact(sender, items)

    /* For Blockcity */
    Generator.document.blockcity.candidate(address, nickname, manifest, count)
    Generator.document.blockcity.userStatistics(hp, strength, agility, dexterity, charisma intelligence, vital)
    Generator.document.blockcity.userDocument(documentId, owner, gold, bankGold, userStatistics)
    Generator.document.blockcity.landDocument(documentId, owner, address, area, renter, account, rentDate, period)
    Generator.document.blockcity.voteDocument(documentId, owner, round, endTime, candidates, bossName, account, office)
    Generator.document.blockcity.historyDocument(documentId, owner, name, account, date, usage, application)

    /* For Feefi */
    Generator.feefi.getPoolRegisterFact(sender, target, initFee, incomeCid, outlayCid, currencyId)
    Generator.feefi.getPoolPolicyUpdaterFact(sender, target, fee, incomeCid, outlayCid, currencyId)
    Generator.feefi.getPoolDepositsFact(sender, pool, incomeCid, outlayCid, amount)
    Generator.feefi.getPoolWithdrawFact(sender, pool, incomeCid, outlayCid, amounts)

    /* For NFT */
    Generator.nft.signer(account, share, signed)
    Generator.nft.signers(total, signers)
    Generator.nft.collectionRegisterForm(target, symbol, name, royalty, uri, whites)
    Generator.nft.collectionPolicy(name, royalty, uri, whites) 
    Generator.nft.mintForm(hash, uri, creators, copyrighters)
    Generator.nft.getMintItem(collection, form, currencyId)
    Generator.nft.getTransferItem(receiver, nftId, currencyId)
    Generator.nft.getBurnItem(nftId, currencyId)
    Generator.nft.getApproveItem(approved, nftId, currencyId)
    Generator.nft.getDelegateItem(collection, agent, mode, currencyId)
    Generator.nft.getSignItem(qualification, nftId, cid)
    Generator.nft.getCollectionRegisterFact(sender, form, currencyId)
    Generator.nft.getCollectionPolicyUpdaterFact(sender, collection, policy, cid)
    Generator.nft.getMintFact(sender, items)
    Generator.nft.getTransferFact(sender, items)
    Generator.nft.getBurnFact(sender, items)
    Generator.nft.getApproveFact(sender, items)
    Generator.nft.getDelegateFact(sender, items)
    Generator.nft.getSignFact(sender, items)

    /* Common */
    Generator.getOperation(fact, memo)
    Generator.getSeal(signKey, operations)

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
| If you would like to use other js packages to export ``Operation`` to file or to print it in JSON format, you don’t need to use ``JSONParser`` of **mitum-js-util**.

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

    JSONParser.getFile(createAccount.dict(), 'createAccount.json'); // getFile(dict object, file path)
    JSONParser.getFile(keyUpdater.dict(), 'keyUpdater.json');
    JSONParser.getFile(transfer.dict(), 'transfer.json');
    JSONParser.getFile(seal, 'seal.json');