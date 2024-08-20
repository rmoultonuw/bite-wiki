[Rmoulton](User:Rmoulton "wikilink")
([talk](User_talk:Rmoulton "wikilink")) 15:53, 8 August 2024 (PDT)

## Hardware information

packing slip:
[Media:Pearson-head-node-20160212.pdf](Media:Pearson-head-node-20160212.pdf "wikilink")


UW tag \#: c1240842

SN \#: SM101004

## Problem

An Ubuntu server is needed for cluster test purposes.

## Solution

Install Ubuntu 24.04 LTS Server

## Notes

Among other references, installation and configuration steps are based
on:


[Ubuntu-24.04.cbs-divid](Ubuntu-24.04.cbs-divid "wikilink")

## Steps taken

### gather info

\- n/a -- in this case I didn't bother with this step -

### create DNS records

DNS records were created previously.


Reference: n/a

IP address assignments, for the record:


Public: sph-login0.hpc.sph.washington.edu - 10.171.246.240

Storage (aka "NFS"): sph-login0-s.hpc.sph.washington.edu - 10.66.54.240

Cluster (aka "compute"): sph-login0-c.hpc.sph.washington.edu -
10.66.62.240

### configure firewall

\- n/a -- UW Managed Firewall service will be used in this case -

### enable opensesame access

\- n/a -- UW Managed Firewall service will be used in this case -

### obtain installation media

Installation media was prepared previously.


