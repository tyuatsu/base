Security Auditoria 
------------------

.. figure:: fedora.png
    :scale: 40 %
    :align: center
    :alt: fedora

.. note:: Auditoria para Ambientes com Sistema Operacional Linux (Fedora/RHEL/CentOS) 


Audit log::

    [root@otoha ~]# uname -r
    5.4.20-200.fc31.x86_64

    [root@otoha ~]# hostnamectl
       Static hostname: otoha.argoproxy.br
             Icon name: computer-laptop
               Chassis: laptop
            Machine ID: 698253ff9d8c47248d1fdeef220cbca1
               Boot ID: 893b065b0d1945978cfc1bdd0f3af677
      Operating System: Fedora 31 (Workstation Edition)
           CPE OS Name: cpe:/o:fedoraproject:fedora:31
                Kernel: Linux 5.4.20-200.fc31.x86_64
          Architecture: x86-64
 

    [root@otoha ~]# systemctl status auditd -l
    ● auditd.service - Security Auditing Service
       Loaded: loaded (/usr/lib/systemd/system/auditd.service; enabled; vendor preset: enabled)
       Active: active (running) since Wed 2020-03-18 10:56:54 -03; 2 days ago
         Docs: man:auditd(8)
               https://github.com/linux-audit/audit-documentation
      Process: 812 ExecStart=/sbin/auditd (code=exited, status=0/SUCCESS)
      Process: 821 ExecStartPost=/sbin/augenrules --load (code=exited, status=0/SUCCESS)
     Main PID: 814 (auditd)
        Tasks: 4 (limit: 9386)
       Memory: 4.2M
          CPU: 182ms
       CGroup: /system.slice/auditd.service
               ├─814 /sbin/auditd
               └─816 /usr/sbin/sedispatch


aureport -x --summary::

    [root@otoha ~]# aureport -x --summary
    [sudo] password for andre: 

    Executable Summary Report
    =================================
    total  file
    =================================
    19457  /usr/lib/systemd/systemd
    538  /usr/lib/systemd/systemd-update-utmp
    410  /usr/bin/sudo
    122  /usr/sbin/runuser
    69  /usr/bin/pkexec
    62  /usr/libexec/gdm-session-worker
    62  /usr/sbin/sshd
    52  /usr/bin/su
    29  /usr/sbin/groupadd
    15  /usr/sbin/useradd
    10  /usr/sbin/usermod
    4  /usr/lib/polkit-1/polkit-agent-helper-1
    3  /usr/libexec/packagekitd
    3  /usr/bin/gnome-shell
    3  /usr/sbin/vsftpd
    2  /usr/bin/passwd
    1  /usr/libexec/gnome-initial-setup
    1  /usr/bin/python2.7
    1  /usr/sbin/userdel
    1  /usr/lib64/virtualbox/VirtualBox
    1  /usr/libexec/gdm-wayland-session
    1  /usr/libexec/gdm-x-session


aureport --failed::

    [root@otoha ~]# aureport --failed

    Failed Summary Report
    ======================
    Range of time in logs: 07/16/2017 17:28:12.032 - 03/18/2020 10:57:19.131
    Selected time for report: 07/16/2017 17:28:12 - 03/18/2020 10:57:19.131
    Number of changes in configuration: 0
    Number of changes to accounts, groups, or roles: 8
    Number of logins: 0
    Number of failed logins: 1
    Number of authentications: 0
    Number of failed authentications: 1
    Number of users: 2
    Number of terminals: 3
    Number of host names: 2
    Number of executables: 4
    Number of commands: 14
    Number of files: 1
    Number of AVC's: 13
    Number of MAC events: 0
    Number of failed syscalls: 0
    Number of anomaly events: 0
    Number of responses to anomaly events: 0
    Number of crypto events: 0
    Number of integrity events: 0
    Number of virt events: 0
    Number of keys: 0
    Number of process IDs: 23
    Number of events: 232

.. note:: One of the main differences between ntpd and chronyd is in the algorithms used to control the computer’s clock. Things chronyd can do better than ntpd!

