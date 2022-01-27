===================================================
Java
===================================================

| This is SDK written in Java.

| Support models are,

* Mitum Currency
* Mitum Blocksign

| Note that this document introduces how to create operations only for Mitum Currency.

| If you would like to check the way to create operations for Mitum Blocksign and the detail explanation for Mitum Currency, please refer to README of `mitum-java-util <https://github.com/ProtoconNet/mitum-java-util>`_.

---------------------------------------------------
Get Started
---------------------------------------------------

Prerequisite and Requirements
'''''''''''''''''''''''''''''''''''''''''''''''''''

| This package has been developed by,

.. code-block:: shell

    $ java -version
    openjdk version "16.0.1" 2021-04-20
    OpenJDK Runtime Environment (build 16.0.1+9-Ubuntu-120.04)
    OpenJDK 64-Bit Server VM (build 16.0.1+9-Ubuntu-120.04, mixed mode, sharing)

    $ javac -version
    javac 16.0.1

| And this package is using this external Java Library.

* `bitcoinj v0.14.7 <https://bitcoinj.org/>`_

Installation
'''''''''''''''''''''''''''''''''''''''''''''''''''

| `Download jar file <https://github.com/ProtoconNet/mitum-java-util/tree/main/release>`_ from the repository.

| Now, the latest version is ``mitum-java-util-1.3.0.jar``

| Using *Gradle*,

.. code-block:: shell

    implementation files('./lib/mitum-java-util-1.3.0.jar')

---------------------------------------------------
Make Your First Operation
---------------------------------------------------

| This tutorial explains how to ``create-account`` by **mitum-java-util**.

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
| For Mitum Currency, use ``Generator.currency()``.

| When declare a ``Generator``, ``network id`` should be provided.
| ``network id`` is up to each network.

| Let's suppose that the network id of the network is ``mitum``.

.. code-block:: java

    /*
    import org.mitumc.sdk.Generator
    import org.mitumc.sdk.operation.currency.CurrencyGenerator;
    */
    String id = "mitum";
    Generator generator = Generator.get(id);
    CurrencyGenerator cgn = generator.currency();

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

.. code-block:: java

    /*
    import org.mitumc.sdk.key.*;
    import org.mitumc.sdk.operation.currency.*;
    */
    Key key1 = generator.currency().newKey("kpYjRwq6gQrjvzeqQ91MNiCcR9Beb9sD67SuhQ6frPGwmpu", 50); // newKey(public key, weight)
    Key key2 = generator.currency().newKey("pWoFhRP3C7ocebSRPxTPfeaJZpnyKpEkxQqi6fAD4SHompu", 50);
    Keys keys = generator.currency().newKeys(new Key[]{ key1, key2 }, 100); // newKeys(key list, threshold)

    Amount amount1 = generator.currency().newAmount("MCC", "10000"); // newAmount(currency id, amount)
    Amount amount2 = generator.currency().newAmount("PEN", "20000");

    CreateAccountsItem item = generator.currency().newCreateAccountsItem(keys, new Amount[]{ amount1, amount2 }); // newCreateAccountsItem(keys, amount list)

* First, create each key by ``Generator.currency().newKey(public key, weight)``.
* Second, combine all keys with account threshold by ``Generator.currency().newKeys(key list, threshold)``.
* Third, create each amount by ``Generator.currency().newAmount(currencyId, amount)``.
* Finally, create an item by ``Generator.currency().createCreateAccountsItem(keys, amount list)``

| Of course you can customize the content of items by following constrains.

.. code-block:: none

    - `Keys` created by `newKeys` can contain up to 10 key pairs.
    - `Amount list` s.t each amount created by `newAmounts` can contain up to 10 in one item.
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

.. code-block:: java

    /*
    import org.mitumc.sdk.operation.currency.*; 
    */
    String senderAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca"; // sender's account address; replace with your address
    CreateAccountsFact fact = generator.currency().newCreateAccountsFact(senderAddress, new CreateAccountsItem[]{ item });  // newCreateAccountsFact(sender address, item list)

