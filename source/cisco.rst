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

Preste muita atenção ao entrar no modo de configuração::

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

3. Comandos Básicos Switch Cisco ME-C3750-24TE (PowerPC405)  
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: O switch Cisco ME-C3750-24TE será nosso SW Core e os SW de acesso será o Nortel Routing Switch 4550T-PWR.

Favor realizar identificação da porta do switch que está ligado a estação (MAC: 00-1b-4f-75-39-82), porém essa máquina não pega IP. 
Se possível, favor alterar para vlan 133::

    KINGTUT-01> show mac-address-table address 00-1b-4f-75-39-82

              Mac Address Table
    -------------------------------------------
    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
      29    001b.4f75.3982    DYNAMIC     Fa1/0/18
    Total Mac Addresses for this criterion: 1

    KINGTUT-01> show interfaces Fa1/0/18

    FastEthernet1/0/18 is up, line protocol is up (connected)
      Hardware is Fast Ethernet, address is ec44.7630.fe94 (bia ec44.7630.fe94)
      Description: The 13th floor SW 192.168.133.4
      MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
         reliability 255/255, txload 6/255, rxload 2/255
      Encapsulation ARPA, loopback not set
      Keepalive set (10 sec)
      Full-duplex, 100Mb/s, media type is 10/100BaseTX
      input flow-control is off, output flow-control is unsupported
      ARP type: ARPA, ARP Timeout 04:00:00
      Last input 00:00:09, output 00:00:00, output hang never
      Last clearing of "show interface" counters 29w6d
      Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
      Queueing strategy: fifo
      Output queue: 0/40 (size/max)
      5 minute input rate 971000 bits/sec, 504 packets/sec
      5 minute output rate 2366000 bits/sec, 662 packets/sec
         6463274408 packets input, 1896886174888 bytes, 0 no buffer
         Received 15512997 broadcasts (0 multicasts)
         0 runts, 0 giants, 0 throttles
         0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
         0 watchdog, 10463928 multicast, 0 pause input
         0 input packets with dribble condition detected
         9994720872 packets output, 6110735013726 bytes, 0 underruns
         0 output errors, 0 collisions, 0 interface resets
         0 babbles, 0 late collision, 0 deferred
         0 lost carrier, 0 no carrier, 0 PAUSE output
         0 output buffer failures, 0 output buffers swapped out

    KINGTUT-01# show run int Fa1/0/18

    Building configuration...
    Current configuration : 269 bytes
    !
    interface FastEthernet1/0/18
    description 13th floor SW 192.168.133.4
    switchport trunk encapsulation dot1q
    switchport trunk native vlan 133
    switchport trunk allowed vlan 23-29,45,47,133,134
    switchport mode trunk
    switchport voice vlan 29
    spanning-tree portfast
    end

Acessar o switch de acesso 192.168.133.4 e verificar o MAC **00-1b-4f-75-39-82**::

    TimeCrystal# show mac-address-table address 00:1b:4f:75:39:82
    Mac Address Table Aging Time: 300
    Learning Enabled Ports ALL
    Number of addresses: 1

       MAC Address    Vid   Type       Source
    ----------------- ---- ------- --------------
    00-1B-4F-75-39-82   29 Dynamic Port:33

    TimeCrystal# show vlan interface vid 33
    Port VLAN VLAN Name         VLAN VLAN Name         VLAN VLAN Name
    ---- ---- ----------------  ---- ----------------  ---- ----------------
    33   23   VLAN23-DADOS      24   VLAN24-DADOS      25   VLAN25-VOZ
         26   VLAN26-VOZ        27   VLAN27-VOZ        28   VLAN28-DADOS
         29   VLAN-29-Voz       45   VLAN45-DADOS      47   VLAN47-VOZ
         133  VLAN-133-Dados    134  VLAN134-DADOS
    ---- ---- ----------------  ---- ----------------  ---- ----------------

Agora vamos apenas alterar para Vlan 133::

    TimeCrystal(config)# vlan ports 33 pvid 133
    TimeCrystal(config)# save conf
    TimeCrystal(config)# exit

    TimeCrystal# show vlan interface info 33
      Filter     Filter
     Untagged Unregistered
    Port  Frames     Frames    PVID PRI    Tagging    Name
    ---- -------- ------------ ---- --- ------------- ----------------
    33   No       Yes          133  0   UntagPvidOnly Port 33
    