chrony Suite::
    
    [root@otoha ~]# date -R
    Fri, 20 Mar 2020 21:34:59 -0300

    [root@otoha ~]# chronyc tracking
    Reference ID    : C8A007C5 (gps.jd.ntp.br)
    Stratum         : 2
    Ref time (UTC)  : Sat Mar 21 00:10:24 2020
    System time     : 0.002918260 seconds slow of NTP time
    Last offset     : -0.003419104 seconds
    RMS offset      : 0.014937137 seconds
    Frequency       : 23.267 ppm slow
    Residual freq   : -0.140 ppm
    Skew            : 3.042 ppm
    Root delay      : 0.011531984 seconds
    Root dispersion : 0.036170207 seconds
    Update interval : 3353.7 seconds
    Leap status     : Normal


    [root@otoha ~]# chronyc sources
    210 Number of sources = 11
    MS Name/IP address         Stratum Poll Reach LastRx Last sample               
    ===============================================================================
    ^+ a.st1.ntp.br                  1   7   377    18  +1330us[+1330us] +/- 8656us
    ^? a.st1.ntp.br                  0   6     0     -     +0ns[   +0ns] +/-    0ns
    ^- c.st1.ntp.br                  1   9   377   424  -9757us[-5882us] +/-   28ms
    ^- d.st1.ntp.br                  1   7   377    39  +5438us[+5438us] +/-   13ms
    ^+ a.ntp.br                      2   9   377   478   -204us[+3648us] +/-   14ms
    ^? a.ntp.br                      0   6     0     -     +0ns[   +0ns] +/-    0ns
    ^- b.ntp.br                      2   9   377    33  +1653us[+1653us] +/-   51ms
    ^? b.ntp.br                      0   6     0     -     +0ns[   +0ns] +/-    0ns
    ^- c.ntp.br                      2  10   177   680  +3034us[+6798us] +/-   35ms
    ^* gps.jd.ntp.br                 1   8   377   106  +2418us[+6433us] +/- 6966us
    ^? gps.jd.ntp.br                 0   6     0     -     +0ns[   +0ns] +/-    0ns


    [root@otoha ~]# systemctl status chronyd
    ● chronyd.service - NTP client/server
       Loaded: loaded (/usr/lib/systemd/system/chronyd.service; enabled; vendor preset: enabled)
       Active: active (running) since Wed 2020-03-18 10:56:56 -03; 2 days ago
         Docs: man:chronyd(8)
               man:chrony.conf(5)
      Process: 868 ExecStart=/usr/sbin/chronyd $OPTIONS (code=exited, status=0/SUCCESS)
      Process: 952 ExecStartPost=/usr/libexec/chrony-helper update-daemon (code=exited, status=0/SUCCESS)
     Main PID: 950 (chronyd)
        Tasks: 1 (limit: 9386)
       Memory: 2.9M
          CPU: 663ms
       CGroup: /system.slice/chronyd.service
               └─950 /usr/sbin/chronyd


SSH Restrito::

    Port 22
    #AddressFamily any
    ListenAddress 0.0.0.0
    #ListenAddress ::

    #HostKey /etc/ssh/ssh_host_rsa_key
    #HostKey /etc/ssh/ssh_host_dsa_key
    #HostKey /etc/ssh/ssh_host_ecdsa_key
    #HostKey /etc/ssh/ssh_host_ed25519_key

    # Ciphers and keying
    #RekeyLimit default none

    # Logging
    #SyslogFacility AUTH
    SyslogFacility AUTHPRIV
    LogLevel INFO

    # Authentication:

    #LoginGraceTime 2m
    PermitRootLogin no
    #StrictModes yes
    MaxAuthTries 3
    #MaxSessions 10

    #PubkeyAuthentication yes

    # The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
    # but this is overridden so installations will only check .ssh/authorized_keys
    AuthorizedKeysFile	    .ssh/authorized_keys

    #AuthorizedPrincipalsFile none

    #AuthorizedKeysCommand none
    #AuthorizedKeysCommandUser nobody

    # For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
    #HostbasedAuthentication no
    # Change to yes if you don't trust ~/.ssh/known_hosts for
    # HostbasedAuthentication
    #IgnoreUserKnownHosts no
    # Don't read the user's ~/.rhosts and ~/.shosts files
    #IgnoreRhosts yes

    # To disable tunneled clear text passwords, change to no here!
    #PasswordAuthentication yes
    #PermitEmptyPasswords no
    PasswordAuthentication yes
    

