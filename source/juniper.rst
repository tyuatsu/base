Router Juniper
--------------

.. note:: Checagem da interface Wan - Análise de perda de pacotes, incremento de erro ou quedas recentes.

.. figure:: juniper.jpg
    :scale: 60 %
    :align: center
    :alt: Juniper

1. Comandos Básicos Router Juniper
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

echo VISUALIZA TODOS OS TIPOS DE ERROS | tr [A-Z] [a-z]

Verificar informações de todas interfaces::

    supersonic@RT01> show interfaces descriptions

Verificar últimos logs de quedas::

    ter00775@RO060> show log messages | last 200

Verificar as informações de todas interfaces::

    supersonic@RT01> show interfaces descriptions | find ge-0/0/1 
    supersonic@RT01> show interfaces descriptions | find ge

Verificar informações da interface::

    supersonic@RT01> show interfaces descriptions | match ge-0/0/1

Verificar se está fisicamente up ou down::

    supersonic@RT01> show interfaces detail | match ge-0/0/1

Visualizar todas as interfaces::

    supersonic@RT01> show interfaces

Visualizar todas as interfaces com detalhamento::

    supersonic@RT01> show interfaces brief

Visualiza todos os tipos de erros::

    supersonic@RT01> show interfaces ge-0/0/1 extensive
    supersonic@RT01> show interfaces ge-0/0/1 extensive | match errors

Verificar tempo de operação do equipamento::

    supersonic@RT01> show system uptime


Mais comandos úteis para verificação
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Verificar tempo de operação do equipamento::

    subatomic@RO02> show system uptime

Verificar usuários logados::

    subatomic@RO02> show system users

Verificar armazenamento de dados local::

    subatomic@RO02> show system storage

Verificar a tabela de processos em execução::

    subatomic@RO02> show system processes

Verificar componentes de hardware instalados::

    subatomic@RO02> show chassis hardware

Verificar status dos componentes e temperatura, e velocidades do sistema de refrigeração::

    subatomic@RO02> show chassis environment

Verificar status da routing engine::

    subatomic@RO02> show chassis routing-engine

Verificar alarmes de alertas no equipamento::

    subatomic@RO02> show chassis alarms 

Versão do Junos OS que esta rodando no equipamento. Também exibe o host name e o modelo do dispositivo::

    subatomic@RO02> show version

Verificar logs relacionados aos alarmes do hardware::

    subatomic@RO02> show log chassisd | no-more 

Verificar mensagens de log::

    subatomic@RO02> show log messages | no-more 

Verificar histórico de login dos usuários::

    subatomic@RO02> show log user 

Verificar a configuração atual::

    subatomic@RO02> show configuration 

Breve descrição sobre o status das interfaces::

    subatomic@RO02> show interfaces terse 

Descrição sobre uma interface::

    subatomic@RO02> show interfaces interface-name 

Descrição detalhada sobre uma interface::

    subatomic@RO02> show interface interface-name extensive 

Reinicia as estatísticas de uma interface::

    subatomic@RO02> clear interfaces statistics interface-name 

Breve descrição sobre o estado do protocolo::

    subatomic@RO02> show bgp summary 

Breve descrição sobre o estado do protocolo::

    subatomic@RO02> show ospf overview 

Breve descrição sobre o estado do protocolo::

    subatomic@RO02> show isis overview 

Descrição detalhada sobre uma rota específica::

    subatomic@RO02> show route 216.142.248.0 extensive 

Descrição sobre uma rota específica::

    subatomic@RO02> show route 192.168.68.0/24 

Descrição da tabela de rotas::

    subatomic@RO02> show route terse 
    
    
    
