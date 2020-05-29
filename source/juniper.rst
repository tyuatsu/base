Router Juniper
--------------

.. note:: Checagem da interface Wan - Análise de perda de pacotes, incremento de erro ou quedas recentes.

.. figure:: juniper.jpg
    :scale: 60 %
    :align: center
    :alt: Juniper

1. Comandos Básicos Router Juniper
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Verificar informações de todas interfaces::

    Supersonic@C3PO> show interfaces descriptions

Mostrar breve resumo da interface::

    Supersonic@C3PO> show interfaces ge-0/0/1 terse

Verificar últimos logs de quedas::

    Supersonic@C3PO>show log messages | last 200

Verificar as informações de todas interfaces::

    Supersonic@C3PO> show interfaces descriptions | find ge-0/0/1 
    Supersonic@C3PO> show interfaces descriptions | find ge

Verificar informações da interface::

    Supersonic@C3PO> show interfaces descriptions | match ge-0/0/1

Verificar se está fisicamente up ou down::

    Supersonic@C3PO> show interfaces detail | match ge-0/0/1

Visualizar todas as interfaces::

    Supersonic@C3PO> show interfaces

Visualizar todas as interfaces com detalhamento::

    Supersonic@C3PO> show interfaces brief

Visualiza todos os tipos de erros::

    Supersonic@C3PO> show interfaces ge-0/0/1 extensive
    Supersonic@C3PO> show interfaces ge-0/0/1 extensive | match errors

Verificar tempo de operação do equipamento::

    Supersonic@C3PO> show system uptime
    
    
Ping teste::

    Supersonic@C3PO> ping 200.185.0.60 rapid count 50 size 1500 
    
    PING 200.185.0.60 (200.185.0.60): 1500 data bytes
    !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
    --- 200.185.0.60 ping statistics ---
    50 packets transmitted, 50 packets received, 0% packet loss
    round-trip min/avg/max/stddev = 0.163/0.174/0.311/0.024 ms
    
    
Troubleshooting    
===============
    
.. note::  Coletar evidências - pontos principais

* Logs
* show interface ge-X/Y/Z
* show bfd session brief
* show isis adjacency
* testes de ping

.. note:: Complementos da análise

* tabela arp
* show bfd session brief
* show isis adjacency 

Exemplos de comandos (simulação) validar o roteamento entre o router *astronauta* e router *magali* 171.28.1.210 (Elemento ROTEAMENTO ISIS PEER)::

    juca@astronauta008> show interfaces ge-1/1/9
    Physical interface: ge-1/1/9, Enabled, Physical link is Up
      Interface index: 159, SNMP ifIndex: 535
      Description: L2L CenturyLink Boston TO-magaliO06 ge-0/0/2
      Link-level type: Ethernet, MTU: 2000, Speed: 1000mbps, BPDU Error: None, MAC-REWRITE Error: None, Loopback: Disabled, Source
      filtering: Disabled, Flow control: Enabled, Auto-negotiation: Enabled, Remote fault: Online
      Device flags   : Present Running
      Interface flags: SNMP-Traps Internal: 0x0
      Link flags     : None
      CoS queues     : 8 supported, 8 maximum usable queues
      Current address: 88:e0:f3:7f:39:81, Hardware address: 88:e0:f3:7f:39:81
      Last flapped   : 2019-11-13 22:36:25 ART (28w0d 11:25 ago)
      Input rate     : 256 bps (0 pps)
      Output rate    : 0 bps (0 pps)
      Active alarms  : None
      Active defects : None
      Interface transmit statistics: Disabled

      Logical interface ge-1/1/9.0 (Index 328) (SNMP ifIndex 567)
        Flags: SNMP-Traps 0x0 Encapsulation: ENET2
        Input packets : 18648522681 
        Output packets: 23471212317
        Protocol inet, MTU: 1986
          Flags: Sendbcast-pkt-to-re
          Addresses, Flags: Is-Preferred Is-Primary
            Destination: 171.28.1.208/30, Local: 171.28.1.209, Broadcast: 171.28.1.211
        Protocol iso, MTU: 1983
        Protocol mpls, MTU: 1974, Maximum labels: 3
        Protocol multiservice, MTU: Unlimited

    juca@astronauta008> 
    juca@astronauta008> show configuration | display set | match ge-1/1/9 
    set interfaces ge-1/1/9 description "L2L CenturyLink Boston TO-magaliO06 ge-0/0/2"
    set interfaces ge-1/1/9 mtu 2000
    set interfaces ge-1/1/9 gigether-options auto-negotiation
    set interfaces ge-1/1/9 unit 0 family inet address 171.28.1.209/30
    set interfaces ge-1/1/9 unit 0 family iso
    set interfaces ge-1/1/9 unit 0 family mpls
    set protocols rsvp interface ge-1/1/9.0 hello-interval 0
    set protocols mpls interface ge-1/1/9.0
    set protocols isis interface ge-1/1/9.0 point-to-point
    set protocols isis interface ge-1/1/9.0 bfd-liveness-detection minimum-interval 1000
    set protocols isis interface ge-1/1/9.0 bfd-liveness-detection minimum-receive-interval 1000
    set protocols isis interface ge-1/1/9.0 bfd-liveness-detection multiplier 3
    set protocols isis interface ge-1/1/9.0 level 1 disable
    set protocols ldp interface ge-1/1/9.0

    juca@astronauta008> 
    juca@astronauta008> show arp no-resolve | match 171.28.1. 
    88:e0:f3:7f:37:79 171.28.1.54     ge-1/1/3.0           none
    88:e0:f3:7f:44:7f 171.28.1.57     ge-1/1/5.0           none
    b8:c2:53:f4:e7:66 171.28.1.177    ge-1/1/4.0           none
    88:a2:5e:64:07:71 171.28.1.202    ge-1/1/8.0           none

    juca@astronauta008> 
    juca@astronauta008> ping no-resolve 171.28.1.210 source 171.28.1.209 
    PING 171.28.1.210 (171.28.1.210): 56 data bytes
    ^C
    --- 171.28.1.210 ping statistics ---
    8 packets transmitted, 0 packets received, 100% packet loss

    juca@astronauta008> 
    juca@astronauta008> ping no-resolve 171.28.1.210 source 171.28.1.209 rapid 
    PING 171.28.1.210 (171.28.1.210): 56 data bytes
    .....
    --- 171.28.1.210 ping statistics ---
    5 packets transmitted, 0 packets received, 100% packet loss

    juca@astronauta008> 
    juca@astronauta008> show bfd session brief    
                                                      Detect   Transmit
    Address                  State     Interface      Time     Interval  Multiplier
    171.28.1.177             Up        ge-1/1/4.0     3.000     1.000        3   
    171.28.1.202             Up        ge-1/1/8.0     3.000     1.000        3   
    31.13.75.94              Up        ge-1/0/6.0     3.000     1.000        3   
    31.13.75.109             Up        ge-1/0/4.0     3.000     1.000        3   
    31.13.3.21               Up        ge-1/1/7.0     3.000     1.000        3   
    31.13.3.146              Up        ge-1/0/1.0     3.000     1.000        3   
    31.13.3.242              Up        ge-1/0/2.0     3.000     1.000        3   

    7 sessions, 7 clients
    Cumulative transmit rate 7.0 pps, cumulative receive rate 7.0 pps

    juca@astronauta008> 
    juca@astronauta008> show isis adjacency    
    Interface             System         L State        Hold (secs) SNPA
    ge-1/0/1.0            RO015          2  Up                   19
    ge-1/0/2.0            RO035          2  Up                   20
    ge-1/0/4.0            RO054          2  Up                   20
    ge-1/0/6.0            RO025          2  Up                   22
    ge-1/1/1.993          RO105          2  Up                   20
    ge-1/1/3.0            RO042          2  Up                   24  88:e0:f3:7f:37:79
    ge-1/1/4.0            RO058          2  Up                   26
    ge-1/1/5.0            RO027          2  Up                    6  88:e0:f3:7f:44:7f
    ge-1/1/7.0            RO056          2  Up                   21
    ge-1/1/8.0            RO063          2  Up                   21

    juca@astronauta008> 
    juca@astronauta008> exit


