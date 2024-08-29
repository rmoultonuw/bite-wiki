[Rmoulton](User:Rmoulton "wikilink")
([talk](User_talk:Rmoulton "wikilink")) 11:37, 15 August 2024 (PDT)

## Hardware information


References:


[Install-clients.p93-p96#Hardware_information](Install-clients.p93-p96#Hardware_information "wikilink")

## Problem

Four compute nodes need to be re-built for this newly-built cluster.

## Solution

Configure and build the client nodes

## Notes

MAC addresses were identified previously, recorded in the installation
logs above.

## Steps taken

### pre-create accounts

Netid accounts and SPNs were created previously.


Reference:
[Ubuntu-24.04.sph-infra0#pre-create_account](Ubuntu-24.04.sph-infra0#pre-create_account "wikilink")

### update dhcp

I added new host entries to dhcpd.conf:

    vi /etc/dhcp/dhcpd.conf

    tail -4 /etc/dhcp/dhcpd.conf

'tail' output:

    host sph-n01 {hardware ethernet 0c:c4:7a:03:ee:10;fixed-address sph-n01;}
    host sph-n02 {hardware ethernet 0c:c4:7a:03:ee:8e;fixed-address sph-n02;}
    host sph-n03 {hardware ethernet 0c:c4:7a:03:ed:f0;fixed-address sph-n03;}
    host sph-n04 {hardware ethernet 0c:c4:7a:03:ee:34;fixed-address sph-n04;}

I added new host entries to the hosts file:

    vi /etc/hosts

    tail -4 /etc/hosts

'tail' output:

    10.66.62.1    sph-n01-c.hpc.sph.washington.edu    sph-n01
    10.66.62.2    sph-n02-c.hpc.sph.washington.edu    sph-n02
    10.66.62.3    sph-n03-c.hpc.sph.washington.edu    sph-n03
    10.66.62.4    sph-n04-c.hpc.sph.washington.edu    sph-n04

I restarted the dhcp service:

`systemctl restart isc-dhcp-server`

### update ssh

I identified the previously-generated ssh-ed25519 cluster public key:

`cat /etc/ssh/cluster-keys/ssh_host_ed25519_key.pub`

'cat' output:

`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE8RmSqeBOWNSAMZRC/xG2AnZ78EbeLeoi38LZIsvKZe sph-login0-cluster-key`

I edited ssh_known_hosts, adding host-specific entries for that key:

    vi /etc/ssh/ssh_known_hosts

    tail -4 /etc/ssh/ssh_known_hosts

'tail' output:

    sph-n01 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE8RmSqeBOWNSAMZRC/xG2AnZ78EbeLeoi38LZIsvKZe sph-login0-cluster-key
    sph-n02 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE8RmSqeBOWNSAMZRC/xG2AnZ78EbeLeoi38LZIsvKZe sph-login0-cluster-key
    sph-n03 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE8RmSqeBOWNSAMZRC/xG2AnZ78EbeLeoi38LZIsvKZe sph-login0-cluster-key
    sph-n04 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE8RmSqeBOWNSAMZRC/xG2AnZ78EbeLeoi38LZIsvKZe sph-login0-cluster-key

I copied the newly-updated file to the compute-node deployment
environment:

`cp /etc/ssh/ssh_known_hosts /srv/fai/config/files/etc/ssh/ssh_known_hosts/COMPUTE`

### update dns

On the DNS server, corresponding records were added previously.


Reference:
[Ubuntu-24.04.sph-infra0#create_DNS_records](Ubuntu-24.04.sph-infra0#create_DNS_records "wikilink")

### update slurm

Node and partition settings were added to the slurm configuration file
previously, but in a disabled state.


Reference:
[Slurm-23.11.6.sph-login0#main_config](Slurm-23.11.6.sph-login0#main_config "wikilink")

`tail -2 /opt/slurm/current/etc/slurm.conf`

output:

    #NodeName=sph-n[01-04] CPUs=12 Boards=1 SocketsPerBoard=2 CoresPerSocket=6 ThreadsPerCore=1 RealMemory=128876 State=UNKNOWN
    #PartitionName=12c128g Nodes=sph-n[01-04] Default=YES DefMemPerCPU=10740 MaxMemPerCPU=128876 DefaultTime=30-00:00:00 MaxTime=INFINITE State=UP AllowGroups=all

I edited the file to enable the nodes and partition:

    vi /opt/slurm/current/etc/slurm.conf
    tail -2 /opt/slurm/current/etc/slurm.conf

output:

    NodeName=sph-n[01-04] CPUs=12 Boards=1 SocketsPerBoard=2 CoresPerSocket=6 ThreadsPerCore=1 RealMemory=128876 State=UNKNOWN
    PartitionName=12c128g Nodes=sph-n[01-04] Default=YES DefMemPerCPU=10740 MaxMemPerCPU=128876 DefaultTime=30-00:00:00 MaxTime=INFINITE State=UP AllowGroups=all

Note: Based on previous experience I already knew the appropriate
hardware settings for the nodes; otherwise I would have left them unset
and updated them later, after using the following command to view the
actual hardware configuration on a specific node itself:

`slurmd -C`

I restarted the slurm controller:

`systemctl restart slurmctld`

I instructed slurm daemons on compute nodes to re-read the config file:

`scontrol reconfigure`

I created boot-configuration files for the hosts:

    sudo -u fai fai-chboot -IPv -f verbose,sshd,reboot -u nfs://sph-login0-c.hpc.sph.washington.edu/srv/fai/config \
    sph-n01 sph-n02 sph-n03 sph-n04

output:

    Booting kernel vmlinuz-6.1.0-23-amd64
     append initrd=initrd.img-6.1.0-23-amd64 ip=dhcp
       FAI_FLAGS=verbose,sshd,reboot FAI_CONFIG_SRC=nfs://sph-login0-c.hpc.sph.washington.edu/srv/fai/config

    sph-n01 has 10.66.62.1 in hex 0A423E01
    Writing file /srv/tftp/fai/pxelinux.cfg/0A423E01 for sph-n01
    sph-n02 has 10.66.62.2 in hex 0A423E02
    Writing file /srv/tftp/fai/pxelinux.cfg/0A423E02 for sph-n02
    sph-n03 has 10.66.62.3 in hex 0A423E03
    Writing file /srv/tftp/fai/pxelinux.cfg/0A423E03 for sph-n03
    sph-n04 has 10.66.62.4 in hex 0A423E04
    Writing file /srv/tftp/fai/pxelinux.cfg/0A423E04 for sph-n04

I viewed the host configuration list:

`fai-chboot -l`

output:

    [DEFAULT]                   default                NOACTION localboot -1
    sph-n01-c                   0A423E01               install vmlinuz-6.1.0-23-amd64
    sph-n02-c                   0A423E02               install vmlinuz-6.1.0-23-amd64
    sph-n03-c                   0A423E03               install vmlinuz-6.1.0-23-amd64
    sph-n04-c                   0A423E04               install vmlinuz-6.1.0-23-amd64

### set admin pw

For an unattended domain join, a domain-admin username is used in a
'firstboot' script. A password file gets copied into place at build
time, then when the firstboot script runs, a domain-join command reads
the password from that file and the file is subsequently deleted:

`grep secret /usr/local/sbin/firstboot.sh`

'grep' output:

    printf "%b" "$(</etc/sadm.secret)" | /usr/sbin/adcli join -U sadm_rmoulton --stdin-password netid.washington.edu
    rm -f /etc/sadm.secret

I temporarily updated the corresponding build-environment 'sadm.secret'
file to include my password:

    cp /srv/fai/config/files/etc/sadm.secret/COMPUTE /srv/fai/config/files/etc/sadm.secret/placeholder

    vi /srv/fai/config/files/etc/sadm.secret/COMPUTE

    cat /srv/fai/config/files/etc/sadm.secret/COMPUTE

'cat' output:

`<-my-actual-password->`

### (re)boot

On cbs-david, the frontend currently controlling the nodes, I stopped
the dhcp server service:

`systemctl stop isc-dhcp-server`

I set nodes to 'drain' state:

`scontrol update nodename=cbs-d[01-04] state=drain reason=migrating`
`sinfo -l`

When 'sinfo' reported node state as 'drained', I connected to each node
and rebooted. for example:

    ssh cbs-d01
    efibootmgr

Note: It's not applicable in this case, but for efi nodes, next boot
device must be set:

`efibootmgr -n `<first-listed-network-device>

Reboot:

`reboot`

### adjust bios settings

\- n/a -- settings were adjusted previously -

In the BIOS, I viewed/adjusted settings as follows:


Advanced menu


Boot Feature:


Quiet Boot \[Disable\]

CPU Configuration:


Hyper-Threading \[Disable\]

PCIe/PCI/PnP Configuration:


Onboard LAN 1 Option ROM {EFI\]

<!-- -->


Boot menu:


Boot Mode Select \[UEFI\]

Boot Option \#1 \[UEFI Network\]

Boot Option \#2 \[UEFI Hard Disk\]

Then I saved the BIOS settings and exited

The system rebooted

### build

The node automatically retrieved installation images from the head node,
reformatted partitions, installed the operating system and necessary
software, ran post-installation scripts, and rebooted. The process took
about 10 minutes.

### optional: observe build process

Note: I didn't perform this step for every node.

I noted that after a minute or two, the host started responding to
'ping':

`ping -c1 sph-n01`

'ping' output:

I connected to the host (entering the default FAI build-time password at
the prompt) ...

`ssh sph-n01`

'ssh' output:

... and watched the log output as the host was built:

`tail -f /tmp/fai/fai.log`

'tail' output (partial, at conclusion):


Note: The sshd banner output (snipped below) was generated three times,
for ssh connections back to the FAI server -- in order to disable pxe
boot and save log files.

<!-- -->

The host then rebooted itself.

### optional: check build

Note: I didn't perform this step for every node.

I verified that the host-specific boot configuration file was disabled
and log files were saved as expected:

`fai-chboot -l sph-n01`

output:

`ls -al /var/log/fai/remote-logs/sph-n01/`

output:

I viewed the error log:

`cat /var/log/fai/remote-logs/sph-n01/last/error.log`

output:

After the host rebooted, I logged on as 'maint', entering the
appropriate password at the prompt:

`ssh -l maint sph-n01`

output:

I verified that auth was working:

`id rmoulton`

output:

I logged off:

`exit`

### optional: submit test job

Note: I didn't perform this step for every node.

I submitted a test job as follows.

I logged on as myself:

`ssh login0.hpc.sph.washington.edu`

Previously, I created a test script:


sleep10.sh

Its contents:

    #!/bin/bash
    # sleep for 10 seconds
    sleep 10
    # report hostname etc ...
    hostname
    uptime
    free -h

I viewed Slurm node-and-partition info:

`sinfo -l`

output:

I submitted the aforementioned shell script to the newly-built node:

`sbatch -w sph-n01 -p 12c128g sleep10.sh`


Note: It isn't strictly necessary a node, and it's necessary to specify
the partition only if it isn't the default (as identified by the
asterisk beside its name in 'sinfo' output).

output:

`Submitted batch job 1`

I viewed queue status, noting that the job had commenced running:

`squeue -l`

'squeue' output:

The job completed after ten seconds, as expected. I viewed its output:

`cat slurm-1.out`

'cat' output:

I logged off:

`exit`

### repeat for other nodes

For the other nodes, I repeated the above steps, starting with
[boot](#boot "wikilink")

### enable monitoring

I enabled compute-node monitoring as follows.


Reference:


<https://documentation.suse.com/sle-hpc/15-SP4/html/hpc-guide/cha-monitoring.html>

I edited the prometheus config file, adding all of the compute nodes as
targets for the node-explorer job.

    vi /etc/prometheus/prometheus.yml

    cat /etc/prometheus/prometheus.yml

'cat' output (partial):

    [...]
      - job_name: node-exporter
        # If prometheus-node-exporter is installed, grab stats about the local
        # machine by default.
        static_configs:
          - targets: ['sph-login0:9100']
          - targets: ['sph-n01:9100']
          - targets: ['sph-n02:9100']
          - targets: ['sph-n03:9100']
          - targets: ['sph-n04:9100']
    [...]

I restarted prometheus server and the node-explorer service:

`systemctl restart prometheus prometheus-node-exporter`

### unset admin pw

I removed my password from the admin password file:

    cp /srv/fai/config/files/etc/sadm.secret/placeholder /srv/fai/config/files/etc/sadm.secret/COMPUTE

    cat /srv/fai/config/files/etc/sadm.secret/COMPUTE

'cat' output:

`<- placeholder for sadm_* password ->`

### optional: submit mpi test job

Once all of the nodes were installed, I submitted an openmpi test job as
follows.

I established a new ssh connection to the cluster frontend:

`ssh login0.hpc.sph.washington.edu`

Previously, in an interactive R session, I installed the Rmpi library:

    > install.packages("Rmpi", configure.args="--with-Rmpi-include=/usr/local/openmpi/current/include --with-Rmpi-libpath=/usr/local/openmpi/current/lib --with-Rmpi-type=OPENMPI")
    < snipped output >

I also created an R script ...


rmpi_test.R

Its contents:

    #!/usr/local/bin/Rscript
    # https://help.rc.ufl.edu/doc/R_MPI_Example
    # Load the R MPI package if it is not already loaded.
    if (!is.loaded("mpi_initialize")) {
        library("Rmpi")
        }

    ns <- mpi.universe.size() - 1
    mpi.spawn.Rslaves(nslaves=ns, needlog=FALSE)
    #
    # In case R exits unexpectedly, have it automatically clean up
    # resources taken up by Rmpi (slaves, memory, etc...)
    .Last <- function(){
           if (is.loaded("mpi_initialize")){
               if (mpi.comm.size(1) > 0){
                   print("Please use mpi.close.Rslaves() to close slaves.")
                   mpi.close.Rslaves()
               }
               print("Please use mpi.quit() to quit R")
               .Call("mpi_finalize")
           }
    }
    # Tell all slaves to return a message identifying themselves
    mpi.bcast.cmd( id <- mpi.comm.rank() )
    mpi.bcast.cmd( ns <- mpi.comm.size() )
    mpi.bcast.cmd( host <- mpi.get.processor.name() )
    mpi.remote.exec(paste("I am",mpi.comm.rank(),"of",mpi.comm.size()))

    # Test computations
    x <- 5
    x <- mpi.remote.exec(rnorm, x)
    length(x)
    x

    # Tell all slaves to close down, and exit the program
    mpi.close.Rslaves(dellog = FALSE)
    mpi.quit()

... and an mpi-specific job submission script:


slurm_submit_rmpi.sh

Its contents:

    #!/bin/sh
    # https://help.rc.ufl.edu/doc/R_MPI_Example
    #SBATCH --cpus-per-task=1 # Number of cores per MPI rank
    #SBATCH --ntasks=31 # Number of MPI ranks
    pwd; hostname; date
    mpirun -np 1 R CMD BATCH -q --no-save rmpi_test.R rmpi_test.Rout
    date

I submitted the job:

`sbatch slurm_submit_rmpi.sh`

output:

`Submitted batch job 101`

I viewed status:

`squeue -l`

output:

    Thu Aug 29 11:03:59 2024
                 JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
                   101   12c128g slurm_su rmoulton  RUNNING       0:04 30-00:00:00      3 sph-n[01-03]

The job completed in a matter of seconds, as expected. I viewed the
corresponding job-submission script output file:

`cat slurm-2.out`

output:

    /home/users/rmoulton
    sph-n01
    Thu Aug 29 11:03:55 AM PDT 2024
    Thu Aug 29 11:03:59 AM PDT 2024

Then I viewed the R script output file:

`cat rmpi_test.Rout`

output:

    > #!/usr/local/bin/Rscript
    > # https://help.rc.ufl.edu/doc/R_MPI_Example
    > # Load the R MPI package if it is not already loaded.
    > if (!is.loaded("mpi_initialize")) {
    +     library("Rmpi")
    +     }
    >
    > ns <- mpi.universe.size() - 1
    > mpi.spawn.Rslaves(nslaves=ns, needlog=FALSE)
        30 slaves are spawned successfully. 0 failed.
    master  (rank 0 , comm 1) of size 31 is running on: sph-n01
    slave1  (rank 1 , comm 1) of size 31 is running on: sph-n01
    slave2  (rank 2 , comm 1) of size 31 is running on: sph-n01
    slave3  (rank 3 , comm 1) of size 31 is running on: sph-n01
    ... ... ...
    slave29 (rank 29, comm 1) of size 31 is running on: sph-n03
    slave30 (rank 30, comm 1) of size 31 is running on: sph-n03
    > #
    > # In case R exits unexpectedly, have it automatically clean up
    > # resources taken up by Rmpi (slaves, memory, etc...)
    > .Last <- function(){
    +        if (is.loaded("mpi_initialize")){
    +            if (mpi.comm.size(1) > 0){
    +                print("Please use mpi.close.Rslaves() to close slaves.")
    +                mpi.close.Rslaves()
    +            }
    +            print("Please use mpi.quit() to quit R")
    +            .Call("mpi_finalize")
    +        }
    + }
    > # Tell all slaves to return a message identifying themselves
    > mpi.bcast.cmd( id <- mpi.comm.rank() )
    > mpi.bcast.cmd( ns <- mpi.comm.size() )
    > mpi.bcast.cmd( host <- mpi.get.processor.name() )
    > mpi.remote.exec(paste("I am",mpi.comm.rank(),"of",mpi.comm.size()))
    $slave1
    [1] "I am 1 of 31"

    $slave2
    [1] "I am 2 of 31"

    $slave3
    [1] "I am 3 of 31"

    $slave4
    [1] "I am 4 of 31"

    $slave5
    [1] "I am 5 of 31"

    $slave6
    [1] "I am 6 of 31"

    $slave7
    [1] "I am 7 of 31"

    $slave8
    [1] "I am 8 of 31"

    $slave9
    [1] "I am 9 of 31"

    $slave10
    [1] "I am 10 of 31"

    $slave11
    [1] "I am 11 of 31"

    $slave12
    [1] "I am 12 of 31"

    $slave13
    [1] "I am 13 of 31"

    $slave14
    [1] "I am 14 of 31"

    $slave15
    [1] "I am 15 of 31"

    $slave16
    [1] "I am 16 of 31"

    $slave17
    [1] "I am 17 of 31"

    $slave18
    [1] "I am 18 of 31"

    $slave19
    [1] "I am 19 of 31"

    $slave20
    [1] "I am 20 of 31"

    $slave21
    [1] "I am 21 of 31"

    $slave22
    [1] "I am 22 of 31"

    $slave23
    [1] "I am 23 of 31"

    $slave24
    [1] "I am 24 of 31"

    $slave25
    [1] "I am 25 of 31"

    $slave26
    [1] "I am 26 of 31"

    $slave27
    [1] "I am 27 of 31"

    $slave28
    [1] "I am 28 of 31"

    $slave29
    [1] "I am 29 of 31"

    $slave30
    [1] "I am 30 of 31"

    >
    > # Test computations
    > x <- 5
    > x <- mpi.remote.exec(rnorm, x)
    > length(x)
    [1] 30
    > x
              X1         X2         X3         X4          X5         X6
    1  0.4527765 -1.2713201 -0.5529722  0.9795176 -0.72681922  0.7677450
    2 -0.5315925 -0.8534243  0.3866902 -0.2568335  0.07200418 -0.5471387
    3  2.0968943 -2.0771795 -0.7663688 -0.3174076 -1.76808005  1.2241250
    4 -0.2749540 -0.4712339  2.3790819  0.2184472 -0.54804727 -2.0985974
    5 -0.2395582 -1.8740173  0.9720815 -1.4239929 -1.53936682  0.7326686
               X7         X8         X9        X10        X11        X12
    1  0.07214603  0.4064177 -0.3121091  0.8932493  0.5601281 -0.9727058
    2 -1.06938726  1.4056680 -0.3864899 -0.3139857 -0.1644438  0.1926692
    3  2.30406649 -1.1762766 -0.5486499  1.3604813 -0.8711911  0.2742145
    4 -0.08445056 -0.0997993 -0.6248848  0.3054629  1.8956942  2.2189375
    5  0.47914152  0.8570709  0.5409668  1.2269269  1.5826063 -1.5216554
              X13       X14        X15        X16        X17        X18        X19
    1  0.08801928 1.3666491  0.2233125 -0.1769483 -0.9151041  0.8463054  0.6440569
    2  1.71560820 0.2179393  2.9392765 -0.3530031  0.1271278  0.9074445  0.1738000
    3 -1.42677175 1.3563829 -0.9349077  2.0552006 -1.9972940 -0.9594836 -1.0264989
    4 -1.66467873 1.3820691 -1.6240477  0.7250194 -1.4089614 -1.4155622 -0.8984991
    5  0.39986914 1.1467088  2.3248433  0.6890700 -2.0523212 -0.1382636  0.1130580
             X20        X21        X22         X23         X24        X25
    1  1.8422408 -0.7457996 -0.4100347 -0.19076801 -0.01130255  0.3982388
    2 -1.8270326  0.8664691 -0.9985587 -0.49640967 -0.09259170 -1.0211236
    3  0.1147372 -0.5297800 -0.9758665  0.07817474 -0.32427744 -3.0457362
    4 -2.1597716 -1.2357627  0.3512924  0.33691228 -1.14008621  2.7162036
    5 -0.6120608 -1.3122395 -0.6929693 -0.40454961 -0.19873503  1.5979552
             X26         X27        X28          X29        X30
    1 -1.7387331 -0.42906042 -0.5443837 -1.275127745 -0.8024483
    2  1.2383767 -0.68663406  1.5279261 -0.643364003 -1.5053308
    3  0.7407305  1.18597093  0.7042790 -0.068175053  1.0153076
    4  1.8780751  1.48668315 -0.3510609  0.005698508 -0.5331999
    5 -0.5741911  0.06188824 -0.1800684 -0.140607309  1.0748247
    >
    > # Tell all slaves to close down, and exit the program
    > mpi.close.Rslaves(dellog = FALSE)
    [1] 1
    > mpi.quit()

### optional: submit interactive test job

Once all of the nodes were installed, I submitted an interactive jupyter
test job as follows.


References:


[Media:jupyter-srun-random-port-miniconda.txt](Media:jupyter-srun-random-port-miniconda.txt "wikilink")

<https://wiki.gacrc.uga.edu/wiki/Jupyter-Teaching>

<!-- -->


Prerequisite: Jupyter must be installed. Miniconda can be used for that.
For example:

<!-- -->

    cd
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh -b -u
    rm -f Miniconda3-latest-Linux-x86_64.sh
    echo 'export PATH="/home/users/rmoulton/miniconda3/bin:$PATH"' >> .bashrc
    source .bashrc
    pip install jupyterlab notebook

I established a new ssh connection to the cluster frontend:

I launched a detachable 'screen' session:

`screen`

In the screen session, I started an interactive 'salloc' job:

`salloc`

output (example):


Note that alternatively, 'srun' can be used:

`srun --pty /bin/bash`

For this newly-launched interactive session, I set IP and PORT
(randomized) variables ...

`IP=$(hostname -f)`
`PORT=$(shuf -i 59600-59699 -n 1)`

... then I started a jupyter notebook server session:

`jupyter-notebook --port $PORT --ip=$IP --no-browser`

output (example):

After making note of the port number referenced in the above output
(59670), from my own computer I established a second ssh connection to
the cluster frontend, with appropriate port-forwarding options
specified:

On my own computer, I opened a new browser tab and entered the url (with
port number and token specified, from above output):



The jupyter server screen loaded.

I didn't do any further testing. I simply went to:


"Files" menu -\> \[Shut Down\]

At the pop-up warning message ...


Shutdown confirmation

Please confirm you want to shut down Jupyter Notebook.


\[Cancel\] \[Shut Down\]

... I clicked \[Shut Down\]

A pop-up confirmation message was displayed:


Server stopped


You have shut down the Jupyter server. You can now close this tab.

To use Jupyter Notebook again, you will need to relaunch it.

I closed the browser tab.

Back in my first login session, I noted the new jupyter session output:

I terminated the 'salloc' job:

`exit`

output (example):

    exit
    salloc: Relinquishing job allocation 3
    <pre>

    I terminated the 'screen' session:
     exit

    output:
     [screen is terminating]

    I logged off:
     exit

    output:
    <pre>

In my second login session, I noted the new output:

I logged off:

`exit`

### optional - test singularity

I tested singularity on compute nodes as follows.


interactive session:

I started an interactive session ...

`salloc -p 12c128g`

output:

... then I ran a download/run container command:

`singularity run library://godlovedc/funny/lolcow`

output:

    INFO:    Downloading library image
    89.2MiB / 89.2MiB [=========================================================================================================] 100 % 18.9 MiB/s 0s
     _______________________________________
    / A Tale of Two Cities LITE(tm)         \
    |                                       |
    | -- by Charles Dickens                 |
    |                                       |
    | A man in love with a girl who loves   |
    | another man who looks just            |
    |                                       |
    | like him has his head chopped off in  |
    | France because of a mean              |
    |                                       |
    | lady who knits.                       |
    |                                       |
    | Crime and Punishment LITE(tm)         |
    |                                       |
    | -- by Fyodor Dostoevski               |
    |                                       |
    | A man sends a nasty letter to a       |
    | pawnbroker, but later                 |
    |                                       |
    | feels guilty and apologizes.          |
    |                                       |
    | The Odyssey LITE(tm)                  |
    |                                       |
    | -- by Homer                           |
    |                                       |
    | After working late, a valiant warrior |
    \ gets lost on his way home.            /
     ---------------------------------------
            \   ^__^
             \  (oo)_______
                (__)\       )\/\
                    ||----w |
                    ||     ||

I closed the interactive-login session ...

`exit`

... and cleared the cache:

`singularity cache clean`
`< output snipped >`


sbatch job:

I added the lolcow command to a compute-node test script:

    echo '# singularity test' >> sleep10.sh
    echo 'singularity run library://godlovedc/funny/lolcow' >> sleep10.sh

    cat sleep10.sh

'cat' output:

    #!/bin/bash
    # sleep for 10 seconds
    sleep 10
    # report hostname etc ...
    hostname
    uptime
    free -h
    # singularity test
    singularity run library://godlovedc/funny/lolcow

I submitted a batch job:

`sbatch sleep10.sh`

output:

`Submitted batch job 5`

I checked job status

`squeue -j 5`

output:

The job took about thirty seconds to complete. I viewed its output file:

`cat slurm-5.out`

output:

I cleared the cache:

`singularity cache clean`
`< output snipped >`

[Category:SPH-LOGIN-MISC](Category:SPH-LOGIN-MISC "wikilink")