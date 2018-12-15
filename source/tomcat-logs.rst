Logs
====


Tomcat
~~~~~~


Arquivo ``etc/rsyslog.d/tomcat.conf``::

    programname,contains,"server" /var/log/tomcat/naftalinas.out
    programname,contains,"server" ~

Reiniciar serviço::

    systemctl restart rsyslog
