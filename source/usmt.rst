User State Migration Tool 'USMT'
--------------------------------

.. note:: A Ferramenta de Migração de Estado do Usuário é um programa utilitário de linha de comando desenvolvido pela Microsoft que permite que os usuários se sintam à vontade com linguagens de script para transferir arquivos e configurações entre PCs com Windows.

Procedimento
""""""""""""

Instalação::
	
Baixe o adksetup.exe pelo site: `docs.microsoft.com <https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install>`_

**Observação:** Nesse exemplo usaremos windows adk, para sistema windows 10 professional.
 
Após a execução do **adksetup.exe** vamos escolher a segunda opção: **download features for installation on a separate computer.**

.. note:: Escolha o local de instalação em **C:\\ADK-Offline**, e após o seu download, copie esta pasta para um pendrive, pois iremos usá-la para instalar em máquinas sem necessidade de conexão com a internet, manteremos tudo no modo offline.


Instalar o USMT
"""""""""""""""

Entre na pasta ADK-Offline e execute o adksetup.exe e coloque no local de instalação o caminho::

   C:\ADK

Escolha a primeira opção e em seguida selecione a ferramenta no caso, apenas o usmt.

Iniciar o processo do usmt na máquina alvo
""""""""""""""""""""""""""""""""""""""""""

.. note:: O teste à seguir foi feito no meu Oracle VM VirtualBOX

Mapear a pasta compartilhada o qual irar armazenar o perfil do usuário::
 
   net use e: \\vboxsrv\andre

   mkdir e:\Downloads\backup_user

Iniciar o processo do usmt::

   cd "C:\ADK\Assessment and Deployment Kit\User State Migration Tool\amd64"

   scanstate.exe e:\Downloads\backup_user /ue:*\* /ui:*\andre /i:miguser.xml /i:migapp.xml /o /c /nocompress

   ou 
 
   scanstate.exe \\vboxsrv\andre\Downloads\backup_user /ue:*\* /ui:*\andre /i:miguser.xml /i:migapp.xml /o /c /nocompress
  
.. figure:: capture-1-scanstate.png
    :scale: 60 %
    :align: center
    :alt: Snipping Tool Capture Scanstate

**OBS**: Isso Irá criar uma pasta nova chamada **USMT** em e:\\Downloads\\backup_user.
  
Restore do perfil em outra máquina, instale o ADK (não esqueça de selecionar apenas o usmt)::

   cd "C:\ADK\Assessment and Deployment Kit\User State Migration Tool\amd64"

   loadstate e:\Downloads\backup_user /ue:*\* /ui:*\andre /i:miguser.xml /i:migapp.xml /nocompress

   ou 

   loadstate.exe \\vboxsrv\andre\Downloads\backup_user\USMT\ /ue:*\* /ui:*\andre /i:miguser.xml /i:migapp.xml /nocompress

.. figure:: capture-2-loadstate.png
    :scale: 60 %
    :align: center
    :alt: Snipping Tool Capture Loadstate


Faça um logon com o usuário andre e veja se tudo está ok (email, imagens, documentos, favoritos, icones, atalhos e etc..).

Consideracoes Finais
--------------------

No windows 8.1 professional, foi necessário reconfigurar o wallpaper e a foto do login do usuário... mas do resto, como ícones e área de trabalho estavam OK! bem como emails, docs e images.

Caso a máquina esteja ingressado num Domain, tanto a máquina alvo como a de destino, devem estar sempre no mesmo domain ou grupo de trabalho, antes de se iniciar o restore do loadstate do perfil do usuário!

=====================
USMT com Criptografia
=====================

.. note:: Caso queira utilizar a migração com mais segurança, podemos usar a criptografia.

Veja um exemplo abaixo
----------------------

Migração completa do perfil do usuário chamado **lidiane**, no windows 10 x64 (hostname notedell). iremos migrá-la para outra máquina, tbm com windows 10 x64 e (hostname note-sam).


Realizar a cópia critografada do perfil completo da máquina.
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

1. Instale o software **adksetup.exe** em ambas as máquinas!

(notedell):: 

   elevate cmd administrator

   cd C:\User State Migration Tool\arm64

   conecte o hd externo (e:) ou se estiver em rede mapeia para o seu fileserver:

   mkdir e:\notelidiane\perfilmigration

   scanstate.exe e:\notelidiane\perfilmigration /ue:*\* /ui:*\Lidia /i:migapp.xml /i:miguser.xml /i:migdocs.xml /v:5 /vsc /encrypt /key:delldalidiane10


2. Restaurar o perfil em outra máquina.

(note-sam)::

   elevate cmd administrator

   cd C:\User State Migration Tool\arm64

   conecte o hd externo (e:) ou se estiver em rede mapeia para o seu fileserver:

   loadstate.exe e:\notelidiane\perfilmigration /ue:*\* /ui:*\Lidia /i:migapp.xml /i:miguser.xml /i:migdocs.xml /v:13 /lae /lac:12345678 /decrypt /key:delldalidiane10


3. Desconecte o hd externo e reinicie o computador. Faça um logon com o usuário lidiane e veja se tudo está ok (email, imagens, documentos, favoritos, icones, atalhos e etc..)

