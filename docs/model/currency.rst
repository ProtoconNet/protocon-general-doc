.. _currency:

===================================================
Mitum Currency
===================================================

---------------------------------------------------
What is Mitum Currency
---------------------------------------------------

* **Mitum Currency** is a currency model that operates on the **Mitum** blockchain networks.
* Mitum Currency allows you to register or update policies for a particular currency id, issue additional tokens, send tokens, create new currency accounts, and update account keys.

---------------------------------------------------
Features of Mitum Currency
---------------------------------------------------

* Mitum Currency provides core features to meet the business needs of various token-related fields.
* *Multiple keys* can be registered when creating an account, and related keys can be replaced through the key update operation.
* Mitum Currency can issue new currencies and related policies can be customized.
* Currency-related policy can be updated at any time as needed.
* A node key, not an account key, is required to adjust the policy for any current id.
* Account keys must be used to transfer tokens or update account keys.
* Mitum Currency has no compensation for block generation and there is also no inflation.
* The node configuration for the Mitum Currency network follows the node operation policy of the Mitum blockchain, and details can be found at :ref:`build network`.

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

| Refer to :ref:`Currency CLIs` to check how to create those operations by commands.
