CONFIGURAR ROUTER HUAWEI WIFI-AX2S + MODEM TECHNICOLOR TG788V V3 LIVE TIM
-------------------------------------------------------------------------

.. figure:: huawei_wifi_ax2s.jpg
    :scale: 50 %
    :align: center
    :alt: Huawei

.. note:: A Topologia da rede é bem simples, aqui temos um Roteador da Huawei WiFi AX2S que será o nosso equipamento concentrador. Todas as máquinas (smartphones, desktops, notebooks ou printers) serão conectados diretamente no Roteador da Huawei. Mas o responsável pelo Gateway da Rede ou Saída para Internet, será o Modem Technicolor TG788v v3 Live TIM. Todas as etapas a seguir, serão orientadas pela premissa de que já existe um modem TG788v v3 configurado e com WiFi/SSID ativo na rede doméstica do usuário. O objetivo aqui é capar as funcionalidades triviais do modem TG788v v3, desabilitar seus serviços nativos tais como o DHCP, Sinal WiFi e autenticação PPoE. Todos esses serviços mencionados, serão ativados agora no equipamento da Huawei.

.. figure:: topologia-bridge.jpg
    :scale: 80 %
    :align: center
    :alt: Topologia
    
**Para saber mais sobre as especificações do Router** (HUAWEI WiFi AX2S) - `Entre no site oficial do fabricante. <https://consumer.huawei.com/br/routers/ax2s/>`_     
    
Configuração do Modem TG788v v3 em modo Bridge
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Antes de mais nada, faça um backup das configurações do modem e em seguida salve em uma pasta no seu computador. 

1. Navegue até a interface web do TG788v v3.
2. Clique em Gateway. A página Gateway é exibida.
3. Na seção Exportar configuração, clique em Exportar.
4. O TG788v v3 solicita que você salve seu arquivo de backup. Não altere a extensão do arquivo.
5. Salve seu arquivo em um local de sua escolha.

.. figure:: Technicolor-Gateway.png
    :scale: 80 %
    :align: center
    :alt: Technicolor Gateway

.. note:: Nunca edite os arquivos de backup, pois isso pode resultar em arquivos corrompidos, tornando-os inúteis como backup de configuração. Uma vez feito o backup, você sempre poderá retornar à configuração de trabalho mais recente do seu TG788v v3 em caso de problemas.

Agora vamos precisar de um cabo de rede para realizar as configurações no modem. Não dá para fazer isso pelo WiFi. Portanto conecte o cabo de rede na **porta 1** do modem Technicolor TG788v v3 e a outra ponta do cabo vai para o seu notebook/desktop.

.. figure:: Technicolor-Notebook.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Porta 01    

Volte para tela inicial do modem TG788v v3, e agora entre em **Rede Local**, vá para a seção **Configurações DHCP** em seguida deixe desativado a opção Servidor DHCP. Em **Endereço IP do Disposivo Local** altere o IP para **192.168.1.254**. Será necessário apertar tecla F5 ou fazer reload da página web e logar novamente no TG788v, porém agora com novo endereço IP **https://192.168.1.254**.

.. figure:: Technicolor-Rede-Local.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Rede Local

Na tela inicial do TG788v v3, vá para **Rede sem fio** e selecione o seu SSID, aqui o meu é o **Aliens**. Deixe desabilitado com botão na posição off.

.. figure:: Technicolor-WiFi.png
    :scale: 60 %
    :align: center
    :alt: Technicolor WiFi

Volte para tela inicial e finalmente vá para **Accesso Internet**, na parte superior direita clique em modo Avançado para mostrar mais detalhes. No **Modo de Conexão** marque a opção **Bridge**. Em seguida altere o valor VlanID para **100**. Para finalizar clique no botao **Alterar modo de conexão**.

.. figure:: Technicolor-Bridge-Mode.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Bridge Mode
    
A **tela inicial** deverá ficar assim: 
    
.. figure:: Technicolor-Tela-Principal.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Tela Principal

Veja abaixo como devem ficar os **status dos leds** no modem TG788v v3.

Antes da Configuração:

.. figure:: Technicolor-Leds-Antes.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Led Antes.png 

Depois da Configuração:

.. figure:: Technicolor-Leds-Depois.png
    :scale: 60 %
    :align: center
    :alt: Technicolor Led Depois.png

=======================================================================

Configuração Router Huawei WiFi-AX2S
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Finalmente chegamos meus caros, na parte mais crucial da configuração! 😛 yeah! Bora configurar o Roteador Huawei WiFi AX2S!

Primeiro ligue o Huawei WiFi AX2S. Vamos utilizar o velho acesso via web, com uso do próprio navegador no endereço padrão de fábrica **http://192.168.2.1**. Para isso funcionar, você deve conectar o cabo de rede em qualquer uma das portas Lan. Poderá ser usada as portas 2,3 ou 4, e a outra ponta do cabo vai direto para seu computador/notebook. 

Na sequência vamos providenciar também a ligação entre o Roteador e o Modem. Conecte outro cabo de rede na **porta 1** (WAN) do Huawei WiFi AX2S, e a outra ponta do cabo vai para a **porta 4** do modem Technicolor TG788v v3.


.. figure:: WiFi-AX2S-porta-wan.png
    :scale: 60 %
    :align: center
    :alt: WiFi-AX2S porta 1 é Wan