| If you want to create fact with multiple items, put them all in item list of ``Generator.currency().newCreateAccountsFact(sender's address, item list)`` as an array.

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

.. code-block:: java

    /*
    import org.mitumc.sdk.operation.Operation;
    */
    String senderPrivateKey = "KxD8T82nfwsUmQu3iMXENm93YTTatGFp1AYDPqTo5e6ycvY1xNXpmpr"; // sender's private key; replace with your private key
    
    Operation operation = generator.newOperation(fact); // newOperation(fact, memo); enter memo if you need
    operation.addSign(senderPrivateKey); // addSign(private key) add fact signature to fact_signs

| Use just ``Generator.newOperation(fact, memo)`` for create operations, not ``Generator.currency().newOperation(fact, memo)``.

| Be sad, an operation can contain only one fact.

Create Seal
'''''''''''''''''''''''''''''''''''''''''''''''''''

| In fact, ``operation`` itself is enough to create an account.

| However, sometimes you may need to wrap multiple operations with a seal.

| Mentioned above, one seal can contain multiple operations.

| The maximum of the number of operations in a seal is decided by the policy of nodes.
| So check how many operations you can include in a seal before create seals.

| Anyway, it is simple to create a seal with **mitum-java-util**.

| What you have to prepare is *private key* from Mitum key package without any conditions.
| Any *btc compressed wif* with suffix *mpr* is okay.

.. code-block:: java

    String signKey = "KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr";
    HashMap<String, Object> seal = gn.newSeal(signKey, new Operation[]{ operation }); // newSeal(sign key, operation list)

| Like ``newOperation``, use ``Generator.newSeal(signer, operation list)``.

| Put all operations to wrap in *operation list*.

Support Operations
'''''''''''''''''''''''''''''''''''''''''''''''''''

| This section will introduce code example for each operation.

| What Mitum Currency operations **mitum-java-util** supports are,

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

.. code-block:: java

    /*
    import org.mitumc.sdk.key.*;
    import org.mitumc.sdk.Generator;
    import org.mitumc.sdk.operation.Operation;
    import org.mitumc.sdk.operation.currency.*;
    */

    String senderPrivateKey = "KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr";
    String senderAddress = "FcLfoPNCYjSMnxLPiQJQFGTV15ecHn3xY4J2HNCrqbCfmca";

    Generator gn = Generator.get("mitum"); // network id: mitum

    Key key = gn.currency().newKey("knW2wVXH399P9Xg8aVjAGuMkk3uTBZwcSpcy4aR3UjiAmpu", 100);
    Keys keys = gn.currency().newKeys(new Key[]{ key }, 100); // becomes single-sig account

    Amount amount = gn.currency().newAmount("MCC", "1000");
    CreateAccountsItem item = gn.currency().newCreateAccountsItem(keys, new Amount[]{ amount });

    CreateAccountsFact fact = gn.currency().newCreateAccountsFact(sourceAddr, new CreateAccountsItem[]{ item });

    Operation createAccount = gn.newOperation(fact);
    createAccount.addSign(senderPrivateKey);

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

.. code-block:: java

    /*
    import org.mitumc.sdk.key.*;
    import org.mitumc.sdk.Generator;
    import org.mitumc.sdk.operation.Operation;
    import org.mitumc.sdk.operation.currency.*;
    */

    Generator gn = Generator.get("mitum"); // network id: mitum

    String targetPrivateKey = "KzejtzpPZFdLUXo2hHouamwLoYoPtoffKo5zwoJXsBakKzSvTdbzmpr";
    String targetAddress = "JDhSSB3CpRjwM8aF2XX23nTpauv9fLhxTjWsQRm9cJ7umca";

    Key key1 = gn.currency().newKey("22ndFZw57ax28ydC3ZxzLJMNX9oMSqAfgauyWhC17pxDpmpu", 50);
    Key key2 = gn.currency().newKey("22wD5RWsRFAr8mHkYmmyUDzKf6VBNgjHcgc3YhKxCvrZDmpu", 50);
    Keys newKeys = gn.currency().newKeys(new Key[]{ key1, key2 }, 100);

    KeyUpdaterFact fact = gn.currency().newKeyUpdaterFact(sourceAddr, "MCC", newKeys); // newKeyUpdaterFact(target address, currency for fee, new keys)
    Operation keyUpdater = gn.newOperation(fact);
    keyUpdater.addSign(targetPrivateKey);

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

.. code-block:: java

    /*
    import org.mitumc.sdk.Generator;
    import org.mitumc.sdk.operation.Operation;
    import org.mitumc.sdk.operation.currency.*;
    */
    Generator gn = Generator.get("mitum"); // network id: mitum

    String senderPrivateKey = "KzdeJMr8e2fbquuZwr9SEd9e1ZWGmZEj96NuAwHnz7jnfJ7FqHQBmpr";
    String senderAddress = "2D5vAb2X3Rs6ZKPjVsK6UHcnGxGfUuXDR1ED1hcvUHqsmca";
    String receiverAddress = "CY1pkxsqQK6XMbnK4ssDNbDR2K7mitSwdS27DwBjd3Gcmca";

    Amount amount1 = currencyGenerator.amount("1000000", "MCC")
    Amount amount2 = currencyGenerator.amount("15000", "PEN")

    TransfersItem item = gn.currency().newTransfersItem(receiverAddress, new Amount[]{ amount1, amount2 }); // newTransfersItem(receiver address, amount list)
    TransfersFact fact = gn.currency().newTransfersFact(sourceAddr, new TransfersItem[]{ item }); // newTransfersFact(sender address, item list)

    Operation transfer = gn.newOperation(fact);
    transfer.addSign(senderPrivateKey); // suppose sender is single-sig  

| There are other operations that **mitum-java-util** supports, like operations of *Mitum Blocksign*, but this document doesn't provide examples of those operations.
| Refer to `README <https://github.com/ProtoconNet/mitum-java-util/blob/main/README.md>`_ if necessary.

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
* Operation as JsonObject, or external JSON file.
* Network ID

| First, create ``Signer`` with ``network id`` like ``Generator``.

.. code-block:: java

    /*
    import org.mitumc.sdk.Signer;
    import org.mitumc.sdk.JSONParser;
    */
    String id = "mitum";
    String key = "KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr";

    Signer signer = Signer.get(id, key);

| Then, sign now!

.. code-block:: java

    HashMap<String, Object> signed = signer.addSignToOperation("operation.json"); // or JsonObject from Operation JSON instead

| Note that the result operation is not ``Operation`` object of **mitum-java-util**. It's just a HashMap object.
| If you want to add multiple signature at once, you must create JsonObject from HashMap then re-sign it with other private keys using ``Signer``.

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

| **mitum-java-util** will create random keypair for you!

| Use ``Keypar.create()``.

.. code-block:: java

    /*
    import org.mitumc.sdk.key.Keypair;
    */
    Keypair kp = Keypair.create();

    kp.getPrivateKey(); // returns private key of the keypair
    kp.getPublicKey(); // returns public key of the keypair

Get Keypair From Your Private Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| If you already have own private key, create keypair with it!

.. code-block:: java

    /*
    import org.mitumc.sdk.key.Keypair;
    */
    String key = "KzafpyGojcN44yme25UMGvZvKWdMuFv1SwEhsZn8iF8szUz16jskmpr";
    Keypair pkp = Keypair.fromPrivateKey(key);

Get Keypair From Your Seed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| You can get keypair from your seed, too. Even if you don't remeber the private key of the keypair, the keypair can be recovered by it's seed.
| Note that string seed length >= 36.

.. code-block:: java

    /*
    import org.mitumc.sdk.key.Keypair;
    */
    String seed =  "Thisisaseedfortheexample;Keypair.fromSeed()";
    Keypair skp = Keypair.fromSeed(seed);

    // or... -----------------------------//
    // byte[] bseed = seed.getBytes();
    // Keypair skp = Keypair.fromSeed(bseed);

Get Account Address with Keys
'''''''''''''''''''''''''''''''''''''''''''''''''''

| You can calculate address from threshold, and every (public key, weight) pair of the account.

| However, it is not available to get address if keys or threshold of the account have changed.
| This method is available only for the account that have not changed yet.

| The account information for the example is,

* key1: (vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu, 40)
* key2: (29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu, 30)
* key3: (uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu, 30)
* threshold: 100

.. code-block:: java

    /*
    import org.mitumc.sdk.Generator
    import org.mitumc.key.Key
    import org.mitumc.key.Keys
    */
    Generator generator = Generator.get("mitum");

    Key key1 = generator.currency().newKey("vmk1iprMrs8V1NkA9DsSL3XQNnUW9SmFL5RCVJC24oFYmpu", 40);
    Key key2 = generator.currency().newKey("29BQ8gcVfJd5hPZCKj335WSe4cyDe7TGrjam7fTrkYNunmpu", 30);
    Key key3 = generator.currency().newKey("uJKiGLBeXF3BdaDMzKSqJ4g7L5kAukJJtW3uuMaP1NLumpu", 30);

    Keys keys = generator.currency().newKeys(100); // newKeys(threshold)
    keys.addKey(key1);
    keys.addKey(key2);
    keys.addKey(key3);
    // or... ------------------------------------------------------//
    // Keys keys = generator.currency().newKeys(new Key[]{ key1, key2, key3 }, 100);

    String address = keys.getAddress(); // This is the goal!

Major Classes
'''''''''''''''''''''''''''''''''''''''''''''''''''

Generator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Generator`` is the class that helps generate operations for Mitum Currency.

| Before you use ``Generator``, ``network id`` must be set.

* For **Mitum Currency**, use ``Generator.currency()``.
* For **Mitum Blocksign**, use ``Generator.blockSign()``.

| For details of generating operations for **Mitum Blocksign**. refer to `README <https://github.com/ProtoconNet/mitum-java-util/blob/main/README.md>`_.

.. code-block:: java

    /*
    import org.mitumc.sdk.Generator;
    */
    String id = "mitum";
    Generator generator = Generator.get(id);

    CurrencyGenerator cgn = generator.currency(); // org.mitumc.sdk.operation.currency.CurrencyGenerator;
    BlockSignGenerator bgn = generator.blockSign(); // org.mitumc.sdk.operation.blocksign.BlockSignGenerator;

| All methods of ``Generator`` provides are,

.. code-block:: java

    /* For Mitum Currency */
    Generator.currency().newKey(String key, int weight);
    Generator.currency().newKeys(int threshold);
    Generator.currency().newKeys(Key[] keys, int threshold); 
    Generator.currency().newAmount(String currency, String amount);
    Generator.currency().newCreateAccountsItem(Keys keys, Amount[] amounts);
    Generator.currency().newTransfersItem(String receiver, Amount[] amounts);
    Generator.currency().newCreateAccountsFact(String sender);
    Generator.currency().newCreateAccountsFact(String sender, CreateAccountsItem[] items);
    Generator.currency().newKeyUpdaterFact(String target, String currencyId, Keys keys);
    Generator.currency().newTransfersFact(String sender);
    Generator.currency().newTransfersFact(String sender, TransfersItem[] items);   

    /* For Mitum Blocksign */
    Generator.blockSign().newCreateDocumentsItem(String fileHash, int documentId, String signcode, String title, int size, String currencyId, String[] signers, String[] signcodes);
    Generator.blockSign().newSignDocumentsItem(String owner, int documentId, String currencyId);
    Generator.blockSign().newTransferDocumentsItem(String owner, String receiver, int documentId, String currencyId);
    Generator.blockSign().newBlockSignFact(String sender, CreateDocumentsItem[] items);
    Generator.blockSign().newBlockSignFact(String sender, SignDocumentsItem[] items);
    Generator.blockSign().newBlockSignFact(String sender, TransferDocumentsItem[] items);

    /* Common */
    Generator.newOperation(OperationFact fact);
    Generator.newOperation(String memo, OperationFact fact);
    Generator.newSeal(String signKey, Operation[] operations);

Signer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| ``Signer`` is the class for adding new fact signature to already create operations.

| Like ``Generator``, ``network id`` must be set.

| You have to prepare *private key* to sign, too.

| ``Signer`` provides only one method, that is,

.. code-block:: java

    HashMap<String, Object> addSignToOperation(JsonObject operation);
    HashMap<String, Object> addSignToOperation(String operationPath);

| To check the exact usage of ``Signer``, go back to **Make Your First Operation - Sign**.

JSONParser
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| This class is constructed just for convenience.
| If you would like to use other java package to export ``Operation`` to file or to print it in JSON format, you don't need to use ``JSONParser`` of **mitum-java-util**.

.. code-block:: java

    /*
    import org.mitumc.sdk.JSONParser;
    */
    // ... omitted
    // ... create operations
    // ... refer to above `Make Your First Operation`
    // ... suppose you have already made operations - createAccount, keyUpdater, transfer and a seal - seal

    JSONParser.createJSON(createAccount.toDict(), 'createAccount.json'); // createJSON(HashMap, filePath)
    JSONParser.createJSON(keyUpdater.toDict(), 'keyUpdater.json');
    JSONParser.createJSON(transfer.toDict(), 'transfer.json');
    JSONParser.createJSON(seal, 'seal.json');