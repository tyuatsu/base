Como converter um arquivo HTML para PDF via linha de comando?
-------------------------------------------------------------

.. note:: Existem muitas formas para se converter um arquivo HTML para o formato PDF, mas a maioria das soluções requer a intervenção do usuário, o que nem sempre é desejável.

Se você necessita de uma conversão automatizada, com um mínimo de intervenção do usuário, então o programa wkhtmltopdf pode ser a solução. Este programa pode ser executado a partir do ambiente de comandos (shell) do sistema operacional (Windows, Linux, etc.), via arquivo de lotes (.bat, .cmd, .sh, etc.), ou a partir de outro programa feito especificamente para este propósito.

Após instalar o programa wkhtmltopdf em seu computador, você poderá executar um comando simples como este abaixo para gerar um arquivo PDF a partir de um arquivo HTML qualquer.

"C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe" C:\Pasta\Exemplo.html C:\Pasta\Exemplo.pdf
A linha de comando acima considera que o programa foi instalado sob a pasta C:\Program Files\, mas você deve conferir o local exato de instalação e fazer os ajustes necessários.

O conteúdo HTML pode estar em um arquivo local ou mesmo em uma página da Internet, como mostra o comando a seguir:

wkhtmltopdf http://google.com.br google.pdf
Note que neste último exemplo não foi informado o endereço completo do programa wkhtmltopdf. Por este motivo a linha de comando acima funcionará somente se o sistema operacional estiver configurado para procurar executáveis na pasta deste programa ou se este comando for executado a partir da pasta do próprio programa.

Junto com o programa wkhtmltopdf é instalado também o programa wkhtmltoimage.exe, que pode ser usado para converter HTML para diversos formatos de imagem.

Como converter vídeo do formato WebM para AVI, WMV, MP4, MPEG, etc?
WebM é um formato de vídeo, aberto e livre de royalties, desenvolvido para fornecer vídeo de alta qualidade, em desenvolvimento pela Google.

Embora seja um ótimo formato de vídeo, talvez você precise converter um vídeo do formato WebM para um outro formato. Para esta tarefa você pode contar com o X Media Recode, um ótimo aplicativo gratuito (freeware) que você pode baixar do site: www.xmedia-recode.de/en/download.html

Os formatos suportados são estes: 3G2, AAC, AC3, ADX, AIFF, AMR, APE, ASF, AVI, AVISynth, AU, Blu-ray, DVD, DIVX, DTS, E-AC3, FLAC, F4V, FLV, H261, H263, H264, IVF, M2TS, MTS, M1V, M2V, M3U, M3U8, M4A, M4P, M4V, MKA, MKV, MMF, MP2, MP3, MP4, MP4V, MPE, MPEG-1, MPEG-2, MPEG-4, MOV, QT, QCP, OGG, OGM, OGV, PVA, REC, RM, RMVB, SVCD, SWF, SPX, THP, TS, TRP, TP0, VCD, VOB, VRO, WebM, WebA, WMA, WMV, WPL, WTV, Y4M e YUV.

Com uma enorme variedade de formatos de entrada e saída, este programa pode ser considerado uma ótima alternativa ao Format Factory, Any Video Converter, RMVB Converter e outros.

