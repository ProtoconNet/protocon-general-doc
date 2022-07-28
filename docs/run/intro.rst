===================================================
Get Started
===================================================

| In this part, it will introduce how to run nodes on **Mitum Currency**.
| For using Mitum Currency, you need to install *docker* and *golang* first.

| If you are looking for the usage of **Mitum Blocksign**, visit `mitum-data-blocksign <https://github.com/ProtoconNet/mitum-data-blocksign>`_.

---------------------------------------------------
About Mitum Currency Node
---------------------------------------------------

| **Mitum Blockchain** network running Mitum Currency uses **PBFT-based ISAAC+ consensus protocol**.
| In the *ISAAC+* consensus protocol, all nodes play the same role and participate in block generation.

| Nodes participating in the network perform the following tasks.

* Making Proposal
* Block Verification
* Voting
* Storing Block
* Providing Digest API Service
* Transaction Requesting Collection

| For more information on the **Mitum Blockchain** network, refer to `Mitum Document <https://mitum-doc.readthedocs.io/en/proto2/>`_.

---------------------------------------------------
Prerequisite
---------------------------------------------------

Database
'''''''''''''''''''''''''''''''''''''''''''''''''''

| **Mitum Currency** uses **MongoDB** as its main storage engine.

| To run the Mitum currency node, you need to prepare mongodb first.

Installation and Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* `Manual Installation Guide <https://docs.mongodb.com/manual/installation/>`_

* Using `Docker Container <https://hub.docker.com/_/mongo>`_,

.. code-block:: shell

    $ docker run --name <db name> -it -p <host port>:<container port> -d mongo

* About DB setup, refer to :ref:`config`.

Golang
'''''''''''''''''''''''''''''''''''''''''''''''''''

| **Mitum Currency** is developed using the programming language `Go <https://golang.org>`_.

| To create an executable binary, you need the source code to be built from.
| We do not provide detailed instructions for installing the Go language here.
| You must have the Golang installed with at least version 1.16 to build Mitum currency.

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
    
    $ go build -ldflags="-X 'main.Version=v0.0.1-tutorial'" -o ./mc ./main.go
    
    $ ./mc version
    v0.0.1

| To see all instructions of Mitum Currency, refer to :ref:`cli`.
