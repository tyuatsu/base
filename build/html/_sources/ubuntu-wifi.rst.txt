Resolvendo problema de conexão Wi-Fi no Ubuntu
----------------------------------------------

.. note:: Se você informa login e senha corretos e o Ubuntu não consegue estabelecer a conexão Wi-Fi, experimente executar os passos seguintes.

Abra uma janela do Terminal/Shell do Ubuntu.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Digite o comando abaixo e tecle **ENTER**::

    sudo nano /etc/NetworkManager/NetworkManager.conf

Se solicitada, digite a senha do usuário root e tecle **ENTER**.

Se tudo der certo até aqui, será exibido um conteúdo semelhante a este abaixo::

    [main]
    plugins=ifupdown,keyfile

    [ifupdown]
    managed=false
    
Pressione SETA-PARA-BAIXO para descer até o final do arquivo e adicione o seguinte conteúdo::

    [device]
    wifi.scan-rand-mac-address=no
    
Pressione **CTRL+O** e tecle **ENTER** para gravar a alteração.

Pressione **CTRL+X** para sair do editor de texto.

Reinicie o Linux e tente conectar novamente.
