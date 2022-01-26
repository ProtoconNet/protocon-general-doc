.. _deploy command:

===================================================
Deploy Command
===================================================

| Execute ``deploy key`` command to create and manage the node’s deploy key.

| The subcommands related to ``deploy key`` command are as follows.

* ``new``
* ``keys``
* ``key``
* ``revoke``

.. note::

    **What is deploy key?**

    Updates of nodes (such as changing the BlockDataMap) should be allowed only by the node owner.
    The node owner uses the key to prove himself when managing the node.
   
    However, it is dangerous to directly use a node’s private key for node management.
    So we need a **replaceable and manageable key that can be used for things like node management**.
    
    ``deploy key`` is used for this purpose.

---------------------------------------------------
new
---------------------------------------------------

| By ``new`` command, create and register new deploy key to the node.

.. code-block:: shell

    $ ./mc deploy key new <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mc deploy key new $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}
    2021-06-09T09:31:22.320055Z INF new deploy key module=command-deploy-key-new

---------------------------------------------------
keys
---------------------------------------------------

| By ``keys`` command,  get the list of registered deploy keys in the node.

.. code-block:: shell

    $ ./mc deploy key keys <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ ./mc deploy key keys $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    [{"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"},{"key":"d-2897ced4-ceb5-4e11-be81-3139350c9c55","added_at":"2021-06-09T03:56:49.393Z"},{"key":"d-fc4179e7-2ff3-4372-bd83-f70526bed476","added_at":"2021-06-09T09:31:22.321675852Z"}]

---------------------------------------------------
key
---------------------------------------------------

| By ``key`` command, check the existence of deploy key in the node.

.. code-block:: shell

    $ ./mc deploy key key <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr

    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mc deploy key key $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    {"key":"d-974702df-89a7-4fd1-a742-2d66c1ead6cd","added_at":"2021-06-09T03:14:33.9Z"}

---------------------------------------------------
revoke
---------------------------------------------------

| By ``revoke`` command, revoke deploy key from the node.

.. code-block:: shell

    $ ./mc deploy key revoke <deploy key> <private key of node> <network-id> [<node url>]

| **EXAMPLE**

.. code-block:: shell

    $ NODE_PRV_KEY=KxaTHDAQnmFeWWik5MqWXBYkhvp5EpWbsZzXeHDdTDb5NE1dVw8wmpr
    
    $ NODE=https://127.0.0.1:54321
    
    $ NETWORK_ID=mitum
    
    $ DEPLOY_KEY=d-974702df-89a7-4fd1-a742-2d66c1ead6cd
    
    $ ./mc deploy key revoke $DEPLOY_KEY $NODE_PRV_KEY $NETWORK_ID $NODE --tls-insecure
    2021-06-09T09:36:19.763339Z INF deploy key revoked deploy_key=d-974702df-89a7-4fd1-a742-2d66c1ead6cd module=command-deploy-key-revoke