Como Criar Midia de Instalação do Windows 10
==================================================
com boot pelo Flash Disk USB (Pendrive) no Linux

.. note:: Atenção! Todos os passoas à seguir, foram feitos com a distro Linux: "Fedora".

Existem muitos tutorias já prontos para o Ubuntu/Debian por aí na net... só pesquisar que você acham. Mas como não encontrei nada parecido para o Fedora Linux, decidi então postar essa dica aqui.

.. _Siga os passos abaixo:

1. Baixe a imagem .iso (DVD de instalação)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    `https://www.microsoft.com/en-us/software-download/ <https://www.microsoft.com/en-us/software-download/>`_

Eu fiz o download da versão PT-BR 64 bits do windows 10: Win10_1803_BrazilianPortuguese_x64.iso (4.4GB).

Vou usá-la para reinstalar todo o Sistema Operacional no meu notebook, que infelizmente sofreu falhas severas nos seus drivers (tudo corrompido), que nem o restrui.exe e systemreset.exe da vida conseguem dar conta do problema. Isso aconteceu depois de uma parada crítica de energia, no meio de uma atualização do windows update. - E não sei porque cargas d'água ele corrompeu os drivers de video e rede..., algum cazto do universo durante esse processo. Eu sei lá. Tem coisas que só a Microsoft faz por você!

1.2. Preparar o PenDrive
""""""""""""""""""""""""

Aqui eu vou usar um pendrive de 32GB, mas também pode usar um de 8GB se ele estiver disponível.

Esse flash disk usb de 32GB, eu costumo utilizá-lo para boot do "fedora 28 live usb". Mas como preciso formatar urgente o meu notebook com windows 10, então vou usá-lo, sem dó e nem piedade! 

**CUIDADO!** O processo à seguir irá destruir todos dados do seu Pendrive, muita calma nessa hora.

Insira o pendrive no seu conector USB e execute no Shell::

    lsblk -f

O dispositivo do meu drive usb é: "/dev/sdb"::

    sudo dd count=1 bs=512 if=/dev/zero of=/dev/sdb 
    sudo umount /run/media/lazaro/Fedora-WS-Live-26-1-5
    sudo mkfs.vfat -n REMOVABLE -I -F32 /dev/sdb

.. note:: Se tiver na mão, pode usar o gparted para formatar o seu pendrive.

2. Vamos baixar o famoso aplicativo "WoeUSB-ng", esse eu garanto que funciona
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. figure:: WoeUSB-ng.png
    :scale: 60 %
    :align: center
    :alt: WoeUSB-ng

Antes precisa instalar algumas dependências para satisfazer os requisitos do software WoeUSB::

    sudo dnf install git
    sudo dnf install p7zip
    sudo dnf install p7zip-plugins
    sudo dnf install python3-pip
    sudo dnf install python3-wxpython4
    
ou se quiser pode rodar tudo num só comando, assim::

    sudo dnf install git p7zip p7zip-plugins python3-pip python3-wxpython4

2.1. Pode fazer o download do source no Git do "WoeUSB-ng" ou instalar via pip3 (gerenciador de pacotes Python) - testado com Fedora 41
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

    `Windows USB stick installer WoeUSB-ng <https://github.com/WoeUSB/WoeUSB-ng/>`_

rode os seguintes comandos::

    sudo pip3 install WoeUSB-ng
    
se quiser desinstalar::
    
    sudo pip3 uninstall WoeUSB-ng
    sudo rm /usr/share/icons/WoeUSB-ng/icon.ico \
    /usr/share/applications/WoeUSB-ng.desktop \
    /usr/local/bin/woeusbgui
    sudo rmdir /usr/share/icons/WoeUSB-ng/

Nesse momento você pode carregar o WoeUSB-ng, execute o comando direto no shell::

    sudo woeusb --target-filesystem NTFS --device Win10_20H2_v2_BR_PT_x64.iso /dev/sdb

unmounting and removing "/media/woeusb_target_1528352659_2304"...
You may now safely detach the target device
Done 
The target device should be bootable now

Agora remova o dispositivo::

    sudo umount /run/media/lazaro/Windows\ USB/

Pronto! Agorá você já pode pegar esse Pendrive e sair formatando tudo com o Windows 10.

3. Observações
^^^^^^^^^^^^^^

3.1. Sobre o boot pelo pendrive
"""""""""""""""""""""""""""""""

Ele costuma demorar bastante até aparecer à tela de instalação do Windows 10.
Fora isso, o instalador rodou normal e todo o processo de instalação foi suave na nave.




