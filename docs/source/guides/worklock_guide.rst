.. _worklock-guide:

==============
WorkLock Guide
==============

Overview
--------

:ref:`worklock-architecture` is the distribution mechanism for the NuCypher token.


WorkLock CLI
------------

The ``nucypher worklock`` CLI command provides the ability to participate in WorkLock. To better understand the
commands and their options, use the ``--help`` option.

All ``nucypher worklock`` commands share a similar structure:

.. code::

    (nucypher)$ nucypher worklock <COMMAND> [OPTIONS] --network <NETWORK> --provider <YOUR PROVIDER URI>


Replace ``<YOUR PROVIDER URI>`` with a valid node web3 node provider string, for example:

    - ``ipc:///home/<username>/.ethereum/geth.ipc`` - IPC Socket-based JSON-RPC server
    - ``https://<host>`` - HTTP(S)-based JSON-RPC server
    - ``wss://<host>:8080`` - Websocket(Secure)-based JSON-RPC server

Show current WorkLock information
---------------------------------

You can obtain information about the current state of WorkLock by running:

.. code::

    (nucypher)$ nucypher worklock status --network <NETWORK> --provider <YOUR PROVIDER URI>


The following is an example output of the ``status`` command (hypothetical values):

.. code::

     _    _               _     _                   _
    | |  | |             | |   | |                 | |
    | |  | |  ___   _ __ | | __| |      ___    ___ | | __
    | |/\| | / _ \ | '__|| |/ /| |     / _ \  / __|| |/ /
    \  /\  /| (_) || |   |   < | |____| (_) || (__ |   <
     \/  \/  \___/ |_|   |_|\_\\_____/ \___/  \___||_|\_\

    ══ <NETWORK> ══

    Reading Latest Chaindata...

    Time
    ══════════════════════════════════════════════════════

    Escrow (Closed)
    ------------------------------------------------------
    Allocations Available . Yes
    Start Date ............ 2020-03-25 00:00:00+00:00
    End Date .............. 2020-03-31 23:59:59+00:00
    Duration .............. 6 days, 23:59:59
    Time Remaining ........ Closed

    Cancellation (Open)
    ------------------------------------------------------
    End Date .............. 2020-04-01 23:59:59+00:00
    Duration .............. 7 days, 23:59:59
    Time Remaining ........ 1 day, 2:47:32


    Economics
    ══════════════════════════════════════════════════════

    Participation
    ------------------------------------------------------
    Lot Size .............. 280000000 NU
    Min. Allowed Escrow ... 15 ETH
    Participants .......... 1000
    ETH Supply ............ 50000 ETH
    ETH Pool .............. 50000 ETH

    Base (minimum escrow)
    ------------------------------------------------------
    Base Deposit Rate ..... 1000 NU per base ETH

    Bonus (surplus over minimum escrow)
    ------------------------------------------------------
    Bonus ETH Supply ...... 35000 ETH
    Bonus Lot Size ........ 265000000 NU
    Bonus Deposit Rate .... 7571.43 NU per bonus ETH

    Refunds
    ------------------------------------------------------
    Refund Rate Multiple .. 4.00
    Bonus Refund Rate ..... 1892.86 units of work to unlock 1 bonus ETH
    Base Refund Rate ...... 250.0 units of work to unlock 1 base ETH

        * NOTE: bonus ETH is refunded before base ETH


For the less obvious values in the output, here are some definitions:

    - Lot Size
        NU to be allocated by WorkLock
    - ETH Supply
        Sum of all ETH escrows that have been placed
    - ETH Pool
        Current ETH balance of WorkLock that accounts for refunded ETH for work performed i.e. `ETH Supply` - `Refunds for Work`
    - Refund Rate Multiple
        Indicates how quickly your ETH is unlocked relative to the deposit rate e.g. a value of ``4`` means that you get your ETH refunded 4x faster than the rate used when you were allocated NU
    - Base Deposit Rate
        Amount of NU to be allocated per base ETH in WorkLock
    - Bonus ETH Supply
        Sum of all bonus ETH escrows that have been placed i.e. sum of all ETH above minimum escrow
    - Bonus Lot Size
        Amount of NU that is available to be allocated based on the bonus part of escrows
    - Bonus Deposit Rate
        Amount of NU to be allocated per bonus ETH in WorkLock
    - Bonus Refund Rate
        Units of work to unlock 1 bonus ETH
    - Base Refund Rate
        Units of work to unlock 1 base ETH


If you want to see specific information about your current escrow, you can specify your participant address with the ``--participant-address`` flag:

.. code::

    (nucypher)$ nucypher worklock status --participant-address <YOUR PARTICIPANT ADDRESS> --network <NETWORK> --provider <YOUR PROVIDER URI>

The following output is an example of what is included when ``--participant-address`` is used

.. code::

    WorkLock Participant <PARTICIPANT ADDRESS>
    =====================================================
    NU Claimed? .......... No
    Total Escrow ......... 22 ETH
        Base ETH ......... 15 ETH
        Bonus ETH ........ 7 ETH
    NU Allocated ......... 68000 NU

    Completed Work ....... 0
    Available Refund ..... 0 ETH

    Refunded Work ........ 0
    Remaining Work ....... <REMAINING WORK>

Alternatively, when the NU has been allocated, the following is an example of the output

.. code::

    WorkLock Participant <PARTICIPANT ADDRESS>
    =====================================================
    NU Claimed? .......... Yes
    Locked ETH ........... 22 ETH

    Completed Work ....... 0
    Available Refund ..... 0 ETH

    Refunded Work ........ 0
    Remaining Work ....... <REMAINING WORK>

where,

    - Total Escrow
        WorkLock Escrow
    - Base ETH
        Minimum required escrow
    - Bonus ETH
        Surplus over minimum escrow
    - NU Allocated
        Allocation of NU
    - Locked ETH
        Remaining ETH to be unlocked via completion of work
    - NU Claimed
        Whether the allocation of NU tokens has been allocated or not
    - Completed Work
        Work already completed by the participant
    - Available Refund
        ETH portion available to be refunded due to completed work
    - Refunded Work
        Work that has been completed and already refunded
    - Remaining Work
        Pending amount of work required before all of the participant's escrowed ETH will be refunded


Place an escrow
---------------

You can place an escrow to WorkLock by running:

.. code::

    (nucypher)$ nucypher worklock escrow --network <NETWORK> --provider <YOUR PROVIDER URI>


Recall that there's a minimum escrow amount needed to participate in WorkLock.


Cancel an escrow
----------------

You can cancel an escrow to WorkLock by running:

.. code::

    (nucypher)$ nucypher worklock cancel-escrow --network <NETWORK> --provider <YOUR PROVIDER URI>


Claim your stake
----------------

Once the allocation window is open, you can claim your NU as a stake in NuCypher:

.. code::

    (nucypher)$ nucypher worklock claim --network <NETWORK> --provider <YOUR PROVIDER URI>


Once allocated, you can check that the stake was created successfully by running:

.. code::

    (nucypher)$ nucypher status stakers --staking-address <YOUR PARTICIPANT ADDRESS> --network {network} --provider <YOUR PROVIDER URI>
    

Check remaining work
--------------------

If you have a stake created from WorkLock, you can check how much work is pending until you can get all your ETH locked in the WorkLock contract back:

.. code::

    (nucypher)$ nucypher worklock remaining-work --network <NETWORK> --provider <YOUR PROVIDER URI>


Refund locked ETH
-----------------

If you've committed some work, you are able to refund proportional part of ETH you've escrowed in the WorkLock contract:

.. code::

    (nucypher)$ nucypher worklock refund --network <NETWORK> --provider <YOUR PROVIDER URI>
