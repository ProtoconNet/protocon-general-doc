.. _currency extension:

===================================================
Mitum Currency Extension
===================================================

---------------------------------------------------
What is Mitum Currency Extension
---------------------------------------------------

* **Mitum Currency extension** is an extended model of **Mitum Currency** as its name suggests.
* Mitum Currency Extension supports the generation of contract accounts and the withdrawal of tokens from contract accounts in addition to the function of Mitum Currency.

---------------------------------------------------
Features of Mitum Currency Extension
---------------------------------------------------

* You can create a contract account, rather than a typical currency account, using **Mitum Currency Extension**.
* A contract account has an address, but it does not have account keys unlike regular accounts.
* Contract account cannot be an operation sender.
* Typically, a contract account cannot own or transfer tokens on its own, but the contract account owner can withdraw tokens from the contract account through withdrawal operation.
* Model designers can develop models that can register different user-defined states with contract accounts.

---------------------------------------------------
Support Operations
---------------------------------------------------

* **Mitum Currency Extension** contains operations of the **Mitum Currency** model.

+-----------------------------------------+-----------------------------------------+
| Operations for Contract Account                                                   |
+=========================================+=========================================+
| create-contract-account                 | Create new contract account             | 
+-----------------------------------------+-----------------------------------------+
| withdraw                                | Withdraw tokens from contract account   | 
+-----------------------------------------+-----------------------------------------+

| Refer to :ref:`Currency Extension CLIs` to check how to create those operations by commands.
