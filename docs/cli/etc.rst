===================================================
Others
===================================================

* ``version``
* ``quic-client``

---------------------------------------------------
version
---------------------------------------------------

| Check the version of the installed Mitum Currency using the ``version`` command.

.. code-block:: shell

    $ ./mc version

| **EXAMPLE**

.. code-block:: shell

    $ ./mc version
    v0.0.1

---------------------------------------------------
quic-client
---------------------------------------------------

| Note that the response of ``quic-client`` is identical to the response when requesting *node info* by API.

.. code-block:: shell

    $ ./mc quic-client <node-url>

| **EXAMPLE**

.. code-block:: shell

    $ ./mc quic-client https://3.35.171.179:54321/
    {
        "_hint": "node-info-v0.0.1",
        "node": {
            "_hint": "base-node-v0.0.1",
            "address": "node4sas",
            "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu"
        },
        "network_id": "bWl0dW0=",
        "state": "CONSENSUS",
        "last_block": {
            "_hint": "block-manifest-v0.0.1",
            "hash": "GBQqKbR6pAs8gWzNmf5mrHGUYUmjs829NVX4WuYz7uzf",
            "height": 994024,
            "round": 0,
            "proposal": "HbxL38mNX8NGTqErNE3Hw5w639qKpbEwC4SkkCDZvrYB",
            "previous_block": "5rPQHEunbAw15YG3GaZneYKQpxsKRgQuThW6Yd7KBZb",
            "block_operations": null,
            "block_states": null,
            "confirmed_at": "2022-01-19T05:58:14.623577286Z",
            "created_at": "2022-01-19T05:58:14.631963244Z"
        },
        "version": "v0.0.1-stable-383cf0c-20211224",
        "policy": {
            "timespan_valid_ballot": 60000000000,
            "network_connection_timeout": 3000000000,
            "threshold": 100,
            "max_operations_in_seal": 10,
            "max_operations_in_proposal": 100,
            "interval_broadcasting_proposal": 1000000000,
            "wait_broadcasting_accept_ballot": 1000000000,
            "timeout_waiting_proposal": 5000000000,
            "interval_broadcasting_init_ballot": 1000000000,
            "interval_broadcasting_accept_ballot": 1000000000,
            "suffrage": "{\"type\":\"\",\"cache_size\":10,\"number_of_acting\":1}"
        },
        "suffrage": [
            {
                "address": "node4sas",
                "publickey": "21im86HvT3aC4p23AExN7PKRD3RF1GR8cD3E95iEJHhNKmpu",
                "conninfo": {
                    "_hint": "http-conninfo-v0.0.1",
                    "url": "https://3.35.171.179:54321",
                    "insecure": true
                }
            }
        ],
        "conninfo": {
            "_hint": "http-conninfo-v0.0.1",
            "url": "https://3.35.171.179:54321",
            "insecure": true
        }
    }
