.. _CLI:

===================================================
Command Line Interface
===================================================

| In this part, we will introduce the commands supported by Mitum and how to utilize them.

| There are seven major commands it supports,

* ``version``
* ``node``
* ``key``
* ``seal``
* ``storage``
* ``deploy``
* ``quic-client``

| You may be familiar with ``node`` command if you have already been to :ref:`Run` page.

| The operation of each model can be generated using the ``seal`` command.

| It is easy to use the ``version`` and ``quic-client`` commands. We will now explain the rest of the commands one by one.

---------------------------------------------------
Summary
---------------------------------------------------

| The following commands are only available in models that contain operations of **Mitum Currency**, and other commands are commonly used in all models.

* ``seal create-account``
* ``seal transfer``
* ``seal key-updater``
* ``seal currency-register``
* ``seal currency-policy-updater``
* ``seal suffrage-inflation``

| As an example, these are all the commands that **Mitum Currency** provides.

.. code-block:: shell

    $ ./mitum-currency --help

    Usage: mitum-currency <command>

    mitum-currency tool

    Flags:
        -h, --help    Show context-sensitive help.

    Commands:
        version                         version

        node                                node
            init                            initialize node
                <node design file>          node design file
            run                             run node
                <node design file>          node design file
            info                            node information
                <node url>                  remote mitum url
            start-handover                  start handover
                <node address>
                <private key of node>
                <network-id>
                <new node url>              new node url

        key                                 key
            new                             new keypair
            verify                          verify key
                <key>                       key
            address                         generate address from key
                [<threshold>]               threshold for keys (default: 100)
                [<key> ...]                 key for address (ex: "<public key>,<weight>")
            sign                            signature signing
                <privatekey>                privatekey
                <signature base>            signature base for signing

        seal                                seal
            send                            send seal to remote mitum node
                <privatekey>                privatekey for sign
            create-account                  create new account
                <privatekey>                privatekey to sign operation
                <sender>                    sender address
                <currency-amount> ...       amount (ex: "<currency>,<amount>")
            transfer                        transfer big
                <privatekey>                privatekey to sign operation
                <sender>                    sender address
                <receiver>                  receiver address
                <currency-amount> ...       amount (ex: "<currency>,<amount>")
            key-updater                     update keys
                <privatekey>                privatekey to sign operation
                <target>                    target address
                <currency>                  currency id
            currency-register               register new currency
                <privatekey>                privatekey to sign operation
                <currency-id>               currency id
                <genesis-amount>            genesis amount
                <genesis-account>           genesis-account address for genesis balance
            currency-policy-updater         update currency policy
                <privatekey>                privatekey to sign operation
                <currency-id>               currency id
            suffrage-inflation              suffrage inflation operation
                <privatekey>                privatekey to sign operation
                <inflation item> ...        ex: "<receiver address>,<currency>,<amount>"
            sign                            sign seal
                <privatekey>                sender's privatekey
            sign-fact                       sign facts of operation seal
                <privatekey>                sender's privatekey

        storage                             storage
            download                        download block data
                <data type>                 data type of block data,
                                            {manifest,operations,operations_tree,states,states_tree,init_voteproof,accept_voteproof,suffrage_info,proposal all}
                <height> ...                heights of block
            verify-blockdata                verify block data
                <blockdata path>
            verify-database                 verify database
                <database uri>
                <blockdata path>
            clean                           clean storage
                <node design file>          node design file
            clean-by-height                 clean storage by height
                <node design file>          node design file
                <height>                    height of block
            restore                         restore blocks from blockdata
                <node design file>          node design file
            set-blockdatamaps               set blockdatamaps
                <deploy key>
                <maps file>                 set blockdatamap file
                [<node url>]                remote mitum url; default: quic://localhost:54321

        deploy                              deploy
            key                             deploy key
                new                         request new deploy key
                    <private key of node>
                    <network-id>
                    [<node url>]            remote mitum url; default: quic://localhost:54321
            keys                            deploy keys
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321
            key                             deploy key
                <deploy key>
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321
            revoke                          revoke deploy key
                <deploy key>
                <private key of node>
                <network-id>
                [<node url>]                remote mitum url; default: quic://localhost:54321

        quic-client                         quic-client
            <node url>                      remote mitum url

    Run "mitum-currency <command> --help" for more information on a command.