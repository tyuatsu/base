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

    
3. Troubleshooting - Queda de conexão com a rede voz - Router Switch Cisco C9404R IGMP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Eis o alerta vermelho..: - Prezados do conselho deliberativo da TI, precisamos de máximo apoio com relação a solicitação do cliente: **Abobrinhas Selvagens**. O pedido é: - 'Verificar a rede até a estação do atendente'.

Foi constatado incontáveis erros de ping contra servidores Avaya. (Aprox. às 11h40hs da manhã, houve surtos na comunicação entre cliente e servidor, os quais geraram um conjunto maior de erros de ping. Sendo assim, precisamos que verifiquem especificamente a estação de trabalho cujo IP é 10.0.12.152.
 
Vale lembrar que tal incidente, tem potencial para gerar crise e virar um grande problema: - O erro é relacionado ao login do insólito operador na aplicação do mal  softwarephone of hell, onde o operador loga e após um determinado tempo (em torno de 6 a 8 ligações) ocorre os erros: Logoff inesperado + comunicação com o servidor imediatamente é perdida. A aplicação fecha abruptamente, deslogando o usuário do sistema. Deve-se frisar que isso não ocorre nas outras máquinas da mesma rede. É algo pontual e afeta apenas uma máquina.


Vamos iniciar a coleta das evidências para montar nosso laudo técnico. Estação IP é **10.0.12.152** e Mac **d0-94-66-b1-90-e5**.


Acesso Core::

    CAATINGA01# show mac address-table address d0:94:66:b1:90:e5

              Mac Address Table
    -------------------------------------------
    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
    120    d094.66b1.90e5    DYNAMIC     Po1
 
    Total Mac Addresses for this criterion: 1


    CAATINGA01# sh int Po1

    Port-channel1 is up, line protocol is up (connected)
      Hardware is EtherChannel, address is d4c9.3c81.a30c (bia d4c9.3c81.a30c)
      Description: ESCOLTA_2B_10.255.10.4
      MTU 9198 bytes, BW 2000000 Kbit/sec, DLY 10 usec,
         reliability 255/255, txload 1/255, rxload 1/255
      Encapsulation ARPA, loopback not set
      Keepalive set (10 sec)
      Full-duplex, 1000Mb/s, link type is auto, media type is N/A
      input flow-control is off, output flow-control is unsupported
      Members in this channel: Gi1/1/0/1 Gi2/1/0/1
      ARP type: ARPA, ARP Timeout 04:00:00
      Last input 1d02h, output 00:00:00, output hang never
      Last clearing of "show interface" counters never
      Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 8927
      Queueing strategy: fifo
      Output queue: 0/40 (size/max)
      5 minute input rate 47000 bits/sec, 32 packets/sec
      5 minute output rate 47000 bits/sec, 29 packets/sec
         1385339740 packets input, 177768658761 bytes, 0 no buffer
         Received 9129816 broadcasts (7769398 multicasts)
         0 runts, 0 giants, 0 throttles
         0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
         0 watchdog, 7769398 multicast, 0 pause input
         0 input packets with dribble condition detected
         596842574 packets output, 281113115957 bytes, 0 underruns
         0 output errors, 0 collisions, 6 interface resets
         0 unknown protocol drops
         0 babbles, 0 late collision, 0 deferred
         0 lost carrier, 0 no carrier, 0 pause output
         0 output buffer failures, 0 output buffers swapped out

    CAATINGA01# show int Po1 capabilities

    Port-channel1
      Model:                 Unknown PID
      Type:                  10/100/1000BaseTX
      Speed:                 10,100,1000,auto
      Duplex:                full,half,auto
      Trunk encap. type:     802.1Q
      Trunk mode:            on,off,desirable,nonegotiate
      Channel:               no
      Broadcast suppression: no
      Unicast suppression:   no
      Multicast suppression: no
      Flowcontrol:           rx-(off,on,desired),tx-(none)
      Fast Start:            no
      QoS scheduling:        no
      CoS rewrite:           no
      ToS rewrite:           no
      UDLD:                  no
      Inline power:          no
      SPAN:                  source
      PortSecure:            no
      Dot1x:                 no
      Diagnostic Monitoring: N/A

    CAATINGA01# show run int po1

    Building configuration...

    Current configuration : 176 bytes
    !
    interface Port-channel1
     description ESCOLTA_2B_10.255.10.4
     switchport trunk native vlan 255
     switchport trunk allowed vlan 110,120,121,255
     switchport mode trunk
    end

    CAATINGA01# show etherchannel 1 summary

    Number of channel-groups in use: 11
    Number of aggregators:           11

    Group  Port-channel  Protocol    Ports
    ------+-------------+-----------+-----------------------------------------------
    1      Po1(SU)         LACP        Gi1/1/0/1(P)  Gi2/1/0/1(P)


    CAATINGA01# show etherchannel port-channel
    
                    Channel-group listing:
                    ----------------------
    Group: 1
    ----------
                    Port-channels in the group:
                    ---------------------------

    Port-channel: Po1    (Primary Aggregator)
    ------------
    Age of the Port-channel   = 108d:14h:11m:46s
    Logical slot/port   = 30/1          Number of ports = 2
    HotStandBy port = null
    Port state          = Port-channel Ag-Inuse
    Protocol            =   LACP
    Port security       = Disabled

    Ports in the Port-channel:

    Index   Load   Port     EC state        No of bits
    ------+------+------+------------------+-----------
      0     00     Gi1/1/0/1 Active             0
      0     00     Gi2/1/0/1 Active             0

    Time since last port bundled:    24d:14h:03m:00s    Gi1/1/0/1
    Time since last port Un-bundled: 24d:14h:10m:37s    Gi1/1/0/1

    CAATINGA01# show int Gi2/1/0/1 controller | i up
    
    GigabitEthernet2/1/0/1 is up, line protocol is up (connected)
      Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
      input flow-control is off, output flow-control is unsupported

    CAATINGA01# show int Gi2/1/0/2 controller | i up
    
    GigabitEthernet2/1/0/2 is up, line protocol is up (connected)
      Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
      input flow-control is off, output flow-control is unsupported

    CAATINGA01# show int Gi2/1/0/1 controller | i drop
    
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 7456
     0 unknown protocol drops
            0 Gold frames dropped                   0 FcsErr frames

    CAATINGA01# show int Gi2/1/0/2 controller | i drop
    
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 2420
     0 unknown protocol drops
            0 Gold frames dropped                   0 FcsErr frames


    CAATINGA01# show int Gi2/1/0/1 controller | i Defer
    
         7456 Excess Defer frames                   0 Collision fragments
            0 Deferred frames                       0 ValidOverSize frames

    CAATINGA01# how int Gi2/1/0/2 controller | i Defer
    
         2420 Excess Defer frames                   0 Collision fragments
            0 Deferred frames                       0 ValidOverSize frames

    CAATINGA01# sh int Po1 | i CRC

         0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored


    CAATINGA01# sh int Po1 | in up | drops

    Port-channel1 is up, line protocol is up (connected)
      Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 8927
         0 unknown protocol drops

    CAATINGA01# ping 10.255.10.4 size 1500 repeat 1000 df-bit

    Type escape sequence to abort.
    Sending 1000, 1500-byte ICMP Echos to 10.255.10.4, timeout is 2 seconds:
    Packet sent with the DF bit set
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    !!!!!!!!!!!!!!!!!!!!
    Success rate is 100 percent (1000/1000), round-trip min/avg/max = 1/1/8 ms


Acesso SW 10.255.10.4 (Catalyst Switch C9200L-48P-4G-E)::


    ANUBIS-N01_4A# show module

    Switch  Ports    Model                Serial No.   MAC address     Hw Ver.       Sw Ver.
    ------  -----   ---------             -----------  --------------  -------       --------
     1       52     C9200L-48P-4G-E       JAE23011SDV  7488.bb49.1d00  V01           16.9.3

    ANUBIS-N01_4A# show mac address-table address d0:94:66:b1:90:e5
              Mac Address Table
    -------------------------------------------
    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
     120    d094.66b1.90e5    DYNAMIC     Gi1/0/45
    Total Mac Addresses for this criterion: 1

    ANUBIS-N01_4A# sh run int Gi1/0/45

    Building configuration...

    Current configuration : 91 bytes
    !
    interface GigabitEthernet1/0/45
     switchport access vlan 120
     switchport mode access
    end

Não há port security ativado nessa porta::

    ANUBIS-N01_4A# sh port int Gi1/0/45
    Port Security              : Disabled
    Port Status                : Secure-down
    Violation Mode             : Shutdown
    Aging Time                 : 0 mins
    Aging Type                 : Absolute
    SecureStatic Address Aging : Disabled
    Maximum MAC Addresses      : 1
    Total MAC Addresses        : 0
    Configured MAC Addresses   : 0
    Sticky MAC Addresses       : 0
    Last Source Address:Vlan   : 0000.0000.0000:0
    Security Violation Count   : 0

    ANUBIS-N01_4A# sh int gi1/0/45 trunk

    Port        Mode             Encapsulation  Status        Native vlan
    Gi1/0/45    off              802.1q         not-trunking  1

    Port        Vlans allowed on trunk
    Gi1/0/45    120

    Port        Vlans allowed and active in management domain
    Gi1/0/45    120

    Port        Vlans in spanning tree forwarding state and not pruned
    Gi1/0/45    120


    ANUBIS-N01_4A# sh cdp neighbor

    Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                      S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone,
                      D - Remote, C - CVTA, M - Two-port Mac Relay

    Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
    ANUBIS-N01.jehuty.com.jp
                     Gig 1/1/2         139             R S I  C9404R    Gig 1/1/0/1
    ANUBIS-N01.jehuty.com.jp
                     Gig 1/1/1         166             R S I  C9404R    Gig 2/1/0/1

    Total cdp entries displayed : 2


    ANUBIS-N01_4A# sh int Gi1/0/45 status

    Port      Name               Status       Vlan       Duplex  Speed Type
    Gi1/0/45                     connected    120        a-full a-1000 10/100/1000BaseTX

    GigabitEthernet1/0/45 is up, line protocol is up (connected)
      Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
      input flow-control is off, output flow-control is unsupported


    ANUBIS-N01_4A# sh int Gi1/0/45 controller | i drops
      Input queue: 0/2000/0/0 (size/max/drops/flushes); Total output drops: 434
     17959 unknown protocol drops

    ANUBIS-N01_4A# sh int Gi1/0/45 controller | i Defer
              434 Excess Defer frames                   0 Collision fragments
                0 Deferred frames                       0 ValidOverSize frames


    ANUBIS-N01_4A# sh int Gi1/0/45 controller | i CRC
         0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored


    ANUBIS-N01_4A# sh int Gi1/0/45 counters errors

    Port        Align-Err     FCS-Err    Xmit-Err     Rcv-Err  UnderSize  OutDiscards
    Gi1/0/45            0           0           0           0          0          434

    Port      Single-Col  Multi-Col   Late-Col  Excess-Col  Carri-Sen      Runts
    Gi1/0/45           0          0          0           0          0          0

    ANUBIS-N01_4A# sh int gi1/0/45 switchport

    Name: Gi1/0/45
    Switchport: Enabled
    Administrative Mode: static access
    Operational Mode: static access
    Administrative Trunking Encapsulation: dot1q
    Operational Trunking Encapsulation: native
    Negotiation of Trunking: Off
    Access Mode VLAN: 120 (Bradesco_Seg-1)
    Trunking Native Mode VLAN: 1 (default)
    Administrative Native VLAN tagging: disabled
    Voice VLAN: none
    Administrative private-vlan host-association: none
    Administrative private-vlan mapping: none
    Administrative private-vlan trunk native VLAN: none
    Administrative private-vlan trunk Native VLAN tagging: enabled
    Administrative private-vlan trunk encapsulation: dot1q
    Administrative private-vlan trunk normal VLANs: none
    Administrative private-vlan trunk associations: none
    Administrative private-vlan trunk mappings: none
    Operational private-vlan: none
    Trunking VLANs Enabled: ALL
    Pruning VLANs Enabled: 2-1001
    Capture Mode Disabled
    Capture VLANs Allowed: ALL

    Protected: false
    Unknown unicast blocked: disabled
    Unknown multicast blocked: disabled
    Vepa Enabled: false
    Appliance trust: none

    ANUBIS-N01_4A# sh int gi1/0/45 sum

     *: interface is up
     IHQ: pkts in input hold queue     IQD: pkts dropped from input queue
     OHQ: pkts in output hold queue    OQD: pkts dropped from output queue
     RXBS: rx rate (bits/sec)          RXPS: rx rate (pkts/sec)
     TXBS: tx rate (bits/sec)          TXPS: tx rate (pkts/sec)
     TRTL: throttle count
      Interface                   IHQ       IQD       OHQ       OQD      RXBS      RXPS      TXBS      TXPS      TRTL
    -----------------------------------------------------------------------------------------------------------------
    * GigabitEthernet1/0/45         0         0         0       434         0         0      4000         4         0


    ANUBIS-N01_4A# show diagnostic description switch 1 test 1

    DiagGoldPktTest :
            The GOLD packet Loopback test verifies the MAC level loopback
            functionality. In this test, a GOLD packet, for which doppler
            provides the support in hardware, is sent. The packet loops back
            at MAC level and is matched against the stored packet. It is a non
            -disruptive test.

    ANUBIS-N01_4A# diagnostic start switch 1 test 1

    ANUBIS-N01_4A# diagnostic stop switch 1
    Diagnostic[switch 1]: Diagnostic is not active.


    ANUBIS-N01_4A# show diagnostic result switch 1

    Current bootup diagnostic level: minimal

    switch 1:   SerialNo : JAE23011SDV

      Overall Diagnostic Result for switch 1 : PASS
      Diagnostic level at card bootup: minimal

      Test results: (. = Pass, F = Fail, U = Untested)

        1) DiagGoldPktTest:

       Port  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24
       ----------------------------------------------------------------------------
             .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .

       Port 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48
       ----------------------------------------------------------------------------
             .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .

       Port 49 50 51 52
       ----------------
             .  .  .  .


        2) DiagThermalTest -----------------> .
        3) DiagPhyLoopbackTest:

       Port  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24
       ----------------------------------------------------------------------------
             U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U

       Port 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48
       ----------------------------------------------------------------------------
             U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U

       Port 49 50 51 52
       ----------------
             U  U  U  U


        4) DiagScratchRegisterTest ---------> .
        5) TestUnusedPortLoopback:

       Port  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24
       ----------------------------------------------------------------------------
             U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U

       Port 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48
       ----------------------------------------------------------------------------
             U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U  U

       Port 49 50 51 52
       ----------------
             U  U  U  U


        6) DiagPoETest ---------------------> U
        7) DiagStackCableTest --------------> U
        8) DiagMemoryTest ------------------> U

    ANUBIS-N01_4A# show logging | i GigabitEthernet1/0/38

    Mar  6 23:01:31.868: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0/38, changed state to down
    Mar  6 23:01:32.870: %LINK-3-UPDOWN: Interface GigabitEthernet1/0/38, changed state to down
    Mar  6 23:01:35.404: %LINK-3-UPDOWN: Interface GigabitEthernet1/0/38, changed state to up
    Mar  6 23:01:36.405: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0/38, changed state to up
    Mar  6 23:01:51.115: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0/38, changed state to down
    Mar  6 23:01:52.117: %LINK-3-UPDOWN: Interface GigabitEthernet1/0/38, changed state to down
    Mar  6 23:01:55.786: %LINK-3-UPDOWN: Interface GigabitEthernet1/0/38, changed state to up
    Mar  6 23:01:56.784: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet1/0/38, changed state to up


    ANUBIS-N01_4A# show clock

    23:02:19.957 BRAZIL Fri Mar 6 2020