Reference:
[Ubuntu-24.04.gac-pearson1#obtain_installation_media](Ubuntu-24.04.gac-pearson1#obtain_installation_media "wikilink")

### mount in rack

The system is in rack 6H07, slot ??.


<https://racktables.biostat.washington.edu/index.php?page=object&tab=edit&object_id=>??

### connect cables

Four ethernet cables and dual power cables were connected previously.

I temporarily connected mobile-KVM-cart cables.

### disable nagios checks

\- n/a -

### reboot system

I inserted the Ubuntu USB drive, then I checked for current login
sessions ...

`w`

output:

... and proceeded to reboot the server.

`shutdown -r +5`

output:

### configure RAID

\- n/a -- his system doesn't have a RAID controller -

### adjust bios settings

BIOS settings were adjusted previously.

### proceed with initial Ubuntu setup

The interactive Ubuntu Server installation utility loaded. I entered
settings as follows.

#### language

At the Language screen, I selected ...


English

... and hit \[Enter\]

At the Keyboard screen, I left the default settings unchanged:


Layout: English (US)

Variant: English (US)

I hit \[tab\] a few times to move to the bottom of the screen and select
...


\[Done\]

... then I hit \[Enter\]

#### type

At the type-of-installation screen, I selected ...


Ubuntu Server

I hit \[tab\] to move to the bottom of the screen and select ...


\[Done\]

... then I hit \[Enter\]

#### network connections

Note: For future reference, at this point it's possible to set up bonded
interfaces by selecting:


\[Create Bond\]

At the network connections screen, I selected adapter 'eno1' and entered
the appropriate settings for the public interface:


Subnet: 128.208.192.128/25

Address: 128.208.192.141

Gateway: 128.208.192.131

Name Servers: 128.95.120.1, 128.95.112.1

Search Domains: cbs.biostat.washington.edu, biostat.washington.edu

I tabbed to select ...


\[Save\]

... and hit \[Enter\]

I selected adapter 'ens15f0' and entered the appropriate settings for
the storage interface:


Subnet: 10.66.48.0/21

Address: 10.66.55.141

Gateway: <blank>

Name Servers: <blank>

Search Domains: <blank>

I tabbed to select ...


\[Save\]

... and hit \[Enter\]

Then I selected adapter 'ens15f1' and entered the appropriate settings
for the compute interface:


Subnet: 10.66.56.0/21

Address: 10.66.63.141

Gateway: <blank>

Name Servers: <blank>

Search Domains: <blank>

I tabbed to select ...


\[Save\]

... and hit \[Enter\]

Upon completion I tabbed to select ...


\[Done\]

... and hit \[Enter\]

#### internet settings

At the Proxy screen, I simply tabbed to select ...


\[Done\]

... and hit \[Enter\]

At the Ubuntu archive mirror screen, I simply tabbed to select ...


\[Done\]

... and hit \[Enter\]

At the "Installer update available" screen, I simply tabbed to select
...


\[Continue without updating\]

... and hit \[Enter\]

#### storage config


Note: This system doesn't have a RAID controller, so software RAID will
be used.

At the storage configuration screen, I tabbed down to ...


\[ \] Custom storage layout

... and hit \[space\] to enable it:


\[x\] Custom storage layout


Note: Doing so effectively disables the default guided-storage option.

Then I tabbed to select ...


\[Done\]

... and hit \[Enter\]

At the subsequent screen, I proceeded to create partitions as follows.

In the AVAILABLE DEVICES section, three devices were listed:


\[ ST4000NM0024-1HT178_Z4F0KC2D local disk 3.638T \> \]

\[ ST4000NM0024-1HT178_Z4F0KD9X local disk 3.638T \> \]

\[ ST4000NM0024-1HT178_Z4F0KDQA local disk 3.638T \> \]

I selected the first-listed device:


\[ ST4000NM0024-1HT178_Z4F0KC2D local disk 3.638T \> \]

In its corresponding drop-down menu I selected:


\[Use as Boot Device\]

Back at the main storage-configuration screen, in the AVAILABLE DEVICES
section, I selected the second-listed device:


\[ ST4000NM0024-1HT178_Z4F0KD9X local disk 3.638T \> \]

In its corresponding drop-down menu I selected:


\[Add As Another Boot Device\]

Back at the main storage-configuration screen, in the AVAILABLE DEVICES
section, I selected the third-listed device:


\[ ST4000NM0024-1HT178_Z4F0KDQA local disk 3.638T \> \]

In its corresponding drop-down menu I selected:


\[Add As Another Boot Device\]

Back at the main storage-configuration screen, in the AVAILABLE DEVICES
section, under the first device, I selected:


\[ free space 3.638T \> \]

In its corresponding drop-down menu I selected:


\[Add GPT Partition\]

At the add-gpt-partition pop-up window, I left size unspecified (in
order to use all space), and in the Format drop-down menu, I selected:


Format: \[ Leave unformatted \]

Then I tabbed to select ...


\[ Create \]

... and hit \[Enter\]

Back at the main storage-configuration screen, in the AVAILABLE DEVICES
section, for the free space of the second and third devices, I repeated
the above add-gpt-partition steps.

Back at the main storage-configuration screen, I tabbed down to ...


\[ Create software RAID (md) \> \]

... and hit \[Enter\]

In the software-RAID configuration pop-up window, I left name and raid
level unchanged ...


Name: md0

\[ RAID Level: 1 (mirrored) \]

... then I arrowed down to the Devices section and for each of the three
devices I hit \[space\] to select them for use, leaving the first two
set as 'Active' and setting the third to 'spare'

Back at the main storage-configuration screen, in the AVAILABLE DEVICES
section, in place of the three disk devices the newly-created RAID
volume was now listed:


\[ md0 (new, unused) software RAID 1 3.638T \> \]

Under that device, I selected:


\[ free space 3.638T \> \]

In its corresponding drop-down menu I selected:


\[Add GPT Partition\]

In the add-gpt-partition pop-up window, in order to create a 'root'
partition, I entered ...


Size: 10G

Format: ext4

Mount: /

... then I tabbed to select ...


\[ Create \]

... and hit \[Enter\]

Back at the main storage-configuration screen, I repeated this
add-partition step several times, in order to create other desired
partitions:



------------------------------------------------------------------------

Size: 20G

Format: ext4

Mount: /usr

------------------------------------------------------------------------

Size: 40G

Format: ext4

Mount: /usr/local

------------------------------------------------------------------------

Size: 10G

Format: ext4

Mount: /srv

------------------------------------------------------------------------

Size: 10G

Format: ext4

Mount: /opt

------------------------------------------------------------------------

Size: 10G

Format: ext4

Mount: /var/log

------------------------------------------------------------------------

Size: 16G

Format: swap

Mount: <blank>

------------------------------------------------------------------------

Size: 10G

Format: ext4

Mount: /tmp

------------------------------------------------------------------------

Size: 500G

Format: ext4

Mount: /var

------------------------------------------------------------------------

Size: \< unspecified \>

Format: ext4

Mount: /scratch

------------------------------------------------------------------------

Note that for the final partition (scratch), I left partition size
unspecified (in order to use all remaining space)

Back at the main storage-configuration screen, I reviewed the resulting
configuration:

     -------------------------------------------------------------------------------------
     |                                                                                   |
     | Storage configuration                                                             |
     |                                                                                   |
     | FILE SYSTEM SUMMARY                                                               |
     |                                                                                   |
     |   MOUNT POINT   SIZE    TYPE       DEVICE TYPE                                    |
     | [ /            10.000G  new  ext4  new partition of local disk > ]                |
     | [ /opt         10.000G  new  ext4  new partition of local disk > ]                |
     | [ /scratch      3.072T  new  ext4  new partition of local disk > ]                |
     | [ /srv         10.000G  new  ext4  new partition of local disk > ]                |
     | [ /tmp         10.000G  new  ext4  new partition of local disk > ]                |
     | [ /usr         20.000G  new  ext4  new partition of local disk > ]                |
     | [ /usr/local   40.000G  new  ext4  new partition of local disk > ]                |
     | [ /var        500.000G  new  ext4  new partition of local disk > ]                |
     | [ /var/log     10.000G  new  ext4  new partition of local disk > ]                |
     | [ SWAP         16.000G  new  swap  new partition of local disk > ]                |
     |                                                                                   |
     | AVAILABLE DEVICES                                                                 |
     |                                                                                   |
     |   No available devices                                                            |
     |                                                                                   |
     | [ Create software RAID (md) > ]                                                   |
     | [ Create volume group (LVM) > ]                                                   |
     |                                                                                   |
     | USED DEVICES                                                                      |
     |                                                                                   |
     |  DEVICE                                               TYPE               SIZE     |
     | [ md0 (new, unused)                                   Software RAID 1  3.638T > ] |
     |   partition 1   new, to be formatted as ext4, mounted at /            10.000G >   |
     |   partition 2   new, to be formatted as ext4, mounted at /usr         20.000G >   |
     |   partition 3   new, to be formatted as ext4, mounted at /usr/local   40.000G >   |
     |   partition 4   new, to be formatted as ext4, mounted at /srv.        10.000G >   |
     |   partition 5   new, to be formatted as ext4, mounted at /opt         10.000G >   |
     |   partition 6   new, to be formatted as ext4, mounted at /var/log     10.000G >   |
     |   partition 7   new, to be formatted as swap                          16.000G >   |
     |   partition 8   new, to be formatted as ext4, mounted at /tmp         10.000G >   |
     |   partition 9   new, to be formatted as ext4, mounted at /var        500.000G >   |
     |   partition 10  new, to be formatted as ext4, mounted at /scratch      3.027T >   |
     |                                                                                   |
     [ [ ST4000NM0024-1HT178_Z4F0KC2D                        local disk       3.638T > ] |
     |   partition 1   new, BIOS grub spacer                                  1.000M >   |
     |   partition 2   new, component of software RAID 1 md0                  3.638T >   |
     |                                                                                   |
     [ [ ST4000NM0024-1HT178_Z4F0KD9X                        local disk       3.638T > ] |
     |   partition 1   new, BIOS grub spacer                                  1.000M >   |
     |   partition 2   new, component of software RAID 1 md0                  3.638T >   |
     |                                                                                   |
     [ [ ST4000NM0024-1HT178_Z4F0KDQA                        local disk       3.638T > ] |
     |   partition 1   new, BIOS grub spacer                                  1.000M >   |
     |   partition 2   new, component of software RAID 1 md0                  3.638T >   |
     |                                                                                   |
     [ [ _Patriot_Memory_<string>_0:0                        local disk       7.460G > ] |
     |   partition 1   existing, already formatted as vfat, in use            7.460G     |
     |                                                                                   |
     |     [ Done ]                                                                      |
     |     [ Reset ]                                                                     |
     |     [ Back ]                                                                      |
     -------------------------------------------------------------------------------------

Satisfied with the storage configuration, I tabbed to select ...


\[Done\]

... and hit \[Enter\]

At the confirmation pop-up window, I tabbed to select ...


\[Continue\]

... and hit \[Enter\]

#### profile

At the server Profile screen, I entered:


Your name: maint

Your server's name: cbs-divid

Pick a username: maint

Choose a password: \<-cbs-root-password-\>

Confirm your password: \<-cbs-root-password-\>

Upon completion, I tabbed to select ...


\[Done\]

... and hit \[Enter\]

#### ubuntu pro

At the upgrade-to-ubuntu-pro screen, I left the default setting selected
...


\[ \] Enable Ubuntu Pro

\[x\] Skip for now

... and simply tabbed to select ...


\[Continue\]

... and hit \[Enter\]

#### ssh

At the ssh setup screen, I arrowed down to ...


\[ \] Install OpenSSH server

... and hit \[space\] to enable it:


\[x\] Install OpenSSH server

Then I tabbed to select ...


\[Done\]

... and hit \[Enter\]

#### snaps

At the Snaps (application-and-dependency bundles) screen, I simply
tabbed to select ...


\[Done\]

... and hit \[Enter\]

Installation proceeded.

Ultimately, installation took about fifteen minutes.

Upon successful completion, I tabbed to select ...


\[Reboot Now\]

... and hit \[Enter\]

The system rebooted.

I observed the reboot process, noting that there was a long (two
minute?) delay for the systemd-networkd-wait-online service. I will
address that issue later.

Once the reboot process completed, at the console I logged on as user
'maint' and verified that I could reach sites on the "public" network:

`ping -c1 google.com`

'ping' output (example):

    PING google.com (142.251.33.110) 56(84) bytes of data.
    64 bytes from sea30s10-in-f14.1e100.net (142.251.33.110): icmp_seq=1 ttl=57 time=1.20 ms
    --- google.com ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms
    rtt min/avg/max/mdev = 1.199/1.199/1.199/0.000 ms

Then I logged off, returned to my desk, and opened an SSH terminal in
order to proceed with remaining setup steps.

### enable firewall

In the ssh terminal, I logged back on as user 'maint' ...

`ssh -l maint sph-login0.hpc.sph.washington.edu`

... then I started a root shell:

    sudo -i

I edited the ufw (Uncomplicated Firewall) configuration file:

    cp -p /etc/default/ufw /var/tmp/

    vi /etc/default/ufw

    diff /var/tmp/ufw /etc/default/ufw

'diff' output:

    7c7
    < IPV6=yes
    ---
    > IPV6=no

I set ufw to allow ssh traffic ...

    ufw allow ssh/tcp
    < output snipped >

... then I enabled and started the firewall:

    systemctl enable --now ufw

    ufw enable

    ufw status

'ufw status' output:

    Status: active

    To                         Action      From
    --                         ------      ----
    22/tcp                     ALLOW       Anywhere

### enable root

I enabled the root account by setting its password:

    passwd root

Note that SSH login for root is disabled by default.

`grep #PermitRootLogin /etc/ssh/sshd_config`

output:

`#PermitRootLogin prohibit-password`

### set fqdn

I set the fully-qualified domain name:

    hostnamectl set-hostname sph-login0.hpc.sph.washington.edu

    hostname -f

'hostname' output:

`sph-login0.hpc.sph.washington.edu`

### update network interface config

I edited the netplan file to assign a second IP address (compute) to one
of the bonded interface pairs:


Note:


At the moment, the bonded interface pair is connected to a single common
switch, but ultimately we plan to upgrade network switch infrastructure
and connect the pair to separate switches for redundancy.

<!-- -->

    cp -p /etc/netplan/50-cloud-init.yaml /var/tmp/

    vi /etc/netplan/50-cloud-init.yaml

    cat /etc/netplan/50-cloud-init.yaml

'cat' output:

I applied the change:

    netplan apply

    ip a

'ip' output:

I verified that DNS name resolution was working:

`host sph-login0-c`

output:

### update system

I updated the system as follows.

I ran 'apt update' ...

    apt update
    < output snipped >

... then I ran 'apt upgrade':

`apt -y upgrade`
`< output snipped >`

Upon completion I rebooted the system:

    reboot

### install system utilities

After the reboot, I logged back on and became root again ...

`sudo -i`

... and installed additional packages.

    apt -y install vim wget lm-sensors smartmontools xauth xterm \
    adwaita-icon-theme-full ldap-utils tree mailutils net-tools \
    logwatch unzip tcsh traceroute whois nfs-common cifs-utils \
    links autofs rcs acct chrony rename fail2ban
    < output snipped >

For the 'postfix' package (a 'mailutils' dependency), an interactive
package-configuration utility was automatically launched.

At postfix-configuration screen I arrowed down to select ...


No configuration

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

The interactive installation utility closed and package installation
proceeded to completion.

### configure fail2ban

I configured fail2ban as follows.

I noted that one jail was enabled by default:

`fail2ban-client status`

output:

    Status
    |- Number of jail:  1
    `- Jail list:   sshd

Note: The sshd jail is enabled by means of defaults-debian configuration
file in the jail.d directory:

`cat jail.d/defaults-debian.conf`

'cat' output:

    [sshd]
    enabled = true

I created a 'jail.local' configuration file, based on the jail.conf
file, but with blank lines and commented-out lines stripped:

    cd /etc/fail2ban/
    egrep -v '^\s*(#|$)' jail.conf > jail.local

After setting aside a copy, I edited the file and viewed the changes:

    cp -p jail.local /var/tmp/

    vi jail.local

    diff /var/tmp/jail.local jail.local

'diff' output:

    16c16,17
    < sender = root@<fq-hostname>
    ---
    > sender = biosys@uw.edu
    > sendername = "Fail2Ban cbs-divid"
    22,23c23
    < banaction = iptables-multiport
    < banaction_allports = iptables-allports
    ---
    > banaction = ufw
    26c26
    <             %(mta)s-whois[sender="%(sender)s", dest="%(destemail)s", protocol="%(protocol)s", chain="%(chain)s"]
    ---
    >             %(mta)s-whois[name=%(__name__)s, sender="%(sender)s", sendername="%(sendername)s", dest="%(destemail)s", protocol="%(protocol)s", chain="%(chain)s"]
    28c28
    <              %(mta)s-whois-lines[sender="%(sender)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]
    ---
    >              %(mta)s-whois-lines[name=%(__name__)s, sender="%(sender)s", sendername="%(sendername)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]
    32c32
    <                 %(mta)s-whois-lines[sender="%(sender)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]
    ---
    >                 %(mta)s-whois-lines[name=%(__name__)s, sender="%(sender)s", sendername="%(sendername)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]
    39a40
    > ignoreip  = 127.0.0.1/8

I restarted fail2ban and verified the configuration:

    cd

    systemctl restart fail2ban

    fail2ban-client status

'fail2ban-client' output:

    Status
    |- Number of jail:  1
    `- Jail list:   sshd

I viewed sshd jail status:

`fail2ban-client status sshd`

output:

    Status for the jail: sshd
    |- Filter
    |  |- Currently failed: 0
    |  |- Total failed: 0
    |  `- File list:    /var/log/auth.log
    `- Actions
       |- Currently banned: 0
       |- Total banned: 0
       `- Banned IP list:


Note: Additional jails will be enabled later.

### configure time sync

I updated the timezone:

    timedatectl set-timezone America/Los_Angeles

    timedatectl status

'timedatectl' output:

                   Local time: Fri 2024-08-09 13:15:28 PDT
               Universal time: Fri 2024-08-09 20:15:28 UTC
                     RTC time: Fri 2024-08-09 20:15:28
                    Time zone: America/Los_Angeles (PDT, -0700)
    System clock synchronized: yes
                  NTP service: active
              RTC in local TZ: no

The network time protocol daemon 'chrony' was installed previously. I
viewed its explicitly-set configuration options, for the record:

`egrep -v '^\s*(#|$)' /etc/chrony/chrony.conf`

output:

    confdir /etc/chrony/conf.d
    pool ntp.ubuntu.com        iburst maxsources 4
    pool 0.ubuntu.pool.ntp.org iburst maxsources 1
    pool 1.ubuntu.pool.ntp.org iburst maxsources 1
    pool 2.ubuntu.pool.ntp.org iburst maxsources 2
    sourcedir /run/chrony-dhcp
    sourcedir /etc/chrony/sources.d
    keyfile /etc/chrony/chrony.keys
    driftfile /var/lib/chrony/chrony.drift
    ntsdumpdir /var/lib/chrony
    logdir /var/log/chrony
    maxupdateskew 100.0
    rtcsync
    makestep 1 3
    leapsectz right/UTC

### set up postfix

With Postfix installed (in a previous step), I re-ran the
package-configuration utility:

`dpkg-reconfigure postfix`
`< output snipped >`

At the configuration-type screen I arrowed down to select ...


Internet Site

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I left the fully-qualified host name unchanged:


System mail name: sph-login0.hpc.sph.washington.edu

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I entered the root mail alias:


Root and postmaster mail recipient: bstatsys@uw.edu

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I noted that default accept-for-delivery
destinations were automatically set:


Other domains to accept mail for (blank for none):

\$myhostname, sph-login0.hpc.sph.washington.edu,
localhost.hpc.sph.washington.edu, , localhost

I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I noted that force-synchronous-updates was disabled
by default:


Force synchronous updates on mail queue?


Yes

\[No\]

I hit \[Enter\]

At the next screen, I noted that default network blocks were
automatically set:


Local networks: 127.0.0.0/8 \[::ffff:127.0.0.0\]/104 \[::1\]/128

I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I noted that default mailbox size was automatically
set to 'no limit':


Mailbox size limit: 0

I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I noted that the address extension character was set
to '+':


Local address extension character: +

In order to disable address extensions I deleted the '+' symbol.

Then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the next screen, I noted that all network protocols were enabled by
default:


Internet protocols to use:


all

I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At this point, the interactive package-configuration utility closed, and
configuration completed.

I updated /etc/aliases ...

    cd /etc/

    cp -p aliases aliases.release

    vi aliases

    cat aliases

'cat' output:

    # See man 5 aliases for format
    postmaster:    root
    servermon-notify:    root
    root:    bstatsys@uw.edu

I reinitialized the alias database:

`newaliases`

I enabled SASL-secured mail relay as follows.

    cd /etc/postfix/

    cp -p main.cf main.cf.release

    vi main.cf

    diff main.cf.release main.cf

'diff' output:

    42c42,48
    < relayhost =
    ---
    > relayhost = [smtp.washington.edu]
    > smtp_sasl_auth_enable = yes
    > smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
    > smtp_sasl_security_options = noanonymous
    > smtp_tls_CApath = /etc/ssl/certs
    > smtp_use_tls = yes
    > smtp_sasl_mechanism_filter = plain, login

I created the SASL password file:

    vi /etc/postfix/sasl_passwd

    cat /etc/postfix/sasl_passwd

'cat' output:

    [smtp.washington.edu] biostatsmtp:<-actual pw from keepass->

I adjusted the password file permissions:

`chmod 0600 /etc/postfix/sasl_passwd`

I created a postfix lookup table:

`postmap /etc/postfix/sasl_passwd`

I checked postfix file/directory ownership and permissions:

    cd

    postfix check
    < no output >

I restarted postfix and checked its status:

    systemctl restart postfix

    systemctl status postfix

'systemctl status' output:

    ● postfix.service - Postfix Mail Transport Agent
         Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; preset: enabled)
         Active: active (exited) since Fri 2024-08-09 15:02:46 PDT; 17ms ago
           Docs: man:postfix(1)
        Process: 10011 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
       Main PID: 10011 (code=exited, status=0/SUCCESS)
            CPU: 1ms

    Aug 09 15:02:46 sph-login0.hpc.sph.washington.edu systemd[1]: Starting postfix.service - Postfix Mail Transport Agent...
    Aug 09 15:02:46 sph-login0.hpc.sph.washington.edu systemd[1]: Finished postfix.service - Postfix Mail Transport Agent.

I tested mail delivery:

    hostname=`hostname`
    echo "test" | mail -s "test e-mail on $hostname" root

The message was delivered as expected.

### configure process accounting

The process-accounting package was installed and automatically enabled
previously.

I addressed a bug as follows.


Reference:
[Ubuntu-20.04.biostat-kriek#configure_process_accounting](Ubuntu-20.04.biostat-kriek#configure_process_accounting "wikilink")

I updated the logrotate 'wtmp' file:

    cd /etc/logrotate.d/

    vi wtmp

    cat wtmp

'cat' output:

    # no packages own wtmp -- we'll rotate it here
    /var/log/wtmp {
        su root adm
        missingok
        monthly
        create 0640 root adm
        rotate 1
    }

I verified that 'logrotate' worked; that is, upon running it, 'wtmp' was
moved to 'wtmp.1', and a new 'wtmp' file was created):

    logrotate -f /etc/logrotate.d/wtmp

    ls -al /var/log/wtmp*

'ls' output:

    -rw-r----- 1 root adm      0 Aug 12 09:39 /var/log/wtmp
    -rw-rw-r-- 1 root utmp 11520 Aug 12 09:36 /var/log/wtmp.1

I made the same change to the 'btmp' config file:

    vi btmp
    cat btmp

'cat' output:

    # no packages own btmp -- we'll rotate it here
    /var/log/btmp {
        su root adm
        missingok
        monthly
        create 0640 root adm
        rotate 1
    }

### set scratch dir perms

For the scratch directory, I set sticky-bit permissions:

    cd

    chmod 1777 /scratch

    ls -ld /scratch

'ls' output:

    drwxrwxrwt 3 root root 4096 Aug  8 11:07 /scratch

### modify motd

I created a message-of-the-day file:

    cd

    hostname >> /etc/motd
    echo "------------------------" >> /etc/motd

    cat /etc/motd

'cat' output:

    sph-login0.hpc.sph.washington.edu
    ------------------------

Then I disabled the dynamic motd as follows.


Reference:
<https://raymii.org/s/tutorials/Disable_dynamic_motd_and_motd_news_spam_on_Ubuntu_18.04.html#toc_4>

I made a copy of the sshd pluggable-auth-module file:

`cp -p /etc/pam.d/sshd /var/tmp/`

I edited the original and reviewed the changes:

    vi /etc/pam.d/sshd

    diff /var/tmp/sshd /etc/pam.d/sshd

'diff' output:

    33,34c33,35
    < session    optional     pam_motd.so  motd=/run/motd.dynamic
    < session    optional     pam_motd.so noupdate
    ---
    > session    optional     pam_motd.so  motd=/etc/motd
    > #session    optional     pam_motd.so  motd=/run/motd.dynamic
    > #session    optional     pam_motd.so noupdate
    37c38
    < session    optional     pam_mail.so standard noenv # [1]
    ---
    > #session    optional     pam_mail.so standard noenv # [1]


Note: I also disabled the mailbox-status message (by commenting out the
pam_mail.so directive).

To verify, I did a new login:

    ssh -l maint sph-login0.hpc.sph.washington.edu

'ssh' output:

    maint@sph-login0.hpc.sph.washington.edu's password:
    sph-login0.hpc.sph.washington.edu
    ------------------------
    Last login: Mon Aug 12 09:36:04 2024 from 10.104.148.72

I logged out.

`exit`

### configure rsyslog

Rsyslog daemon was installed and automatically enabled previously. I
updated its configuration.

    cp -p /etc/cron.daily/00logwatch /var/tmp/
    vi /etc/cron.daily/00logwatch
    diff /var/tmp/00logwatch /etc/cron.daily/00logwatch

'diff' output:

`7c7`

\< /usr/sbin/logwatch --output mail --- \> /usr/sbin/logwatch --output
file --filename /tmp/logwatch

(It's likely that log events will be redirected/monitored in some other
manner.)

### install splunkforwarder

\- n/a -- an alternative monitoring solution will likely be utilized in
this case -

In order to redirect log events to our Splunk server, I installed
SplunkForwarder as follows.

I created the splunkforwarder installation directory:

`mkdir -p /opt/splunkforwarder/etc/system/local`

I created a local user and group:

    groupadd -g 597 splunk
    useradd -u 597 -g 597 -d /opt/splunkforwarder splunk

    grep 597 /etc/passwd /etc/group

'grep' output:

    useradd warning: splunk's uid 597 outside of the UID_MIN 1000 and UID_MAX 60000 range.
    /etc/passwd:splunk:x:597:597::/opt/splunkforwarder:/bin/sh
    /etc/group:splunk:x:597:

From the splunk server (keller), I retrieved the files necessary for
deployment:

`scp -r keller.biostat.washington.edu:/var/tmp/splunkforwarder /var/tmp/`

I moved two splunk-configuration files to appropriate locations:

    mv /var/tmp/splunkforwarder/user-seed.conf /opt/splunkforwarder/etc/system/local/
    mv /var/tmp/splunkforwarder/deploymentclient.conf /opt/splunkforwarder/etc/system/local/

I moved two system-configuration files into place:

    mv /var/tmp/splunkforwarder/00logwatch.ubuntu /etc/cron.daily/00logwatch
    mv /var/tmp/splunkforwarder/logrotate.conf.ubuntu /etc/logrotate.conf

I recursively adjusted ownership of the installation directory:

`chown -R splunk:splunk /opt/splunkforwarder`

I installed the splunkforwarder package ...

    dpkg -i /var/tmp/splunkforwarder/splunkforwarder-8.2.0-e053ef3c985f-linux-2.6-amd64.deb

'dpkg' output:

    Selecting previously unselected package splunkforwarder.
    (Reading database ... 82287 files and directories currently installed.)
    Preparing to unpack .../splunkforwarder-8.2.0-e053ef3c985f-linux-2.6-amd64.deb ...
    Unpacking splunkforwarder (8.2.0) ...
    Setting up splunkforwarder (8.2.0) ...
    cp: cannot stat '/opt/splunkforwarder/etc/regid.2001-12.com.splunk-UniversalForwarder.swidtag': No such file or directory
    complete

... then I adjusted log-directory permissions and added the splunk user
to group 'adm':

    chmod -R g+r /var/log
    usermod -aG adm splunk

I enabled the application as follows.

I switched to user 'splunk' ...

`su - splunk`

... then I ran the appropriate 'splunk start' command:

    ./bin/./splunk start --accept-license --no-prompt

'splunk start' output:

    This appears to be your first time running this version of Splunk.

    Splunk> Australian for grep.

    Checking prerequisites...
        Checking mgmt port [8089]: open
            Creating: /opt/splunkforwarder/var/lib/splunk
            Creating: /opt/splunkforwarder/var/run/splunk
            Creating: /opt/splunkforwarder/var/run/splunk/appserver/i18n
            Creating: /opt/splunkforwarder/var/run/splunk/appserver/modules/static/css
            Creating: /opt/splunkforwarder/var/run/splunk/upload
            Creating: /opt/splunkforwarder/var/run/splunk/search_telemetry
            Creating: /opt/splunkforwarder/var/spool/splunk
            Creating: /opt/splunkforwarder/var/spool/dirmoncache
            Creating: /opt/splunkforwarder/var/lib/splunk/authDb
            Creating: /opt/splunkforwarder/var/lib/splunk/hashDb
    New certs have been generated in '/opt/splunkforwarder/etc/auth'.
        Checking conf files for problems...
        Done
        Checking default conf files for edits...
        Validating installed files against hashes from '/opt/splunkforwarder/splunkforwarder-8.2.0-e053ef3c985f-linux-2.6-x86_64-manifest'
        All installed files intact.
        Done
    All preliminary checks passed.

    Starting splunk server daemon (splunkd)...
    Done

I reverted to user 'root' ...

`exit`

... and enabled startup on boot:

    /opt/splunkforwarder/bin/splunk enable boot-start -user splunk

output:

    Init script installed at /etc/init.d/splunk.
    Init script is configured to run at boot.

Finally, I restarted splunk ...

`systemctl restart splunk`

... then after a moment, I restarted it again:

`systemctl restart splunk`

In a web browser, I logged on to the Splunk server:


<https://loghost.biostat.washington.edu:8000/>

I went to


"Settings" menu -\> "Distributed Environment" section -\> Forwarder
Management

In the "filter" search box, I entered:


cbs-divid

The host was displayed as expected; apps had been deployed and it had
just phoned home.

### join netid domain

I joined the system to the netid domain as follows.

#### pre-create account

I created a netid account and spn as follows.

I logged on to a Windows admin server (biostat-wallace) with my sadm
account, launched an administrative powershell session, and pre-created
the netid account and desired SPNs:

    import-module activedirectory

    C:\bite\New-UWWIComputer -computerName "cbs-divid" -path "OU=cbs,OU=Delegated,DC=netid,dc=washington,dc=edu"

    setspn -S HOST/cbs-divid.cbs.biostat.washington.edu cbs-divid

    setspn -S nfs/cbs-divid.cbs.biostat.washington.edu cbs-divid

    setspn -l cbs-divid

    exit

powershell input and output:

    PS C:\Windows\system32> import-module activedirectory

    PS C:\Windows\system32> C:\bite\New-UWWIComputer -computerName "sph-login0" -path "OU=hpc,OU=sph,OU=Delegated,DC=netid,dc=washington,dc=edu"
    New computer objected created for sph-login0

    PS C:\Windows\system32> setspn -S HOST/sph-login0.hpc.sph.washington.edu sph-login0
    Checking domain DC=netid,DC=washington,DC=edu
    Registering ServicePrincipalNames for CN=sph-login0,OU=hpc,OU=sph,OU=Delegated,DC=netid,DC=washington,DC=edu
            HOST/sph-login0.hpc.sph.washington.edu
    Updated object

    PS C:\Windows\system32> setspn -S nfs/sph-login0.hpc.sph.washington.edu sph-login0
    Checking domain DC=netid,DC=washington,DC=edu
    Registering ServicePrincipalNames for CN=sph-login0,OU=hpc,OU=sph,OU=Delegated,DC=netid,DC=washington,DC=edu
            nfs/sph-login0.hpc.sph.washington.edu
    Updated object

    PS C:\Windows\system32> setspn -l sph-login0
    Registered ServicePrincipalNames for CN=sph-login0,OU=hpc,OU=sph,OU=Delegated,DC=netid,DC=washington,DC=edu:
            nfs/sph-login0.hpc.sph.washington.edu
            HOST/sph-login0.hpc.sph.washington.edu

    PS C:\Windows\system32> exit

#### install packages

I installed required packages:

    apt -y install realmd libnss-sss libpam-sss sssd sssd-tools \
    adcli samba-common-bin oddjob oddjob-mkhomedir packagekit krb5-user
    < output snipped >

In the course of installation, an interactive package-configuration
utility was automatically launched.

At the domain-realm screen, I left the default setting unchanged (for
the moment) ...


Default Kerberos version 5 realm: CBS.BIOSTAT.WASHINGTON.EDU

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the kerberos-server screen, I left the default setting blank (for the
moment) ...


Kerberos servers for your realm: \< blank \>

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

At the admin-server screen, I left the default setting blank (for the
moment) ...


Administrative server for your Kerberos realm: \< blank \>

... then I tabbed to the bottom of the screen to select ...


\[Ok\]

... and hit \[Enter\]

The interactive installation utility closed and package installation
proceeded to completion.

#### create domain users group

In netid domain 'Domain Users' is set as all users' primary group, but
sssd is unable to enumerate members of that group, so a corresponding
local group with the same gidNumber needs to be created. I created the
group:

    addgroup --gid 147849729 domain_users

    getent group 147849729

'getent' output:

`domain_users:x:147849729:`

#### join domain

I proceeded to join the netid domain as follows.

I verified that the domain could be found:

`realm discover netid.washington.edu`

output:

    netid.washington.edu
      type: kerberos
      realm-name: NETID.WASHINGTON.EDU
      domain-name: netid.washington.edu
      configured: no
      server-software: active-directory
      client-software: sssd
      required-package: sssd-tools
      required-package: sssd
      required-package: libnss-sss
      required-package: libpam-sss
      required-package: adcli
      required-package: samba-common-bin

Using my sadm_ account, I joined the domain, entering the appropriate
password at the prompt:

    realm join -U sadm_rmoulton netid.washington.edu

I verified that the domain was joined successfully, noting the line ...


configured: kerberos-member

... in the command output:

`realm discover netid.washington.edu`

output:

    netid.washington.edu
      type: kerberos
      realm-name: NETID.WASHINGTON.EDU
      domain-name: netid.washington.edu
      configured: kerberos-member
      server-software: active-directory
      client-software: sssd
      required-package: sssd-tools
      required-package: sssd
      required-package: libnss-sss
      required-package: libpam-sss
      required-package: adcli
      required-package: samba-common-bin
      login-formats: %U@netid.washington.edu
      login-policy: allow-realm-logins

#### adjust sssd config

I updated the sssd configuration as follows.

    cd /etc/sssd/

    cp -p sssd.conf sssd.conf.orig

    vi sssd.conf

    cat sssd.conf

'cat' output:

    [sssd]
    domains = netid.washington.edu
    config_file_version = 2
    services = nss, pam
    implicit_pac_responder = false

    [domain/netid.washington.edu]
    default_shell = /bin/bash
    krb5_store_password_if_offline = True
    cache_credentials = True
    krb5_realm = NETID.WASHINGTON.EDU
    realmd_tags = manages-system joined-with-adcli
    id_provider = ad
    fallback_homedir = /home/users/%u
    ad_domain = netid.washington.edu
    use_fully_qualified_names = False
    ldap_id_mapping = False
    access_provider = simple
    override_homedir = /home/users/%u

    # disable gpo access controls
    ad_gpo_access_control = disabled

    # renew tickets automatically every 600 seconds for 90 days
    krb5_renewable_lifetime = 90d
    krb5_renew_interval = 600

    # Domain Users isn't readable so force it here
    override_gid = 147849729

    # group search
    ldap_group_search_base = OU=Standard,OU=GDS,OU=Groups,DC=netid,DC=washington,DC=edu?sub?(cn=uw_sph_hpc_*)
    ldap_use_tokengroups = False
    ldap_group_nesting_level = 2

    # groups allowed access to the system
    simple_allow_groups = uw_sph_hpc_admins,uw_sph_hpc_users

I stopped and disabled various SSSD services:


Reference:<https://lists.fedorahosted.org/archives/list/sssd-users@lists.fedorahosted.org/thread/LVYKVL6KNBOQ6O2MKTBCQSSRQATLWPYT/>

<!-- -->

    systemctl stop sssd-pac.socket
    systemctl stop sssd-nss.socket
    systemctl stop sssd-pam.socket
    systemctl stop sssd-pam-priv.socket
    systemctl disable sssd-pac.socket
    systemctl disable sssd-nss.socket
    systemctl disable sssd-pam.socket
    systemctl disable sssd-pam-priv.socket

I restarted sssd:

`systemctl restart sssd`

</pre>

#### adjust kerberos config

I updated the kerberos configuration as follows.

    cd /etc/

    cp -p krb5.conf krb5.conf.netid-join.orig

    vi krb5.conf

    cat krb5.conf

'cat' output:

    [libdefaults]
    default_realm = NETID.WASHINGTON.EDU
    dns_lookup_realm = false
    dns_lookup_kdc = true
    rdns = false

#### enable sudo access

For sadm_\* admin accounts, I enabled sudo access as follows.

As root, I set the default editor to 'vim' ...

`update-alternatives --config editor`

input/output:

    There are 4 choices for the alternative editor (providing /usr/bin/editor).

      Selection    Path                Priority   Status
    ------------------------------------------------------------
    * 0            /bin/nano            40        auto mode
      1            /bin/ed             -100       manual mode
      2            /bin/nano            40        manual mode
      3            /usr/bin/vim.basic   30        manual mode
      4            /usr/bin/vim.tiny    15        manual mode

    Press <enter> to keep the current choice[*], or type selection number: 3
    update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in manual mode

... then I created a sadm_sudo sudoers config file:

    visudo /etc/sudoers.d/sadm_sudo

    cat /etc/sudoers.d/sadm_sudo

'cat' output:

    # allow members of group uw_sph_hpc_admins to execute any command
    %uw_sph_hpc_admins   ALL=(ALL:ALL) ALL

#### test

I tested kerberos:

    kinit rmoulton

    klist

'klist' output:

    Ticket cache: FILE:/tmp/krb5cc_0
    Default principal: rmoulton@NETID.WASHINGTON.EDU

    Valid starting       Expires              Service principal
    08/12/2024 14:23:12  08/13/2024 00:23:12  krbtgt/NETID.WASHINGTON.EDU@NETID.WASHINGTON.EDU
        renew until 08/13/2024 14:23:04

I destroyed the ticket:

`kdestroy`

I tested user lookup

`id rmoulton`

output:

    uid=45068(rmoulton) gid=147849729(domain_users) groups=147849729(domain_users),1044389(uw_sph_hpc_users),1044502(uw_sph_hpc_admins)

I tested ldap:

    kinit -k 'SPH-LOGIN0$@NETID.WASHINGTON.EDU'

    ldapsearch -LLL -Y GSSAPI -N -H ldap://ezra.netid.washington.edu:389 -b DC=netid,DC=washington,DC=edu '(name=biostat-wpkg)'

'ldapsearch' output:

    SASL/GSSAPI authentication started
    SASL username: SPH-LOGIN0$@NETID.WASHINGTON.EDU
    SASL SSF: 256
    SASL data security layer installed.
    dn: CN=biostat-wpkg,OU=UWNetID,DC=netid,DC=washington,DC=edu
    objectClass: top
    objectClass: uwEntity
    objectClass: uwCatalystMacSupport
    objectClass: uwPrincipal
    objectClass: person
    objectClass: organizationalPerson
    objectClass: user
    cn: biostat-wpkg
    sn: biostat-wpkg
    givenName: -
    distinguishedName: CN=biostat-wpkg,OU=UWNetID,DC=netid,DC=washington,DC=edu
    displayName: Department of Biostatistics
    name: biostat-wpkg
    objectGUID:: zT93AgpQsUCGv+/4JSRd7g==
    userAccountControl: 1049088
    codePage: 0
    countryCode: 0
    primaryGroupID: 513
    objectSid:: AQUAAAAAAAUVAAAARugdWAxflwfuf2d1NR8oAA==
    sAMAccountName: biostat-wpkg
    sAMAccountType: 805306368
    userPrincipalName: biostat-wpkg@uw.edu
    objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=netid,DC=washington,DC
     =edu
    msDS-ExternalDirectoryObjectId: User_03b465e0-104c-4877-a2a0-63887ec4703e
    mail: biostat-wpkg@uw.edu
    gidNumber: 1102629429
    uwTest: 0
    uidNumber: 1203142
    uwEntityType: non-person
    uwCatalystMacHomeDir: /Users/biostat-wpkg
    uwNetID: biostat-wpkg
    extensionAttribute1: 0

    # refldap://ForestDnsZones.netid.washington.edu/DC=ForestDnsZones,DC=netid,DC=w
     ashington,DC=edu

    # refldap://DomainDnsZones.netid.washington.edu/DC=DomainDnsZones,DC=netid,DC=w
     ashington,DC=edu

    # refldap://netid.washington.edu/CN=Configuration,DC=netid,DC=washington,DC=edu

I destroyed the ticket:

`kdestroy`

I viewed the default keytab file:

`klist -ke`

output:

    Keytab name: FILE:/etc/krb5.keytab
    KVNO Principal
    ---- --------------------------------------------------------------------------
       3 SPH-LOGIN0$@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 SPH-LOGIN0$@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 SPH-LOGIN0$@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 SPH-LOGIN0$@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)
       3 host/SPH-LOGIN0@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 host/SPH-LOGIN0@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 host/SPH-LOGIN0@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 host/SPH-LOGIN0@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)
       3 host/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 host/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 host/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 host/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)
       3 RestrictedKrbHost/SPH-LOGIN0@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 RestrictedKrbHost/SPH-LOGIN0@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 RestrictedKrbHost/SPH-LOGIN0@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 RestrictedKrbHost/SPH-LOGIN0@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)
       3 RestrictedKrbHost/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 RestrictedKrbHost/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 RestrictedKrbHost/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 RestrictedKrbHost/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)
       3 nfs/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes256-cts-hmac-sha1-96)
       3 nfs/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (aes128-cts-hmac-sha1-96)
       3 nfs/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:des3-cbc-sha1)
       3 nfs/sph-login0.hpc.sph.washington.edu@NETID.WASHINGTON.EDU (DEPRECATED:arcfour-hmac)

### configure automount

Note: In this case (for an HPC demo system), for now at least, local
scratch storage will be used for user home and project data directories
instead.

    cp -p /usr/share/pam-configs/mkhomedir /var/tmp/
    vi /usr/share/pam-configs/mkhomedir
    diff /var/tmp/mkhomedir /usr/share/pam-configs/mkhomedir
    7c7
    <   optional            pam_mkhomedir.so
    ---
    >   optional            pam_mkhomedir.so umask=007

    mkdir /scratch/users
    chmod 0775 /scratch/users
    ln -s /scratch/users /home/users
    ls -al /home/users
    lrwxrwxrwx 1 root root 14 Aug 12 15:23 /home/users -> /scratch/users

    pam-auth-update --enable mkhomedir
    grep 007 /etc/pam.d/*
    /etc/pam.d/common-session:session   optional            pam_mkhomedir.so umask=007

    mkdir /scratch/projects
    chgrp domain_users /scratch/projects/
    chmod 2770 /scratch/projects/
    ln -s /scratch/projects /projects
    ls -al /projects
    lrwxrwxrwx 1 root root 17 Aug 15 10:01 /projects -> /scratch/projects

For NFS-exported filesystems, I enabled automount as follows.

I made a copy of '/etc/auto.misc' and checked the file into RCS:

    cd /etc/

    mkdir RCS

    cp -p auto.misc auto.misc.release

    ci -i auto.misc

    co -l ./auto.misc

I didn't make any changes to the file at this time.

I made a copy of '/etc/auto.master' and checked the file into RCS:

    cp -p auto.master auto.master.release

    ci -i auto.master

    co -l auto.master

Then I edited the file and reviewed the updates:

    vi auto.master

    cat auto.master

'cat' output:

    /home  /etc/auto.home  --timeout=1200
    /projects  /etc/auto.projects  --timeout=1200

I added/edited the automount files corresponding to the locations
specified in the above 'auto.master' file as follows.

I edited auto.home:

    vi auto.home

    cat auto.home

'cat' output:

    # example
    #users     -rw,nosuid,nodev,bg,hard,intr,nfsvers=3 cbs-fs2-s.cbs.biostat.washington.edu:/mnt/tank/users

I set aside a copy:

    cp -p auto.home auto.home.orig

    ci -i auto.home

    co -l ./auto.home

I created an auto.projects file:

    vi auto.projects

    cat auto.projects

'cat' output:

    # example
    #*  -rw,nosuid,nodev,bg,hard,intr,nfsvers=3 cbs-fs2-s.cbs.biostat.washington.edu:/mnt/tank/cbs_admin/projects/&

I set aside a copy:

    cp -p auto.projects auto.projects.orig

    ci -i auto.projects

    co -l ./auto.projects

I reloaded and restarted 'autofs':

    systemctl reload autofs
    systemctl restart autofs
    systemctl status autofs

'systemctl status' output:

stop/disable autofs for now.

    systemctl stop autofs
    systemctl disable autofs

export /home/users and /projects for now.

    vi /etc/exports
    tail -3 /etc/exports
    # temporary exports for user homedirs and projects
    /home/users 10.66.62.240/21(async,rw,no_subtree_check)
    /projects 10.66.62.240/21(async,rw,no_subtree_check)
    exportfs -r

### modify sshd config

With netid auth enabled, I proceeded to secure ssh as follows.

I changed to the ssh configuration directory, checked in an
'sshd_config' file revision, and edited the file, explicitly enabling
admin and other selected group logins:

    cd /etc/ssh/

    mkdir RCS

    ci -i sshd_config

    co -l sshd_config

    vi sshd_config

    rcsdiff sshd_config

'rcsdiff' output:

    ===================================================================
    RCS file: RCS/sshd_config,v
    retrieving revision 1.1
    diff -r1.1 sshd_config
    122a123,125
    >
    > # allow access only for members of certain groups
    > AllowGroups root adm uw_sph_hpc_admins uw_sph_hpc_users

I restarted sshd:

`systemctl restart ssh`

### test auth and automount

I tested authentication (but not automount, since it isn't yet in use)
as follows.

I verified that user/group enumeration was working:

    cd

    id rmoulton

'id' output:

    uid=45068(rmoulton) gid=147849729(domain_users) groups=147849729(domain_users),1044389(uw_sph_hpc_users),1044502(uw_sph_hpc_admins)

In a separate ssh terminal, I logged on to the system with my own user
account:

`ssh sph-login0.hpc.sph.washington.edu`

output:

    rmoulton@sph-login0.hpc.sph.washington.edu's password:
    Creating directory '/home/users/rmoulton'.
    sph-login0.hpc.sph.washington.edu
    ------------------------

    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

I listed the location of my home dir (not auto-mounted in this case):

`pwd`

output:

    /home/users/rmoulton

I viewed homedir file system usage info:

`df -Th .`

'df' output:

    Filesystem     Type  Size  Used Avail Use% Mounted on
    /dev/md0p10    ext4  3.0T   52K  2.9T   1% /scratch

I viewed homedir contents:

`ls -al`

output (partial):

    total 24
    drwxrwx--- 3 rmoulton domain_users 4096 Aug 12 15:37 .
    drwxrwxr-x 3 root     root         4096 Aug 12 15:37 ..
    -rw-rw---- 1 rmoulton domain_users  220 Aug 12 15:37 .bash_logout
    -rw-rw---- 1 rmoulton domain_users 3771 Aug 12 15:37 .bashrc
    drwx------ 2 rmoulton domain_users 4096 Aug 12 15:37 .cache
    -rw-rw---- 1 rmoulton domain_users  807 Aug 12 15:37 .profile

I logged out:

`exit`

</pre>

Note: As a member of the multiple allowed groups, I was permitted to log
on.

I attempted to log on as a test user belonging to none of the permitted
groups. The attempt failed as expected:

`ssh -l tan18 sph-login0.hpc.sph.washington.edu`

'ssh' output:

    tan18@sph-login0.hpc.sph.washington.edu's password:
    Permission denied, please try again.
    tan18@sph-login0.hpc.sph.washington.edu's password:

Knowing that password-entry attempts would continue to fail, I did not
retry. I simply hit \[ctrl\]-\[c\] to cancel.

Back in the first ssh terminal, as root, I scanned the auth log and
found the user-not-allowed message corresponding to the attempt:

`grep AllowGroups /var/log/auth.log`

output:

    2024-08-12T22:43:29.139758+00:00 sph-login0 sshd[22137]: User tan18 from 10.104.148.72 not allowed because none of user's groups are listed in AllowGroups

I logged on to the system with my sadm_\* admin account:

`ssh -l sadm_rmoulton sph-login0.hpc.sph.washington.edu`

output:

    sadm_rmoulton@sph-login0.hpc.sph.washington.edu's password:
    Creating directory '/home/users/sadm_rmoulton'.
    sph-login0.hpc.sph.washington.edu
    ------------------------

    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

I verified that my sadm_\* admin user account had sudo access:

`sudo tail -2 /var/log/auth.log`

output:

    [sudo] password for sadm_rmoulton:
    2024-08-12T22:45:27.366114+00:00 sph-login0 sudo: sadm_rmoulton : TTY=pts/0 ; PWD=/scratch/users/sadm_rmoulton ; USER=root ; COMMAND=/usr/bin/tail -2 /var/log/auth.log
    2024-08-12T22:45:27.367908+00:00 sph-login0 sudo: pam_unix(sudo:session): session opened for user root(uid=0) by sadm_rmoulton(uid=1171819)

I became root to proceed with subsequent steps:

`sudo -i`

I returned to the first ssh terminal where I was still logged on as
maint, logged off, then verified that I could still log on:

`ssh -l maint sph-login0.hpc.sph.washington.edu`

output:

    maint@sph-login0.hpc.sph.washington.edu's password:
    sph-login0.hpc.sph.washington.edu
    ------------------------
    Last login: Mon Aug 12 14:14:27 2024 from 10.104.148.72

I logged off.

### install cron-apt

I installed and configured cron-apt as follows.

I installed the package:

    sudo -i

    apt install -y cron-apt
    < output snipped >

I added desired mail-and-log-related settings to the config file:

    echo 'MAILON="always"' >> /etc/cron-apt/config
    echo 'SYSLOGON="always"' >> /etc/cron-apt/config

    cat /etc/cron-apt/config

'cat' output:

    # Configuration for cron-apt. For further information about the possible
    # configuration settings see /usr/share/doc/cron-apt/README.gz.

    MAILON="always"
    SYSLOGON="always"

I created an action-related config file:

    echo 'dist-upgrade -y -o APT::Get::Show-Upgraded=true' > /etc/cron-apt/action.d/7-dist-upgrade

    cat /etc/cron-apt/action.d/7-dist-upgrade

'cat' output:

    dist-upgrade -y -o APT::Get::Show-Upgraded=true

Normally, I would to set the cron-apt cron job to run on the first Sat
of every month. In this case, as it may not be desirable to run
automatic backups on a user-facing compute system, I opted to disable it
for now.

    cp /etc/cron.d/cron-apt /var/tmp/

    vi /etc/cron.d/cron-apt

    diff /var/tmp/cron-apt /etc/cron.d/cron-apt

'diff' output:

    4,5c4,5
    < # Every night at 4 o'clock.
    < 0 4   * * *   root    test -x /usr/sbin/cron-apt && /usr/sbin/cron-apt
    ---
    > ## Every night at 4 o'clock.
    > ##0 4 * * *   root    test -x /usr/sbin/cron-apt && /usr/sbin/cron-apt
    9a10,11
    > # first Sat of month at 6am -- disabled for now
    > #00 06 * * 6     root    [ $(date +\%d) -le 07 ] && test -x /usr/sbin/cron-apt && /usr/sbin/cron-apt

I did a manual test run.

    /usr/bin/test -x /usr/sbin/cron-apt
    /usr/sbin/cron-apt

The test run took a minute or so and completed silently. An email
message was delivered promptly as expected.

### enable public key auth

\- nope, maybe later -

### enable config-file backups

\- nope, maybe later -

### generate keypair

For password-less root access from this cluster frontend to the compute
nodes under its control, a keypair will be needed. I generated it:


Note: At the 'file' and 'passphrase' prompts, I simply hit \[enter\]

`ssh-keygen -t ecdsa -b 521`

output:

    Generating public/private ecdsa key pair.
    Enter file in which to save the key (/root/.ssh/id_ecdsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /root/.ssh/id_ecdsa
    Your public key has been saved in /root/.ssh/id_ecdsa.pub
    The key fingerprint is:
    SHA256:yjnYtcYGY7SiW0uNFXciN0yxb8KqCfnlxe8g3mi4Wdk root@sph-login0.hpc.sph.washington.edu
    The key's randomart image is:
    +---[ECDSA 521]---+
    |        o.       |
    |       o .       |
    |      + B .      |
    |     . B =       |
    |    . * S o      |
    |   o X % +       |
    |  + =.& E        |
    |   *.X.O o       |
    |  . Bo+ ..o      |
    +----[SHA256]-----+

### install build deps

For R and its packages (as well as other software), I installed build
dependences as follows.


References:


<https://docs.rstudio.com/resources/install-r-source/>

<https://askubuntu.com/questions/1512042/ubuntu-24-04-getting-error-you-must-put-some-deb-src-uris-in-your-sources-list>

I temporarily enabled deb-src repositories by editing the apt
package-sources file, after setting aside a backup copy of the original:

    cp -p /etc/apt/sources.list.d/ubuntu.sources /var/tmp/

    vi /etc/apt/sources.list.d/ubuntu.sources

    diff /var/tmp/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources

'diff' output:

    11a12,17
    >
    > Types: deb-src
    > URIs: http://us.archive.ubuntu.com/ubuntu/
    > Suites: noble noble-updates noble-backports noble-proposed
    > Components: main restricted universe multiverse
    > Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

I refreshed package information:

`apt update`
`< output snipped >`

I initiated the R build-dependencies installation process:

`apt build-dep r-base`

output:

    Reading package lists... Done
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following NEW packages will be installed:
      at-spi2-common autoconf automake autopoint autotools-dev binutils binutils-common binutils-x86-64-linux-gnu bison
      build-essential bzip2 ca-certificates-java cpp cpp-13 cpp-13-x86-64-linux-gnu cpp-x86-64-linux-gnu dconf-gsettings-backend
      dconf-service debhelper debugedit default-jdk default-jdk-headless default-jre default-jre-headless dh-autoreconf
      dh-strip-nondeterminism dpkg-dev dwz fonts-lmodern g++ g++-13 g++-13-x86-64-linux-gnu g++-x86-64-linux-gnu gcc gcc-13
      gcc-13-base gcc-13-x86-64-linux-gnu gcc-x86-64-linux-gnu gettext gfortran gfortran-13 gfortran-13-x86-64-linux-gnu
      gfortran-x86-64-linux-gnu gir1.2-freedesktop gir1.2-freedesktop-dev gir1.2-glib-2.0-dev gir1.2-harfbuzz-0.0 gir1.2-pango-1.0
      icu-devtools intltool-debian java-common libapache-pom-java libarchive-zip-perl libasan8 libasound2-data libasound2t64
      libatk-bridge2.0-0t64 libatk1.0-0t64 libatomic1 libatspi2.0-0t64 libbinutils libblas-dev libblas3 libblkid-dev libbrotli-dev
      libbz2-dev libcairo-script-interpreter2 libcairo2-dev libcc1-0 libcolord2 libcommons-logging-java libcommons-parent-java
      libctf-nobfd0 libctf0 libcurl4-openssl-dev libdatrie-dev libdconf1 libdebhelper-perl libdeflate-dev libdpkg-perl
      libdrm-amdgpu1 libdrm-intel1 libdrm-nouveau2 libdrm-radeon1 libepoxy0 libexpat1-dev libffi-dev libfile-homedir-perl
      libfile-stripnondeterminism-perl libfile-which-perl libfontbox-java libfontconfig-dev libfontconfig1-dev libfontenc1
      libfreetype-dev libfribidi-dev libgcc-13-dev libgfortran-13-dev libgfortran5 libgif7 libgirepository-2.0-0 libgl1
      libgl1-mesa-dri libglapi-mesa libglib2.0-dev libglib2.0-dev-bin libglvnd0 libglx-mesa0 libglx0 libgomp1 libgprofng0
      libgraphite2-dev libgtk-3-0t64 libgtk-3-common libharfbuzz-cairo0 libharfbuzz-dev libharfbuzz-gobject0 libharfbuzz-icu0
      libharfbuzz-subset0 libhwasan0 libice-dev libicu-dev libisl23 libitm1 libjbig-dev libjpeg-dev libjpeg-turbo8-dev libjpeg8-dev
      libjs-jquery libkpathsea6 liblapack-dev liblapack3 liblcms2-2 liblerc-dev libllvm17t64 liblsan0 liblzma-dev
      libmime-charset-perl libmount-dev libmpc3 libncurses-dev libpango1.0-dev libpangoxft-1.0-0 libpaper-utils libpaper1
      libpciaccess0 libpcre2-16-0 libpcre2-32-0 libpcre2-dev libpcre2-posix3 libpcsclite1 libpdfbox-java libpixman-1-dev libpkgconf3
      libpng-dev libpotrace0 libptexenc1 libpthread-stubs0-dev libquadmath0 libreadline-dev libselinux1-dev libsepol-dev libsframe1
      libsharpyuv-dev libsm-dev libsombok3 libstdc++-13-dev libsub-override-perl libsynctex2 libteckit0 libtexlua53-5
      libtext-unidecode-perl libthai-dev libtiff-dev libtiffxx6 libtirpc-dev libtk8.6 libtool libtsan2 libubsan1
      libunicode-linebreak-perl libvulkan1 libwayland-client0 libwayland-cursor0 libwayland-egl1 libwebp-dev libwebpdecoder3
      libwebpdemux2 libwebpmux3 libx11-dev libx11-xcb1 libxau-dev libxcb-dri2-0 libxcb-dri3-0 libxcb-glx0 libxcb-present0
      libxcb-randr0 libxcb-render0-dev libxcb-shm0-dev libxcb-sync1 libxcb-xfixes0 libxcb1-dev libxcomposite1 libxcursor1
      libxdamage1 libxdmcp-dev libxext-dev libxfixes3 libxfont2 libxft-dev libxi6 libxkbfile1 libxml-libxml-perl
      libxml-namespacesupport-perl libxml-sax-base-perl libxml-sax-perl libxrandr2 libxrender-dev libxshmfence1 libxss-dev libxss1
      libxt-dev libxtst6 libxxf86vm1 libyaml-tiny-perl libzstd-dev libzzip-0-13t64 lmodern lto-disabled-list m4 make mpack
      openjdk-21-jdk openjdk-21-jdk-headless openjdk-21-jre openjdk-21-jre-headless pango1.0-tools pkgconf pkgconf-bin po-debconf
      preview-latex-style python3-packaging t1utils tcl8.6-dev tex-common texinfo texinfo-lib texlive-base texlive-binaries
      texlive-extra-utils texlive-fonts-extra texlive-fonts-recommended texlive-latex-base texlive-latex-extra
      texlive-latex-recommended texlive-luatex texlive-pictures texlive-plain-generic tk8.6 tk8.6-dev uuid-dev x11-xkb-utils
      x11proto-core-dev x11proto-dev xdg-utils xfonts-base xfonts-encodings xfonts-utils xorg-sgml-doctools xserver-common
      xtrans-dev xvfb zlib1g-dev
    0 upgraded, 273 newly installed, 0 to remove and 4 not upgraded.
    Need to get 1,156 MB of archives.
    After this operation, 3,304 MB of additional disk space will be used.
    Do you want to continue? [Y/n]

At the prompt, I entered 'Y'

Package installation proceeded to completion.

`< output snipped >`

I installed other desired packages:

    apt install -y \
    cmake icewm x2goserver x2goserver-xsession nfs-kernel-server libxml2-dev libssl-dev libpq-dev \
    unixodbc-dev libgdal-dev default-libmysqlclient-dev libmysqlclient-dev libudunits2-dev libnode-dev \
    libgsl-dev libopenblas-dev liblapack-dev liblapacke-dev pipx python3-pip python3-venv virtualenvwrapper \
    python3-pandas rclone environment-modules libglpk-dev python-is-python3
    < output snipped >

I moved the backup copy of the package-sources file back into place:

`cp -p /var/tmp/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources`

### install raid utilities

\- n/a (this system is using software raid) -

### install nrpe/nagios

\- maybe later? -

### enable logon banner

I enabled a logon banner as follows.

I created a banner file:

    cd /etc/ssh

    vi sshd_banner

    cat sshd_banner

'cat' output:

    This system is for authorized users only.  Any unauthorized access is
    prohibited and may result in administrative or legal action.  Authorized
    users are subject to UW School of Public Health system use policies.  Activity
    may be monitored and/or recorded to ensure the activity is authorized, for
    system management, to facilitate protection against unauthorized access,
    and to verify security procedures.  System records may be audited, copied,
    inspected and/or disclosed to UW or law enforcement personnel.  Therefore
    no user, authorized or not, has any expectation of privacy.

    DISCONNECT IMMEDIATELY if you do not agree to these terms and conditions

I updated the sshd configuration file:

    ci -u sshd_config

    co -l sshd_config

    vi sshd_config

    rcsdiff sshd_config

'rcsdiff' output:

    ===================================================================
    RCS file: RCS/sshd_config,v
    retrieving revision 1.2
    diff -r1.2 sshd_config
    109c109
    < #Banner none
    ---
    > Banner /etc/ssh/sshd_banner

I restarted sshd:

`systemctl restart ssh`

I also updated the motd file (to include the system namesake
information, etc ...):

    cd /etc/

    cp -p motd motd.orig

    ci -u motd

    co -l ./motd

    vi motd

    cat motd

'cat' output:

    sph-login0.hpc.sph.washington.edu
    (login.hpc.sph.washington.edu)
    ------------------------

To verify, I did a new login:

`ssh login.hpc.sph.washington.edu`

ssh output:

    This system is for authorized users only.  Any unauthorized access is
    prohibited and may result in administrative or legal action.  Authorized
    users are subject to UW School of Public Health system use policies.  Activity
    may be monitored and/or recorded to ensure the activity is authorized, for
    system management, to facilitate protection against unauthorized access,
    and to verify security procedures.  System records may be audited, copied,
    inspected and/or disclosed to UW or law enforcement personnel.  Therefore
    no user, authorized or not, has any expectation of privacy.

    DISCONNECT IMMEDIATELY if you do not agree to these terms and conditions

    rmoulton@login.hpc.sph.washington.edu's password:
    sph-login0.hpc.sph.washington.edu
    (login.hpc.sph.washington.edu)
    ------------------------

    Last login: Mon Aug 12 15:37:17 2024 from 10.104.148.72

I logged off:

`exit`

### install cluster utils

I installed and configured some cluster-specific utilities.

#### mariadb

MariaDB is required for Slurm. I installed it ...


Note: 'libmariadb-dev' \*may\* be needed in some cases.

<!-- -->

    apt -y install mariadb-server
    < output snipped >

... then I ran the secure-installation script:

`mysql_secure_installation`

input/output:

    NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
          SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

    In order to log into MariaDB to secure it, we'll need the current
    password for the root user. If you've just installed MariaDB, and
    haven't set the root password yet, you should just press enter here.

    Enter current password for root (enter for none):
    OK, successfully used password, moving on...

    Setting the root password or using the unix_socket ensures that nobody
    can log into the MariaDB root user without the proper authorisation.

    You already have your root account protected, so you can safely answer 'n'.

    Switch to unix_socket authentication [Y/n] n
     ... skipping.

    You already have your root account protected, so you can safely answer 'n'.

    Change the root password? [Y/n]
    New password:
    Re-enter new password:
    Password updated successfully!
    Reloading privilege tables..
     ... Success!

    By default, a MariaDB installation has an anonymous user, allowing anyone
    to log into MariaDB without having to have a user account created for
    them.  This is intended only for testing, and to make the installation
    go a bit smoother.  You should remove them before moving into a
    production environment.

    Remove anonymous users? [Y/n]
     ... Success!

    Normally, root should only be allowed to connect from 'localhost'.  This
    ensures that someone cannot guess at the root password from the network.

    Disallow root login remotely? [Y/n]
     ... Success!

    By default, MariaDB comes with a database named 'test' that anyone can
    access.  This is also intended only for testing, and should be removed
    before moving into a production environment.

    Remove test database and access to it? [Y/n]
     - Dropping test database...
     ... Success!
     - Removing privileges on test database...
     ... Success!

    Reloading the privilege tables will ensure that all changes made so far
    will take effect immediately.

    Reload privilege tables now? [Y/n]
     ... Success!

    Cleaning up...

    All done!  If you've completed all of the above steps, your MariaDB
    installation should now be secure.

    Thanks for using MariaDB!

Note: MariaDB will be configured for Slurm use later.

#### munge

Munge is required for Slurm. I installed it as follows.

I created a group and user:

    groupadd -g 326 munge
    useradd -u 326 -g munge -d /nonexistent -M -s /usr/sbin/nologin munge

'useradd' output:

`useradd warning: munge's uid 326 outside of the UID_MIN 1000 and UID_MAX 60000 range.`

I installed packages:

`apt install -y munge libmunge-dev`
`< output snipped >`

I viewed munge status:

`systemctl status munge`

output:

    ● munge.service - MUNGE authentication service
         Loaded: loaded (/usr/lib/systemd/system/munge.service; enabled; preset: enabled)
         Active: active (running) since Mon 2024-08-12 16:29:07 PDT; 11s ago
           Docs: man:munged(8)
        Process: 37584 ExecStart=/usr/sbin/munged $OPTIONS (code=exited, status=0/SUCCESS)
       Main PID: 37586 (munged)
          Tasks: 4 (limit: 309306)
         Memory: 676.0K (peak: 2.3M)
            CPU: 7ms
         CGroup: /system.slice/munge.service
                 └─37586 /usr/sbin/munged

    Aug 12 16:29:07 sph-login0.hpc.sph.washington.edu systemd[1]: Starting munge.service - MUNGE authentication service...
    Aug 12 16:29:07 sph-login0.hpc.sph.washington.edu (munged)[37584]: munge.service: Referenced but unset environment variable evalu>
    Aug 12 16:29:07 sph-login0.hpc.sph.washington.edu systemd[1]: Started munge.service - MUNGE authentication service.

I viewed munge-directory ownership and permissions:

`ls -al /etc/munge/`

output:

    total 20
    drwx------   2 munge munge  4096 Aug 12 16:29 .
    drwxr-xr-x 149 root  root  12288 Aug 12 16:29 ..
    -rw-------   1 munge munge   128 Aug 12 16:29 munge.key

I viewed numeric user and group IDs:

`ls -aln /etc/munge/`

output:

    total 20
    drwx------   2 326 326  4096 Aug 12 16:29 .
    drwxr-xr-x 149   0   0 12288 Aug 12 16:29 ..
    -rw-------   1 326 326   128 Aug 12 16:29 munge.key

I increased the default number of threads (2) as follows:

I set aside a copy of the munge configuration file, then I edited the
original file:

    cp -p /etc/default/munge /etc/default/munge.release

    vi /etc/default/munge

    diff /etc/default/munge.release /etc/default/munge

'diff' output:

    4a5
    > OPTIONS="--num-threads=10"

I restarted the daemon:

`systemctl restart munge`

I scanned the munge log for thread-related messages:

`grep thread /var/log/munge/munged.log`

output:

    2024-08-12 16:29:07 -0700 Info:      Created 2 work threads
    2024-08-12 16:31:01 -0700 Info:      Created 10 work threads

In order to make the munge configuration directory (containing the key
file) available on compute-node systems, I set up an nfs export for the
cluster-control interface as follows.

I identified the interface:

`host sph-login0-c.hpc.sph.washington.edu`

output:

`sph-login0-c.hpc.sph.washington.edu has address 10.66.62.240`

I added an export:

    vi /etc/exports

    tail -1 /etc/exports

'tail' output:

    /etc/munge 10.66.62.240/21(async,rw,no_subtree_check)

I enabled the export:

    exportfs -r

    showmount -e localhost

'showmount' output:

    Export list for localhost:
    /etc/munge 10.66.62.240/21

#### go

Go is needed for compilation of some essential applications. I installed
it as follows.


References:


<https://docs.sylabs.io/guides/latest/admin-guide/installation.html#install-go>

<https://go.dev/dl/>

After identifying the current version, I downloaded, unpacked, and
installed it ...

    export VERSION=1.22.4 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    rm go$VERSION.$OS-$ARCH.tar.gz

... then I set its environment:

    echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
    source ~/.bashrc

#### shared-software mount

Some applications are expected to be compiled and installed in
/usr/local. In order to make that directory available on compute-node
systems, I set up an nfs export:

    vi /etc/exports

    tail -1 /etc/exports

'tail' output:

    /usr/local 10.66.62.240/21(async,rw,no_subtree_check)

I enabled the export:

    exportfs -r

    showmount -e localhost

'showmount' output:

    Export list for localhost:
    /usr/local 10.66.63.141/21
    /etc/munge 10.66.63.141/21

#### slurm

I installed and configured Slurm:


[Slurm-23.11.6.sph-login0](Slurm-23.11.6.sph-login0 "wikilink")

#### apache

For various web services (ubuntu package repo, Grafana, other?), I
installed apache:


[Apache.install.sph-login0](Apache.install.sph-login0 "wikilink")

#### grafana

I installed and configured Grafana (and Prometheus):


[Grafana%2BPrometheus.install.sph-login0](Grafana%2BPrometheus.install.sph-login0 "wikilink")

### install essential software

I installed some essential research software.

#### mkl

I installed and configured oneMKL:


[MKL-2022.0.2.cbs](MKL-2022.0.2.cbs "wikilink")

#### mpi

I installed and configured Open MPI:


[Openmpi-4.1.4.cbs](Openmpi-4.1.4.cbs "wikilink")

#### singularity

I installed and configured Singularity


[Singularity-4.1.4.cbs](Singularity-4.1.4.cbs "wikilink")

#### R

I installed and configured R:


[R-4.3.3.cbs](R-4.3.3.cbs "wikilink")

#### texlive

I installed texlive:


[Texlive-2024.cbs](Texlive-2024.cbs "wikilink")

#### emacs

I installed emacs and ESS:


[Emacs-29.1.cbs](Emacs-29.1.cbs "wikilink")

### install fai

I installed and configured FAI:


[FAI-setup.sph-login0](FAI-setup.sph-login0 "wikilink")

### build nodes

I (re)built compute nodes. This process is logged separately.


[Nodes-install.all.2024.cbs-divid](Nodes-install.all.2024.cbs-divid "wikilink")

### install additional software

I installed additional research software as requested/needed.

#### bcftools

I installed bcftools:


[Bcftools-1.17.cbs](Bcftools-1.17.cbs "wikilink")

#### htslib

I installed htslib:


[Htslib-1.17.cbs](Htslib-1.17.cbs "wikilink")

#### plink1

I installed plink1:


[Plink.1.90b7.cbs](Plink.1.90b7.cbs "wikilink")

#### plink2

I installed plink2:


[Plink.2.00a6LM.cbs](Plink.2.00a6LM.cbs "wikilink")

[Category:SPH-LOGIN-MISC](Category:SPH-LOGIN-MISC "wikilink")