===================================================
Quick Start
===================================================

| In this part, we will introduce how to run a node.
| You need to install *docker* and *golang* first.

| For smooth explanation, **Mitum Currency** is used as an example in this chapter.

---------------------------------------------------
About Mitum Currency Node
---------------------------------------------------

| **Mitum Blockchain** network uses **PBFT-based ISAAC+ consensus protocol**.
| In the *ISAAC+* consensus protocol, all nodes play the same role and participate in block generation.

| Nodes participating in the network perform the following tasks.

* Making Proposal
* Block Verification
* Voting
* Storing Block
* Providing Digest API Service
* Transaction Requesting Collection

| For more information on the **Mitum Blockchain** network, refer to `Mitum Doc <https://mitum-doc.readthedocs.io/en/proto2/>`_.

---------------------------------------------------
Prerequisite
---------------------------------------------------

Database
'''''''''''''''''''''''''''''''''''''''''''''''''''

| **Mitum** uses **MongoDB** as its main storage engine.

| To run the node, you need to prepare mongodb first.

Installation and Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* `Manual Installation Guide <https://docs.mongodb.com/manual/installation/>`_

* Using `Docker Container <https://hub.docker.com/_/mongo>`_,

.. code-block:: shell

    $ docker run --name <db name> -it -p <host port>:<container port> -d mongo

* About DB setup, refer to :ref:`config`.

Golang
'''''''''''''''''''''''''''''''''''''''''''''''''''

| **Mitum** and **Mitum Models** are developed using the programming language `Go <https://golang.org>`_.

| To create an executable binary, you need the source code to be built from.
| We do not provide detailed instructions for installing the Go language here.
| You must have the Golang installed with at least version 1.17 to build Mitum.

| For more information, refer to `How to Install Go <https://go.dev/doc/install>`_.

---------------------------------------------------
Installation
---------------------------------------------------

* Please download the source code of `Mitum Currency <https://github.com/spikeekips/mitum-currency>`_.

* Using **Git**,

.. code-block:: shell

    $ git clone https://github.com/spikeekips/mitum-currency.git

* Build exe file.

.. code-block:: shell

    $ cd mitum-currency
    
    $ go build -ldflags="-X 'main.Version=v0.0.1-tutorial'" -o mitum ./main.go
    
    $ ./mitum version
    v0.0.1

* The installation method is the same for other models.

| To see all instructions of Mitum and its models, refer to :ref:`CLI`.
