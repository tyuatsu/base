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
    
    
    
