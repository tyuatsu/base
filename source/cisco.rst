Router Cisco
------------

.. note:: Checagem da interface Wan - Análise de perda de pacotes, incremento de erro ou quedas recentes.

.. figure:: ciscologo.jpg
    :scale: 40 %
    :align: center
    :alt: Cisco

1. Comandos Básicos Router Cisco
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Router IP: 192.168.131.108 x 192.172.100.102::

    TESTRO0075# clear counter int GigabitEhternet0/1 
    
    TESTRO0075# ping 192.172.100.102 size 1500 repeat 1000 df-bit (escape sequence to abort: 'press ctrl + shift + 6' simultainiously)

    Type escape sequence to abort.
    Sending 1000, 1500-byte ICMP Echos to 192.172.100.102, timeout is 2 seconds:
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    Success rate is 100 percent (2162/2268), round-trip min/avg/max = 40/58/224 ms
    
    TESTRO0075# show int description
    TESTRO0075# show ip int brief
    TESTRO0075# show int GigabitEhternet0/1
    TESTRO0075# show int GigabitEhternet0/1 | in up | drops
    TESTRO0075# show int summ
    TESTRO0075# show run int GigabitEthernet0/1
    TESTRO0075# show int | i CRC
    TESTRO0075# show ver | i uptime
    TESTRO0001# show clock

Acesso ao equipamento TESTRO0001 com incremento de erro na interface operadora.

Router IP: 192.172.100.101 x 192.172.100.102::

    TESTRO0001# clear counter int FastEthernet0/1

    TESTRO0001# ping 192.172.100.102 size 1500 repeat 1000 df-bit (escape sequence to abort: 'press ctrl + shift + 6' simultainiously)

    Type escape sequence to abort.
    Sending 1000, 1500-byte ICMP Echos to 192.172.100.102, timeout is 2 seconds:
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    Success rate is 100 percent (2162/2268), round-trip min/avg/max = 40/58/224 ms    
    
    TESTRO0001# show int description
    TESTRO0001# show ip int brief
    TESTRO0001# show int FastEthernet0/1
    TESTRO0001# show int FastEthernet0/1 | in up | drops
    TESTRO0001# show int summ
    TESTRO0001# show run int FastEthernet0/1
    TESTRO0001# show int | i CRC
    TESTRO0001# show ver | i uptime
    TESTRO0001# show clock
    