.. note:: Foi identificado erros de network flapping na porta 45 do switch 10.255.10.4. Onde ocorre um número elevado de conexão e desconexão progressiva da estação, recorrente sempre na mesma porta. Consultamos a documentação do próprio fabricante o qual diz que isso está relacionado à problema de camada física.

**Excess Defer frames**:: - According to Cisco documentation it is the number of frames that are not sent after the time exceeds the maximum-packet time. It means that the port is under heavy load. The device connected to the port is transmitting or receiving more traffic that can be handled by the port.

.. note:: Veja as recomendações gerais do fabricante Cisco, que ajudam mitigar os erros de **network flapping**.

**Cabo incorreto**:: - Troque o cabo suspeito por um cabo em bom funcionamento (use apenas padrao cat 6). Procure por pinos quebrados ou faltantes nos conectores. É uma boa hora para revisar a pinagem das tomadas em ambos os pontos (da sala tecnica até a PA). Também de preferência, tente conectar um dispositivo fluke e rode um diagnostico completo para checar às medições de impedância do cabo UTP (Veja se há interferencias, do tipo crossstalk no cabeamento).

**Verificar possíveis conexões soltas ou mal encaixadas**:: - Ver se existem conexões soltas porque às vezes, parece que um cabo está colocado na tomada, mas não está. Desconecte o cabo e o reintroduza.

**Na estação verifique a placa de rede**:: - Atualize os drivers de rede através do site do fabricante (Dell Optiplex 3060), desative economia de energia da interface e a mantenha configurada em 1000 full duplex com a opção manual ativada.

'Outros testes possíveis e que podem ser válidos para mitigação do problema':: - Trocar a porta de conexão com o mesmo SW. Realizar também a mudança da máquina para outro ponto de rede (ponto da máquina ao lado que estiver normal). Checar se o problema ainda persiste. 













