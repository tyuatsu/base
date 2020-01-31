Avaya Nortel Ethernet Routing Switch
------------------------------------

.. note:: Esse é um guia rápido para os iniciantes se familiarizarem com os comandos básicos dos Switches da Nortel.


Dados fornecidos geralmente são o MAC ou IP e Vlan de cada Operação.

Acesso ao SW Cores (apenas vizualização não entrar no modo config). Procure localizar o SW de acesso da operação e conferir
sempre a porta (tag all) com o sys topology (nem todos os SW de acesso estão conectados nas portas padrão 49 e 50)
Uma vez localizado o Switch de acesso, acessar e verificar o status da porta, o Mac e as Vlans que estão passando na interface desse Switch.

Configurar o Switch IP 10.255.11.28 portas:45 e 46 na vlan-id: **101** (IP 10.221.64.0) e também as portas: 6, 9 ,17, 21 e 39 na vlan-id: **220** (IP 172.31.78.0).
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Acesse o SW CORE, digite o comando abaixo e tecle **ENTER**::

    CORETESTSW01# show ip arp info 10.255.11.28

    *******************************************************************************
    Command Execution Time: TUE JAN 28 09:58:58 2020 GMT
    *******************************************************************************

    ================================================================================
                             IP Arp - GlobalRouter
    ================================================================================
    IP_ADDRESS      MAC_ADDRESS        VLAN    PORT       TYPE    TTL(10 Sec)
    --------------------------------------------------------------------------------
    10.255.11.28    00:23:0d:03:68:00  255     3/7      DYNAMIC 1263

    CORETESTSW02# show ip arp info 10.255.11.28
    
    *******************************************************************************
    Command Execution Time: TUE JAN 28 10:00:09 2020 GMT
    *******************************************************************************

    ================================================================================
                             IP Arp - GlobalRouter
    ================================================================================
    IP_ADDRESS      MAC_ADDRESS        VLAN    PORT       TYPE    TTL(10 Sec)
    --------------------------------------------------------------------------------
    10.255.11.28    00:23:0d:03:68:00  255     1/39      DYNAMIC 295


VLANS QUE ATUALMENTE ESTÃO PASSANDO PELA PORTA DO **CORE PRIMÁRIO** ONDE O SWITCH ESTÁ CONECTADO::

    CORETESTSW01#  show ports info vlans port 3/7
    
    *******************************************************************************
    Command Execution Time: TUE JAN 28 09:50:06 2020 GMT
    *******************************************************************************

    ================================================================================
                                   Port Vlans
    ================================================================================
    PORT          DISCARD DISCARD   DEFAULT VLAN   UNTAG
    NUM   TAGGING TAGFRAM UNTAGFRAM VLANID  IDS    DEFVLAN
    --------------------------------------------------------------------------------
    3/7   enable  false   true      255     100 109 214 250 255 271 3138   disable


VLANS QUE ATUALMENTE ESTÃO PASSANDO PELA PORTA DO **CORE SECUNDÁRIO** ONDE O SWITCH ESTÁ CONECTADO::

    CORETESTSW02# show ports info vlans port 1/39
    
    *******************************************************************************
    Command Execution Time: TUE JAN 28 10:00:55 2020 GMT
    *******************************************************************************

    ================================================================================
                                   Port Vlans
    ================================================================================
    PORT          DISCARD DISCARD   DEFAULT VLAN   UNTAG
    NUM   TAGGING TAGFRAM UNTAGFRAM VLANID  IDS    DEFVLAN
    --------------------------------------------------------------------------------
    1/39  enable  false   true      255     100 109 214 250 255 271 3138   disable


Há 8 VLANS atualmente cadastradas no SWITCH DE ACESSO **(IP 10.255.11.28)** mas nenhuma delas atendem as vlans 101 e 220::

    sw1.teste# show vlan
    
    Id   Name                 Type     Protocol         PID     Active IVL/SVL Mgmt
    ---- -------------------- -------- ---------------- ------- ------ ------- ----
    1    VLAN #1              Port     None             0x0000  Yes    IVL     No
            Port Members: NONE
    100  VLAN-10.220.11       Port     None             0x0000  Yes    IVL     No
            Port Members: 30-32,35,37-38,49-50
    109  VLAN_73              Port     None             0x0000  Yes    IVL     No
            Port Members: 1-29,33-34,38-50
    214  VLAN_172.31.173      Port     None             0x0000  Yes    IVL     No
            Port Members: 1-13,15-17,19-21,23-29,38-50
    250  VLAN-WIFI-MONICA     Port     None             0x0000  Yes    IVL     No
            Port Members: 36-37,49-50
    255  GER_BIDU             Port     None             0x0000  Yes    IVL     Yes
            Port Members: 49-50
    271  VLAN-271             Port     None             0x0000  Yes    IVL     No
            Port Members: 49-50
    3138 CORP-VOZ             Port     None             0x0000  Yes    IVL     No
            Port Members: 22,30-35,49-50
    Total VLANs: 8

