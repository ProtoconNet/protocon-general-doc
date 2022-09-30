.. _document:

===================================================
Mitum Document
===================================================

---------------------------------------------------
What is Mitum Document
---------------------------------------------------

* **Mitum Document** is a tool that allows you to create, update, and sign documents.
* Mitum Document allows you to register various document items (blocksign, blockcity document, etc.), but here we describe them based on the most commonly used BlockSign model.

---------------------------------------------------
Features of Mitum Document
---------------------------------------------------

* **Mitum Document** corresponds to any form of document.
* You can register the document's title, file hash, file size, signer, etc. together.
* The document creator can update the document.
* An account registered as a signer can sign the generated document as a document signer.
* Depending on the characteristics of the blockchain, the update history of the document is permanently stored in the blockchain.

---------------------------------------------------
Document ID
---------------------------------------------------

| There is a document id suffix corresponding to each document type.

| For **blockcity**,

* user doc: ``cui``
* land doc: ``cli``
* voting doc: ``cvi``
* history doc: ``chi``

| For **blocksign**,

* blocksign doc: ``sdi``

---------------------------------------------------
Support Operations
---------------------------------------------------

* **Mitum Document** contains operations of the **Mitum Currency** model.

+-----------------------------------------+-----------------------------------------+
| Operations for Document                                                           |
+=========================================+=========================================+
| create-document                         | Create new document                     | 
+-----------------------------------------+-----------------------------------------+
| update-document                         | Update the registered document          | 
+-----------------------------------------+-----------------------------------------+
| sign-document                           | Sign the registered document            | 
+-----------------------------------------+-----------------------------------------+

| Refer to :ref:`Document CLIs` to check how to create those operations by commands.
