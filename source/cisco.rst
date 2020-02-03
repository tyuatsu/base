3. Comandos Básicos Router Cisco
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: Checagem da interface Wan - Análise de perda de pacotes, incremento de erro ou quedas recentes.

.. figure:: ciscologo.jpg
    :scale: 60 %
    :align: center
    :alt: Cisco

Router IP: 192.168.131.108 x 192.172.100.102::

    TESTRO0075# clear counter int br
    TESTRO0075# ping 192.172.100.102 repeat 1000 size 1500 **(escape sequence to abort: 'press ctrl + shift + 6' simultainiously)**
    TESTRO0075# show ip int br
    TESTRO0075# show ip int br | in up | drops
    TESTRO0075# show int | i CRC
    TESTRO0075# show ver | i uptime
    TESTRO0001# show clock

Acesso ao equipamento TESTRO0001 com incremento de erro na interface operadora.

Router IP: 192.172.100.101 x 192.172.100.102::

    TESTRO0001# clear counter int FastEthernet0/1
    TESTRO0001# ping 192.172.100.102 repeat 1000 size 1500 **(escape sequence to abort: 'press ctrl + shift + 6' simultainiously)**
    TESTRO0001# show int description
    TESTRO0001# show int FastEthernet0/1
    TESTRO0001# show int FastEthernet0/1 | in up | drops
    TESTRO0001# show int | i CRC
    TESTRO0001# show ver | i uptime
    TESTRO0001# show clock