Para adicionar 2 novas vlans no Switch de Acesso, antes será necessário realizar ALTERAÇÔES em abos os CORES (Primário e Secundário), em seguida TAGEAR VLAN PORT DO CORE de acordo com o tipo da vlan **(by port ou by srcmac)**. Muita calma nessa hora pois em abientes de produção, um bom planejamento por Gmud se faz necessário em situações como essas. ::


    CORETESTSW01# show vlan info basic 101
    
    *******************************************************************************
    Command Execution Time: WED JAN 29 03:55:15 2020 GMT
    *******************************************************************************

    ================================================================================
                                   Vlan Basic
    ================================================================================
    VLAN                              STG
    ID    NAME             TYPE         ID  PROTOCOLID SUBNETADDR      SUBNETMASK   
    --------------------------------------------------------------------------------
    101   VLAN - 64        byPort       1   none       N/A             N/A          

    CORETESTSW01# show vlan info basic 220
    
    *******************************************************************************
    Command Execution Time: WED JAN 29 03:55:20 2020 GMT
    *******************************************************************************

    ================================================================================
                                   Vlan Basic
    ================================================================================
    VLAN                              STG
    ID    NAME             TYPE         ID  PROTOCOLID SUBNETADDR      SUBNETMASK   
    --------------------------------------------------------------------------------
    220   VLAN - 172.31.78.0/24 - Magali bySrcMac     1   none       N/A             N/A


    CORETESTSW01# show vlan info advance 220
    
    ================================================================================
                                      Vlan Advance
    ================================================================================
    VLAN             IF    QOS   AGING MAC                USER
    ID    NAME       INDEX LVL   TIME  ADDRESS       DEFINEPID ENCAP  DSAP/                                                                                        
    --------------------------------------------------------------------------------

    220   VLAN - 172.31.78.0/24 - Magali 2268  1     600   3c:b1:5b:2d:22:3b  0x0000
    

    CORETESTSW01# show vlan info advance 101
    
    *******************************************************************************
    Command Execution Time: WED JAN 29 04:05:49 2020 GMT
    *******************************************************************************

    ================================================================================
                                  Vlan Advance
    ================================================================================
    VLAN             IF    QOS   AGING MAC                USER
    ID    NAME       INDEX LVL   TIME  ADDRESS            DEFINEPID ENCAP  DSAP/SSAP
    --------------------------------------------------------------------------------
    101   VLAN - 64  2149  1     0     3c:b1:5b:2d:22:0f  0x0000

A vlan 101 é By Port, portanto temos que adicionar Vlan-Id através da porta (BY PORT) em ambos os CORES::

    CORETESTSW01# vlan 101 ports add 3/7 member portmember
    CORETESTSW02# vlan 101 ports add 1/39 member portmember

A vlan 220 é By Source Mac, portanto temos que adicionar Vlan-Id através da porta (BY SRCMAC) em ambos os CORES::

    CORETESTSW01# vlan 220 ports add 3/7 member static 
    CORETESTSW02# vlan 220 ports add 1/39 member static 


Vamos realizar agora as alterações necessárias no SWITCH DE ACESSO **(IP 10.255.11.28)**.

Após ter realizado o tageamento vlan port nos CORES, agora vamos ajustar as Vlans 101 e 220 nas interfaces no SW de acesso.

OBS: No PVID a vlan padrão 101 e 220 ambas são vlan de dados (prioridade sempre é para o tráfego de dados).
 
Adicionar a VLAN-ID 101 nas Portas 46 e 45.::

    sw1.teste# vlan members add 101 45-46
    sw1.teste# vlan ports 45-46 pvid 101
    sw1.teste# show vlan

Adicionar a VLAN-ID 220 nas Portas 6, 9, 17, 21 e 39::

    sw1.teste# vlan members add 220 6,9,17,21,39
    sw1.teste# vlan ports 6,9,17,21,39 pvid 220
    sw1.teste# show vlan


Nos CORES (Primário e Secundário), deverão ser adicionados, obrigatoriamente, os MACs das estações na tabela SRCMAC da vlan 220. Veja o exemplo abaixo::

    CORETESTSW01# vlan 220 srcmac add 64:1c:67:9B:82:6B
    CORETESTSW01# show ip arp


Avaya Nortel Ethernet Routing Switch - Tagging, Tagall, Untagall, Tagpvid e Untagpvid
-------------------------------------------------------------------------------------

.. note:: Conceito básico de frames UntagPvid Only para o correto funcionamento do tráfego de dados dos PCs e tráfego de voz em bases de IP Phone Avaya.

.. figure:: untagpvidonly.png
    :scale: 90 %
    :align: center
    :alt: Avaya Nortel

