SUPAIDAMAN SSH TUNNEL
---------------------

.. note:: É raro precisarmos apelar por um singelo túnel para passar todo nosso tráfego web através de algum servidor externo. Pode parecer ridículo, mas nós, meros mortais usuário, costumamos fazer isso na surdina, quando nos deparamos ou temos algumas limitações do provedor de internet e também quando necessitamos urgentemente de entrar em páginas que normalmente não temos acesso. Devido a um bloqueio de proxy server ou webfilter de algum firewall. Neste breve tutorial, mostrarei como fazer isso raidamente usando apenas o PuTTY (SSH) e o navegador Chrome.

.. figure:: supaidaman.jpg
    :scale: 60 %
    :align: center
    :alt: Supaidaman
    
**Baixe a versão mais atualizada do putty.exe (free SSH and Telnet client)** - pelo site oficial:: `www.putty.org <https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>`_     
    
Configuração básica do Putty
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Antes de mais nada, segue o comando para carregar o server ssh, aqui eu usarei o fedora linux como exemplo::
 
    # systemctl start sshd

.. figure:: putty-05.png
    :scale: 80 %
    :align: center
    :alt: service sshd running

Abra o **putty.exe** e entre com o IP ou a URL do seu servidor remoto ou desktop doméstico remoto, por exemplo:: **supaidaman.dynu.net** sempre resolverá para mim p/ IP dinâmico aleatório 179.55.x.x na porta 22. Esse IP é entregue pelo meu provedor de internet.

.. figure:: putty-01.png
    :scale: 80 %
    :align: center
    :alt: putty.exe

No lado esquerdo do painel do putty, vá p/ Connection → SSH → Tunnels

Agora siga as etapas:: selecione **Dynamic**, preencha a porta de origem (source port) por exemplo 9999, mas caso queira pode-se usar qualquer outra porta livre e depois click no botão **Add button**

Na figura abaixo irá aparecer listado com o formato **D{PORT_NUMBER}**

.. figure:: putty-02.png
    :scale: 80 %
    :align: center
    :alt: putty.exe

Agora para finalizar Click no botão **Open**

Em seguida pode logar no seu servidor como de costume

.. figure:: putty-03.png
    :scale: 80 %
    :align: center
    :alt: putty.exe
    
Abra o **cmd** e com o comando netstat veja se o seu SSH está em modo listen na porta 9999 **(127.0.0.1:9999)**

.. figure:: putty-04.png
    :scale: 80 %
    :align: center
    :alt: putty.exe

Usando um túnel no Google Chrome
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Chegou a hora meus caros, na parte crucial da configuração. 😛 Vamos ter que configurar o proxy no Chrome. Não é tão intuitivo igual o Firefox ou IE, porque quando queremos configurá-lo no Chrome pela GUI dele, infelizmente ele nos redireciona para o maldito Painel de Controle do Windows. Como não desejamos alterar a configuração de todo o PC, mas apenas do navegador Chrome. 

Por isso se faz necessário proceder de outra maneira, através do **command prompt** cmd::

    "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --proxy-server="socks5://127.0.0.1:9999"

Se o comando não funcionar, pode ser devido seu google chrome estar instalado em outro local diferente:: **C:\Program Files\Google\Chrome**

Se tudo der certo o chrome abrirá e você pode abrir o endereço **http://httpbin.org/ip** e ver seu ip::

  {
  "origin": "179.55.170.40"
  }

Aqui por exemplo abriu com a resposta do IP usado pelo meu provedor de internet. Pronto vc pode navegar livremente via tunel ssh.

.. note:: Certifique-se de ter fornecido o número de porta correto. Lembrem-se aqui eu usei 9999 porém você pode usar outra que quiser, MAS É IMPORTANTE sempre usar a mesmo número de porta nas configurações do PuTTY e do Chrome.