.. figure:: Technicolor-Porta-04.jpg
    :scale: 60 %
    :align: center
    :alt: Technicolor Porta 4

.. note:: A porta 1 será mantida fora da Bridge no modem Technicolor TG788v v3. Portanto enquanto ela estiver funcionando em modo Bridge, procure manter a porta 1 sempre livre. Essa porta deverá ser usada só para fins de manutenção local ou gerência do modem.

.. figure:: Technicolor-Porta-01.jpg
    :scale: 60 %
    :align: center
    :alt: Technicolor Porta 1
    
Na tela inicial do Huawei WiFi AX2S, entre em **Mais Funções**, vá para o Menu a esquerda na seção **Configurações de Rede** e em Endereço IP da LAN altere o IP para **192.168.1.254**. Na Máscara da sub-rede insira **255.255.255.0**. 	

.. figure:: WiFi-AX2S-DHCP-01.png
    :scale: 60 %
    :align: center
    :alt: WiFi AX2S IP de Gerencia

Na opção **Servidor DHCP** deverá deixá-lo ativado, agora entre com os seguintes dados em Int. atribuição end. IP:: **192.168.1.60 - 192.168.1.100** e Tempo de concessão será de **1 Dia**.	

Em Servidor DNS Preferencial entre com: **208.67.222.222** e em DNS Alternativo entre com: **208.67.220.220**.
Para finalizar clique no botão Salvar. Será necessário apertar tecla F5 para reload da página web, e depois logar novamente no Huawei WiFi AX2S. Porém agora pelo novo endereço IP **https://192.168.1.253**.

.. figure:: WiFi-AX2S-DHCP-02.png
    :scale: 60 %
    :align: center
    :alt: WiFi AX2S IP de Gerencia

Volte para a tela principal e vá para **Meu WiFi** e insira um nome SSID para sua rede WiFi, aqui eu usei **Aliens-5G** porém você pode usar outra que quiser. As demais configurações WiFi serão deixadas no padrão de fábrica, permanecerá na melhor performance.

.. figure:: WiFi-AX2S-Meu-WiFi.png
    :scale: 60 %
    :align: center
    :alt: WiFi AX2S Meu WiFi
    
Após salvar as alterações, volte para tela principal do Huawei WiFi AX2S, e depois vá para o ícone do globo (Conectar-se à Internet). 

.. figure:: WiFi-AX2S-Internet.png
    :scale: 60 %
    :align: center
    :alt: WiFi-AX2S Internet
    
Na seção **Modo de Acesso à Internet**, selecione a opção **PPPoE** (PPP over Ethernet is a common connection method used for xDSL.) 
Depois insira as informações de acesso, conforme os dados abaixo::

   User:	guest
   Password:	guest
   MRU:		1492

   Servidor DNS preferencial:  208.67.222.222 
   Servidor DNS alternativo:   208.67.220.220

.. figure:: WiFi-AX2S-Internet-01.png
    :scale: 60 %
    :align: center
    :alt: WiFi-AX2S Internet Step1

Finalize em botão **SALVAR**.

Vá para o topo da página e clique no botão **Reconectar**, para realizar a autenticação. 
    
.. figure:: WiFi-AX2S-Internet-02.png
    :scale: 60 %
    :align: center
    :alt: WiFi-AX2S Internet Step2

Se tudo estiver certo, será estabelecido a conexão WAN de Internet Banda Larga.

.. figure:: WiFi-AX2S-Internet-03.png
    :scale: 60 %
    :align: center
    :alt: WiFi-AX2S Internet Step3

**IMPORTANTE:** Se você não utiliza Internet Fibra Live TIM, então será necessário entrar em contato com seu provedor de serviços de Internet/Banda Larga para obter Conta e Senha de Banda Larga para o tipo de conexão PPoE. É válido lembrar que dependendo do tipo de Provedor, pode usar uma conexão diferente do PPoE, como por exemplo, em alguns ISPs de TV a Cabo podem utilizar outro modo de acesso à Internet com **DHCP - Obter IP automaticamente**.

.. note:: Não é necessário selecionar a vlan 100 e Service Name: vdsl (opcional), podem pular essas configurações. Mas tenha atenção na hora de configurar o IP de Gerência em ambos equipamentos - "Roteador e Modem". O default gateway para a Rede 192.168.1.0/24 é oculto e fixo em 192.168.1.1. Lembrando que para o Router HUAWEI WiFi AX2S seu IP é 192.168.1.253 e para o Modem MediaAccess TG788v v3 seu IP é 192.168.1.254.

=======================================================================

Uhhhhuuuuuuuuu!  😛 Meus parabéns ^^  agora vc pode usufruir do padrão de transmissão: **802.11ax**/ac/a/n 2 × 2 e 802.11b/g/n 2 × 2, MU-MIMO, OFDMA, ATF.
Com uma taxa de transmissão sem fio de até 1500 Mbps, banda de 5 GHz que suporta Wi-Fi 6 e taxa de transmissão máxima de 1201 Mbps1, jogos, streaming e downloads nunca funcionaram tão bem. Para ficar melhor ainda pessoal, quem puder já podem adquirir o Huawei WiFi AX3 Pro Plus 3000 mbps Quad-Core pelo site oficial:: `www.huawei.com <https://consumer.huawei.com/br/routers/ax3-pro/>`_, melhoria contínua sempre, vamo que vamo 😛!
