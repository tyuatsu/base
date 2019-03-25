Como verificar a velocidade da memória via linha de comando no Windows?
-----------------------------------------------------------------------

.. note:: Este comando foi testado no Windows 10, mas deve funcionar também em outras versões deste sistema operacional.

.. _No Windows existe uma forma simples para ver a velocidade (frequência) da memória do computador. Basta seguir os passos abaixo:

Abra o Prompt de Comando como Administrador.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Digite o comando abaixo:

    wmic memorychip get speed

Após digitar o comando, tecle **ENTER** e confira o resultado.
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Como ativar e desativar a conexão de rede via linha de comando no Windows?
--------------------------------------------------------------------------

Parar desativar ou ativar uma conexão de rede via linha de comando no Windows siga os passos abaixo:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Abra o Prompt de Comando como Administrador
"""""""""""""""""""""""""""""""""""""""""""

Para desativar digite este comando e tecle ENTER:

    wmic path win32_networkadapter where NetConnectionID="NomeDoAdaptador" call disable
    
Para ativar troque disable por enable, como segue:

    wmic path win32_networkadapter where NetConnectionID="NomeDoAdaptador" call enable
    
Para maior praticidade você pode salvar este comando em um arquivo de lote **(extensão .bat)**, que poderá ser executado (como Administrador) em vez de digitar o comando toda vez que precisa ativar ou desativar a rede.