Login SSH Nominal::    
    
    [root@otoha ~]# cat /etc/ssh/sshd_config | grep -i allow
    # be allowed through the ChallengeResponseAuthentication and
    #AllowAgentForwarding yes
    #AllowTcpForwarding yes
    AllowUsers andre
    

Protocolo IPV4 instalado e configurado e IPV6 desabilitado::
    
    [root@otoha ~]# cat /etc/sysconfig/network-scripts/ifcfg-Aliens 
    ESSID=Aliens
    MODE=Managed
    KEY_MGMT=WPA-PSK
    MAC_ADDRESS_RANDOMIZATION=default
    TYPE=Wireless
    PROXY_METHOD=none
    BROWSER_ONLY=no
    BOOTPROTO=dhcp
    DEFROUTE=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    NAME=Aliens
    UUID=03324a37-1baf-478c-9e63-4d685d4ea370
    DEVICE=wlp2s0
    ONBOOT=yes
    DNS1=192.168.1.18
    DNS2=208.62.220.220
    PEERDNS=no
    ZONE=FedoraWorkstation



Regras de Firewall::

    [root@otoha ~]# sudo firewall-cmd --state
    running

    [root@otoha ~]# sudo firewall-cmd --get-active-zones
    FedoraWorkstation
      interfaces: eth0 wlp2s0
    libvirt
      interfaces: virbr0

    [root@otoha ~]# sudo firewall-cmd --get-default-zone
    FedoraWorkstation

    [root@otoha ~]# sudo firewall-cmd --list-services
    Shoutcast dhcp dhcpv6 dhcpv6-client dns docker-registry ftp http https imap imaps ipp ipp-client mdns nfs ntp openvpn pptp samba samba-client ssh

    [root@otoha ~]# sudo firewall-cmd --list-ports --zone=FedoraWorkstation
    1025-65535/udp 1025-65535/tcp 15567/udp 22000/udp 1900/udp 8895/tcp 54807/tcp 9000/tcp

    [root@otoha ~]# sudo firewall-cmd --zone=FedoraWorkstation --list-all
    FedoraWorkstation (active)
      target: default
      icmp-block-inversion: no
      interfaces: eth0 wlp2s0
      sources: 
      services: Shoutcast dhcp dhcpv6 dhcpv6-client dns docker-registry ftp http https imap imaps ipp ipp-client mdns nfs ntp openvpn pptp samba samba-client ssh
      ports: 1025-65535/udp 1025-65535/tcp 15567/udp 22000/udp 1900/udp 8895/tcp 54807/tcp 9000/tcp
      protocols: 
      masquerade: no
      forward-ports: 
      source-ports: 
      icmp-blocks: 
      rich rules: 