Mais comandos úteis para verificação
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Verificar tempo de operação do equipamento::

    Atom@R2D2> show system uptime

Verificar usuários logados::

    Atom@R2D2> show system users

Verificar armazenamento de dados local::

    Atom@R2D2> show system storage

Verificar a tabela de processos em execução::

    Atom@R2D2> show system processes

Verificar componentes de hardware instalados::

    Atom@R2D2> show chassis hardware

Verificar status dos componentes e temperatura, e velocidades do sistema de refrigeração::

    Atom@R2D2> show chassis environment

Verificar status da routing engine::

    Atom@R2D2> show chassis routing-engine

Verificar alarmes de alertas no equipamento::

    Atom@R2D2> show chassis alarms 

Versão do Junos OS que esta rodando no equipamento. Também exibe o host name e o modelo do dispositivo::

    Atom@R2D2> show version

Verificar logs relacionados aos alarmes do hardware::

    Atom@R2D2> show log chassisd | no-more 

Verificar mensagens de log::

    Atom@R2D2> show log messages | no-more 

Verificar histórico de login dos usuários::

    Atom@R2D2> show log user 

Verificar a configuração atual::

    Atom@R2D2> show configuration 

Breve descrição sobre o status das interfaces::

    Atom@R2D2> show interfaces terse 

    Atom@R2D2> run show interfaces ge-0/0/1.0 terse


Descrição sobre uma interface::

    Atom@R2D2> show interfaces interface-name 

Descrição detalhada sobre uma interface::

    Atom@R2D2> show interface interface-name extensive 

Reinicia as estatísticas de uma interface::

    Atom@R2D2> clear interfaces statistics interface-name 

Breve descrição sobre o estado do protocolo::

    Atom@R2D2> show bgp summary 

Breve descrição sobre o estado do protocolo::

    Atom@R2D2> show ospf overview 

Breve descrição sobre o estado do protocolo::

    Atom@R2D2> show isis overview 

Descrição detalhada sobre uma rota específica::

    Atom@R2D2> show route 216.142.248.0 extensive 

Descrição sobre uma rota específica::

    Atom@R2D2> show route 192.168.68.0/24 

Descrição da tabela de rotas::

    Atom@R2D2> show route terse 

ENABLE / DISABLE INTERFACE IN JUNIPER::

    Atom@R2D2# set interfaces ge-0/0/1.0 disable  (This is cisco equivalent of **shutdown**)
    Atom@R2D2# delete interfaces ge-0/0/1.0 disable (This is cisco equivalent of **no shutdown**)
    Atom@R2D2# show ge-0/0/1.0
    Atom@R2D2# run show interfaces ge-0/0/1.0 terse
    
    
    
