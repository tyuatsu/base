Cisco
-----

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
    TESTRO0075# show standby brief
    TESTRO0075# show standby (hscp - hot stanby cisco protocol)
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
    TESTRO0001# show standby brief
    TESTRO0001# show standby
    TESTRO0001# show int FastEthernet0/1
    TESTRO0001# show int FastEthernet0/1 | in up | drops
    TESTRO0001# show int summ
    TESTRO0001# show run int FastEthernet0/1
    TESTRO0001# show int | i CRC
    TESTRO0001# show ver | i uptime
    TESTRO0001# show clock
        
2. Comandos Básicos Switch Cisco SG 300-20
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: A máquina com o mac f8:0f:41:35:17:22 do setor fac-símiles, não se encontra na vlan abacaxi. Portanto será necessário mudar para a vlan correta que é 10.221.60.0/24 (vlan id 17).

Sabendo quais os dados da vlan correta através do switch core, acesse o sw de acesso::

    MARSUPIAL-SW06# show vlan

    Vlan       Name                   Ports                Type     Authorization
    ---- ----------------- --------------------------- ------------ -------------
     1           1                 gi20,Po1-8            Default      Required
     17     CATCHUP-DADOS                gi20              permanent     Required
     58     GERAL-DADOS    gi1-4,gi5,gi7-11,gi13-15,    permanent     Required
                           gi17-20
     60    ABACAXI-DADOS        gi6,gi12,gi16,gi20       permanent     Required
    144      CATCHUP-VOZ                 gi20              permanent     Required
    561     GERENCIA-SW               gi20              permanent     Required

    MARSUPIAL-SW06# show int conf gi4
                                               Flow    Admin     Back   Mdix
    Port     Type         Duplex  Speed  Neg      control  State   Pressure Mode
    -------- ------------ ------  -----  -------- -------  -----   -------- ----
    gi4      1G-Copper    Full    1000   Enabled  Off      Up      Disabled Auto

Prete muita atenção ao entrar no modo de configuração::

    MARSUPIAL-SW06# conf t
    MARSUPIAL-SW06(config)# int gi4
    MARSUPIAL-SW06(config-if)# switchport mode access
    MARSUPIAL-SW06(config-if)# switchport access vlan 17
    MARSUPIAL-SW06(config-if)# no shut
    MARSUPIAL-SW06(config-if)# end

Vamos verificar as alterações e coletar as evidências::

    MARSUPIAL-SW06# show vlan

    Vlan       Name                   Ports                Type     Authorization
    ---- ----------------- --------------------------- ------------ -------------
     1           1                 gi20,Po1-8            Default      Required
     17     CATCHUP-DADOS           gi4,gi20            permanent     Required
     58     GERAL-DADOS    gi1-3,gi5,gi7-11,gi13-15,    permanent     Required
                           gi17-20
     60     ABACAXI-DADOS        gi6,gi12,gi16,gi20     permanent     Required
    144      CATCHUP-VOZ               gi20             permanent     Required
    561     GERENCIA-SW                gi20             permanent     Required

    MARSUPIAL-SW06# show int switchport gi4

    Port : gi4
    Port Mode: Access
    Gvrp Status: disabled
    Ingress Filtering: true
    Acceptable Frame Type: admitAll
    Ingress UnTagged VLAN ( NATIVE ): 17
    Port is member in:
    Vlan               Name               Egress rule Port Membership Type
    ---- -------------------------------- ----------- --------------------
     17             CATCHUP-DADOS             Untagged          Static
   
    MARSUPIAL-SW06# show run int Gi4

     interface gigabitethernet4
     storm-control broadcast enable
     storm-control broadcast level kbps 20
     storm-control include-multicast unknown-unicast
     spanning-tree bpduguard enable
     switchport mode access
     switchport access vlan 17
 
    MARSUPIAL-SW06# show int desc

    Port      Description
    -------   -----------
    gi1
    gi2
    gi3
    gi4
    gi5
    gi6
    gi7
    gi8
    gi9
    gi10
    gi11
    gi12
    gi13
    gi14
    gi15
    gi16
    gi17
    gi18
    gi19
    gi20

    Ch        Description
    -------   -----------
    Po1
    Po2
    Po3
    Po4
    Po5
    Po6
    Po7
    Po8