Hard Disk Analyses::

    [root@otoha ~]# sudo hdparm -C /dev/sda
    /dev/sda:
     drive state is:  active/idle

    [root@otoha ~]# sudo hdparm -tT /dev/sda
    /dev/sda:
     Timing cached reads:   7316 MB in  2.00 seconds = 3664.79 MB/sec
     Timing buffered disk reads:  88 MB in  3.00 seconds =  29.33 MB/sec

    [root@otoha ~]# sudo hdparm -I /dev/sda
    /dev/sda:
    ATA device, with non-removable media
     	Model Number:       SAMSUNG HM500JI                         
	Serial Number:      S2NVJ56B704117      
	Firmware Revision:  2AC101C4
	Transport:          Serial, ATA8-AST, SATA 1.0a, SATA II Extensions, SATA Rev 2.5, SATA Rev 2.6
    Standards:
	Used: unknown (minor revision code 0x0028) 
	Supported: 8 7 6 5 
	Likely used: 8
    Configuration:
	Logical		max	current
	cylinders	16383	16383
	heads		16	16
	sectors/track	63	63
	--
	CHS current addressable sectors:    16514064
	LBA    user addressable sectors:   268435455
	LBA48  user addressable sectors:   976773168
	Logical  Sector size:                   512 bytes
	Physical Sector size:                   512 bytes
	device size with M = 1024*1024:      476940 MBytes
	device size with M = 1000*1000:      500107 MBytes (500 GB)
	cache/buffer size  = 8192 KBytes
	Form Factor: 2.5 inch
    Capabilities:
	LBA, IORDY(can be disabled)
	Queue depth: 32
	Standby timer values: spec'd by Standard, no device specific minimum
	R/W multiple sector transfer: Max = 16	Current = ?
	Advanced power management level: disabled
	Recommended acoustic management value: 254, current value: 0
	DMA: mdma0 mdma1 mdma2 udma0 udma1 udma2 udma3 udma4 udma5 *udma6 
	     Cycle time: min=120ns recommended=120ns
	PIO: pio0 pio1 pio2 pio3 pio4 
	     Cycle time: no flow control=120ns  IORDY flow control=120ns
    Commands/features:
	Enabled	Supported:
	   *	SMART feature set
	    	Security Mode feature set
	   *	Power Management feature set
	   *	Write cache
	   *	Look-ahead
	   *	Host Protected Area feature set
	   *	WRITE_BUFFER command
	   *	READ_BUFFER command
	   *	NOP cmd
	   *	DOWNLOAD_MICROCODE
	    	Advanced Power Management feature set
	    	Power-Up In Standby feature set
	   *	SET_FEATURES required to spinup after power up
	    	SET_MAX security extension
	    	Automatic Acoustic Management feature set
	   *	48-bit Address feature set
	   *	Device Configuration Overlay feature set
	   *	Mandatory FLUSH_CACHE
	   *	FLUSH_CACHE_EXT
	   *	SMART error logging
	   *	SMART self-test
	   *	General Purpose Logging feature set
	   *	64-bit World wide name
	   *	IDLE_IMMEDIATE with UNLOAD
	   *	WRITE_UNCORRECTABLE_EXT command
	   *	{READ,WRITE}_DMA_EXT_GPL commands
	   *	Segmented DOWNLOAD_MICROCODE
	   *	Gen1 signaling speed (1.5Gb/s)
	   *	Gen2 signaling speed (3.0Gb/s)
	   *	Native Command Queueing (NCQ)
	   *	Host-initiated interface power management
	   *	Phy event counters
	   *	Idle-Unload when NCQ is active
	   *	NCQ priority information
	   *	DMA Setup Auto-Activate optimization
	   *	Device-initiated interface power management
	   *	Software settings preservation
	   *	SMART Command Transport (SCT) feature set
	   *	SCT Read/Write Long (AC1), obsolete
	   *	SCT Write Same (AC2)
	   *	SCT Error Recovery Control (AC3)
	   *	SCT Features Control (AC4)
	   *	SCT Data Tables (AC5)
    Security: 
	Master password revision code = 65534
		supported
	not	enabled
	not	locked
	not	frozen
	not	expired: security count
		supported: enhanced erase
	130min for SECURITY ERASE UNIT. 130min for ENHANCED SECURITY ERASE UNIT.
    Logical Unit WWN Device Identifier: 50024e9400bff8ef
	NAA		: 5
	IEEE OUI	: 0024e9
	Unique ID	: 400bff8ef
    Checksum: correct

    [root@otoha ~]# udevadm info /sys/class/block/sda
    P: /devices/pci0000:00/0000:00:1f.2/ata1/host0/target0:0:0/0:0:0:0/block/sda
    N: sda
    L: 0
    S: disk/by-id/wwn-0x50024e9400bff8ef
    S: disk/by-path/pci-0000:00:1f.2-ata-1
    S: disk/by-id/ata-SAMSUNG_HM500JI_S2NVJ56B704117
    E: DEVPATH=/devices/pci0000:00/0000:00:1f.2/ata1/host0/target0:0:0/0:0:0:0/block/sda
    E: DEVNAME=/dev/sda
    E: DEVTYPE=disk
    E: MAJOR=8
    E: MINOR=0
    E: SUBSYSTEM=block
    E: USEC_INITIALIZED=33973013
    E: ID_ATA=1
    E: ID_TYPE=disk
    E: ID_BUS=ata
    E: ID_MODEL=SAMSUNG_HM500JI
    E: ID_MODEL_ENC=SAMSUNG\x20HM500JI\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
    E: ID_REVISION=2AC101C4
    E: ID_SERIAL=SAMSUNG_HM500JI_S2NVJ56B704117
    E: ID_SERIAL_SHORT=S2NVJ56B704117
    E: ID_ATA_WRITE_CACHE=1
    E: ID_ATA_WRITE_CACHE_ENABLED=1
    E: ID_ATA_FEATURE_SET_HPA=1
    E: ID_ATA_FEATURE_SET_HPA_ENABLED=1
    E: ID_ATA_FEATURE_SET_PM=1
    E: ID_ATA_FEATURE_SET_PM_ENABLED=1
    E: ID_ATA_FEATURE_SET_SECURITY=1
    E: ID_ATA_FEATURE_SET_SECURITY_ENABLED=0
    E: ID_ATA_FEATURE_SET_SECURITY_ERASE_UNIT_MIN=130
    E: ID_ATA_FEATURE_SET_SECURITY_ENHANCED_ERASE_UNIT_MIN=130
    E: ID_ATA_FEATURE_SET_SECURITY_FROZEN=1
    E: ID_ATA_FEATURE_SET_SMART=1
    E: ID_ATA_FEATURE_SET_SMART_ENABLED=1
    E: ID_ATA_FEATURE_SET_AAM=1
    E: ID_ATA_FEATURE_SET_AAM_ENABLED=0
    E: ID_ATA_FEATURE_SET_AAM_VENDOR_RECOMMENDED_VALUE=254
    E: ID_ATA_FEATURE_SET_AAM_CURRENT_VALUE=0
    E: ID_ATA_FEATURE_SET_PUIS=1
    E: ID_ATA_FEATURE_SET_PUIS_ENABLED=0
    E: ID_ATA_FEATURE_SET_APM=1
    E: ID_ATA_FEATURE_SET_APM_ENABLED=0
    E: ID_ATA_DOWNLOAD_MICROCODE=1
    E: ID_ATA_SATA=1
    E: ID_ATA_SATA_SIGNAL_RATE_GEN2=1
    E: ID_ATA_SATA_SIGNAL_RATE_GEN1=1
    E: ID_WWN=0x50024e9400bff8ef
    E: ID_WWN_WITH_EXTENSION=0x50024e9400bff8ef
    E: ID_PATH=pci-0000:00:1f.2-ata-1
    E: ID_PATH_TAG=pci-0000_00_1f_2-ata-1
    E: ID_PART_TABLE_UUID=d3818ed3
    E: ID_PART_TABLE_TYPE=dos
    E: UDISKS_PRESENTATION_NOPOLICY=0
    E: UDISKS_PARTITION_TABLE=1
    E: UDISKS_PARTITION_TABLE_SCHEME=mbr
    E: UDISKS_PARTITION_TABLE_COUNT=2
    E: UDISKS_ATA_SMART_IS_AVAILABLE=1
    E: DEVLINKS=/dev/disk/by-id/wwn-0x50024e9400bff8ef /dev/disk/by-path/pci-0000:00:1f.2-ata-1 /dev/disk/by-id/ata-SAMSUNG_HM500JI_S2NVJ56B704117
    E: TAGS=:systemd:

    [root@otoha ~]# sudo udisksctl info -d SAMSUNG_HM500JI_S2NVJ56B704117
    /org/freedesktop/UDisks2/drives/SAMSUNG_HM500JI_S2NVJ56B704117:
    org.freedesktop.UDisks2.Drive:
    CanPowerOff:                false
    Configuration:              {}
    ConnectionBus:              
    Ejectable:                  false
    Id:                         SAMSUNG-HM500JI-S2NVJ56B704117
    Media:                      
    MediaAvailable:             true
    MediaChangeDetected:        true
    MediaCompatibility:         
    MediaRemovable:             false
    Model:                      SAMSUNG HM500JI
    Optical:                    false
    OpticalBlank:               false
    OpticalNumAudioTracks:      0
    OpticalNumDataTracks:       0
    OpticalNumSessions:         0
    OpticalNumTracks:           0
    Removable:                  false
    Revision:                   2AC101C4
    RotationRate:               -1
    Seat:                       seat0
    Serial:                     S2NVJ56B704117
    SiblingId:                  
    Size:                       500107862016
    SortKey:                    00coldplug/00fixed/sd____a
    TimeDetected:               1584539804577748
    TimeMediaDetected:          1584539804577748
    Vendor:                     
    WWN:                        0x50024e9400bff8ef
    org.freedesktop.UDisks2.Drive.Ata:
    AamEnabled:                                 false
    AamSupported:                               true
    AamVendorRecommendedValue:                  254
    ApmEnabled:                                 false
    ApmSupported:                               true
    PmEnabled:                                  true
    PmSupported:                                true
    ReadLookaheadEnabled:                       true
    ReadLookaheadSupported:                     true
    SecurityEnhancedEraseUnitMinutes:           130
    SecurityEraseUnitMinutes:                   130
    SecurityFrozen:                             true
    SmartEnabled:                               true
    SmartFailing:                               false
    SmartNumAttributesFailedInThePast:          0
    SmartNumAttributesFailing:                  0
    SmartNumBadSectors:                         0
    SmartPowerOnSeconds:                        73742400
    SmartSelftestPercentRemaining:              0
    SmartSelftestStatus:                        success
    SmartSupported:                             true
    SmartTemperature:                           308.15000000000003
    SmartUpdated:                               1584917235
    WriteCacheEnabled:                          true
    WriteCacheSupported:                        true
    
    [root@otoha ~]# sudo skdump /dev/sda
    Device: sat16:/dev/sda
    Type: 16 Byte SCSI ATA SAT Passthru
    Size: 476940 MiB
    Model: [SAMSUNG HM500JI]
    Serial: [S2NVJ56B704117]
    Firmware: [2AC101C4]
    SMART Available: yes
    Quirks:
    Awake: yes
    SMART Disk Health Good: yes
    Off-line Data Collection Status: [Off-line data collection activity was never started.]
    Total Time To Complete Off-Line Data Collection: 8040 s
    Self-Test Execution Status: [The previous self-test routine completed without error or no self-test has ever been run.]
    Percent Self-Test Remaining: 0%
    Conveyance Self-Test Available: no
    Short/Extended Self-Test Available: yes
    Start Self-Test Available: yes
    Abort Self-Test Available: yes
    Short Self-Test Polling Time: 2 min
    Extended Self-Test Polling Time: 134 min
    Conveyance Self-Test Polling Time: 0 min
    Bad Sectors: 0 sectors
    Powered On: 2.3 years
    Power Cycles: 3371
    Average Powered On Per Power Cycle: 6.1 h
    Temperature: 34.0 C
    Attribute Parsing Verification: Good
    Overall Status: GOOD
    ID# Name                        Value Worst Thres Pretty      Raw            Type    Updates Good Good/Past
      1 raw-read-error-rate         100   100    51   2           0x020000000000 prefail online  yes  yes 
      2 throughput-performance      252   252     0   n/a         0x000000000000 old-age online  n/a  n/a 
      3 spin-up-time                 91    90    25   3.0 s       0xc20b00000000 prefail online  yes  yes 
      4 start-stop-count             97    97     0   3357        0x1d0d00000000 old-age online  n/a  n/a 
      5 reallocated-sector-count    252   252    10   0 sectors   0x000000000000 prefail online  yes  yes 
      7 seek-error-rate             252   252    51   0           0x000000000000 old-age online  yes  yes 
      8 seek-time-performance       252   252    15   n/a         0x000000000000 old-age offline yes  yes 
      9 power-on-hours              100   100     0   2.3 years   0x115000000000 old-age online  n/a  n/a 
     10 spin-retry-count            252   252    51   0           0x000000000000 old-age online  yes  yes 
     11 calibration-retry-count      99    99     0   1272        0xf80400000000 old-age online  n/a  n/a 
     12 power-cycle-count            97    97     0   3371        0x2b0d00000000 old-age online  n/a  n/a 
    191 g-sense-error-rate          100   100     0   831         0x3f0300000000 old-age online  n/a  n/a 
    192 power-off-retract-count     252   252     0   0           0x000000000000 old-age online  n/a  n/a 
    194 temperature-celsius-2        64    56     0   34.0 C      0x22000f002c00 old-age online  n/a  n/a 
    195 hardware-ecc-recovered      100   100     0   0           0x000000000000 old-age online  n/a  n/a 
    196 reallocated-event-count     252   252     0   0           0x000000000000 old-age online  n/a  n/a 
    197 current-pending-sector      252   252     0   0 sectors   0x000000000000 old-age online  n/a  n/a 
    198 offline-uncorrectable       252   252     0   0 sectors   0x000000000000 old-age offline n/a  n/a 
    199 udma-crc-error-count        200   200     0   0           0x000000000000 old-age online  n/a  n/a 
    200 multi-zone-error-rate       100   100     0   555         0x2b0200000000 old-age online  n/a  n/a 
    223 load-retry-count             99    99     0   1272        0xf80400000000 old-age online  n/a  n/a 
    225 load-cycle-count-2            1     1     0   2257323     0xab7122000000 old-age online  n/a  n/a 
    
    [root@otoha ~]# inxi -G
    Graphics:  Device-1: Intel 2nd Generation Core Processor Family Integrated Graphics driver: i915 v: kernel
               Display: x11 server: Fedora Project X.org 1.20.6 driver: i915 resolution: 1366x768~60Hz 
               OpenGL: renderer: Mesa DRI Intel Sandybridge Mobile v: 3.3 Mesa 19.2.8

    [root@otoha ~]# free -t
                  total        used        free      shared  buff/cache   available
    Mem:        8048200     1573928     2936012      426072     3538260     5742860
    Swap:       8208380           0     8208380
    Total:     16256580     1573928    11144392
               
    [root@otoha ~]# df -k
    Filesystem                    1K-blocks      Used Available Use% Mounted on
    devtmpfs                        4005040         0   4005040   0% /dev
    tmpfs                           4024100     74368   3949732   2% /dev/shm
    tmpfs                           4024100      1588   4022512   1% /run
    /dev/mapper/fedora_otoha-root  51343840  34031904  14674112  70% /
    tmpfs                           4024100      1536   4022564   1% /tmp
    /dev/mapper/fedora_otoha-home 418950048 172133744 225465088  44% /home
    /dev/sda1                        999320    143800    786708  16% /boot
    tmpfs                            804820       124    804696   1% /run/user/1000
    
    [root@otoha ~]# sudo fdisk -l
    Disk /dev/sda: 465.78 GiB, 500107862016 bytes, 976773168 sectors
    Disk model: SAMSUNG HM500JI 
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0xd3818ed3

    Device     Boot   Start       End   Sectors   Size Id Type
    /dev/sda1  *       2048   2099199   2097152     1G 83 Linux
    /dev/sda2       2099200 976773119 974673920 464.8G 8e Linux LVM

    Disk /dev/mapper/fedora_otoha-root: 50 GiB, 53687091200 bytes, 104857600 sectors
    Unit: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes

    Disk /dev/mapper/fedora_otoha-swap: 7.85 GiB, 8405385216 bytes, 16416768 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes

    Disk /dev/mapper/fedora_otoha-home: 406.95 GiB, 436937424896 bytes, 853393408 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    

