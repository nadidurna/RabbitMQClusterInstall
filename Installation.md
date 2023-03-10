<H1>Step 1: Install Erlang/OTP</H1>

<h2>Step 1: Import Erlang GPG Key</h2>
Run the following commands to import Erlang repository GPG key:

     $> sudo apt update
     $> sudo apt install curl software-properties-common apt-transport-https lsb-release
     $> curl -fsSL https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/erlang.gpg

<h2>Step 2: Add Erlang Repository to Ubuntu 20.04</h2>
Once you have imported the key, add the repository to your Ubuntu 20.04 system by running the following commands:

    $> echo "deb https://packages.erlang-solutions.com/ubuntu $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/erlang.list

<h2>Step 3: Install Erlang on Ubuntu 22.04|20.04|18.04</h2>
The last step is the actual installation of Erlang. Update your system package list and install Erlang:

    sudo apt update
    sudo apt install erlang
---
To start  Erlang shell, run the command:

    $ erl
--

    Erlang/OTP 24 [erts-12.1.4] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [jit]
    Eshell V12.1.4  (abort with ^G)
    1> ^G
    --> q

<H1>Step 2: Install RabbitMQ</H1>

<h2>Step 1: Add RabbitMQ Repository to Ubuntu</h2>

Let’s add RabbitMQ Repository to our Ubuntu system.

    $> curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.deb.sh | sudo bash

<h2>Step 2: Install RabbitMQ Server Ubuntu 20.04</h2>

To install RabbitMQ Server Ubuntu 20.04, update apt list first:

    $> sudo apt update

Then install rabbitmq-server package:
    
    $> sudo apt install rabbitmq-server

Hit the **y** key to start the installation

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
    socat
    The following NEW packages will be installed:
    rabbitmq-server soca
    0 upgraded, 2 newly installed, 0 to remove and 27 not upgraded.
    Need to get 12.3 MB of archives.
    After this operation, 15.3 MB of additional disk space will be used.
    Do you want to continue? [Y/n] y

After installation, RabbitMQ service is started and enabled to start on boot. To check the status, run:

    $ systemctl status rabbitmq-server.service
    ● rabbitmq-server.service - RabbitMQ broker
        Loaded: loaded (/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2022-05-12 23:57:22 EAT; 40s ago
    Main PID: 5631 (beam.smp)
        Tasks: 28 (limit: 9460)
        Memory: 95.0M
            CPU: 3.177s
        CGroup: /system.slice/rabbitmq-server.service
                ├─5631 /usr/lib/erlang/erts-12.2.1/bin/beam.smp -W w -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -sbwt none -sbwtdcpu>
                ├─5642 erl_child_setup 32768
                ├─5672 /usr/lib/erlang/erts-12.2.1/bin/epmd -daemon
                ├─5699 inet_gethost 4
                └─5700 inet_gethost 4

    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Doc guides:  https://rabbitmq.com/documentation.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Support:     https://rabbitmq.com/contact.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Tutorials:   https://rabbitmq.com/getstarted.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Monitoring:  https://rabbitmq.com/monitoring.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Logs: /var/log/rabbitmq/rabbit@ubuntu22.log
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:         /var/log/rabbitmq/rabbit@ubuntu22_upgrade.log
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:         <stdout>
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Config file(s): (none)
    Mei 12 23:57:22 ubuntu22 rabbitmq-server[5631]:   Starting broker... completed with 0 plugins.
    Mei 12 23:57:22 ubuntu22 systemd[1]: Started RabbitMQ broker.

You can confirm if the service is configured to start on boot using the command:

    $> systemctl is-enabled rabbitmq-server.service 
    enabled

<h2>Step 3: Enable the RabbitMQ Management Dashboard</h2>

You can optionally enable the RabbitMQ Management Web dashboard for easy management.

    $> sudo rabbitmq-plugins enable rabbitmq_management
    Enabling plugins on node rabbit@ubuntu:
    rabbitmq_management
    The following plugins have been configured:
    rabbitmq_management
    rabbitmq_management_agent
    rabbitmq_web_dispatch
    Applying plugin configuration to rabbit@ubuntu...
    The following plugins have been enabled:
    rabbitmq_management
    rabbitmq_management_agent
    rabbitmq_web_dispatch

    started 3 plugins.

If you have an active UFW firewall, open both ports 5672 and 15672:

    $> sudo ufw allow 5672
    $> sudo ufw allow 15672

Access it by opening the URL http://[server IP|Hostname]:15672

**IMAGE**

By default, the guest user exists and can connect only from localhost. You can login with this user locally with the password “guest”

To be able to login on the network, create an admin user like below:

    $> sudo rabbitmqctl add_user admin StrongPassword
    $> sudo rabbitmqctl set_user_tags admin administrator

Login with this admin username and the password assigned.

<h1>Step 3: Set RabbitMQ Cluster</h1>

**Setup Requirements**
<p>Installed Ubuntu 20.04 LTS servers</p>
<p>At least two RabbitMQ servers</p>
<p>A user with sudo privileges</p>
<p>The servers should have internet access</p>


This setup of RabbitMQ Cluster on Ubuntu 20.04 is based on three servers with the following IP addresses and hostnames.

| Server       | Hostname    | IP Address     |
| ------------ | ----------- | -------------- |
| rabbitmq-1   | mq1.com     | 4.231.106.167  |
| rabbitmq-2   | mq2.com     | 74.234.187.167 |
| rabbitmq-3   | mq2.com     | 74.234.240.177 |


<h2>Step 1: Setup Hostnames and DNS</h2>

The first step in the installation of the RabbitMQ cluster on Ubuntu 20.04 is to configure correct hostnames and DNS, you can add the records to the **/etc/hosts** file.

Add server, hostname and ip information on all servers :

    sudo echo "4.231.106.167 mq1.com rabbitmq-1" >> /etc/hosts
    sudo echo "74.234.187.167 mq2.com rabbitmq-2" >> /etc/hosts
    sudo echo "74.234.240.177 mq3.com rabbitmq-3" >> /etc/hosts

Do this steps in all your servers.

Then update your systems:

    sudo apt update
    sudo apt -y upgrade

<h2>Step 2: Install Erlang and RabbitMQ</h2>

Repeat Step 1 and Step 2 for all of your RabbitMQ Servers

<h2>Step 3: Copy RabbitMQ Server 1 Cookie RabbitMQ to Server 2 and Server 3</h2>

For RabbitMQ cluster to work, all the nodes participating in the cluster should have the same Cookie. Copy Cookie on your first node to all other nodes in the cluster.

On mq1 run:

    $> sudo scp /var/lib/rabbitmq/.erlang.cookie rabbitmq-2:/var/lib/rabbitmq/.erlang.cookie
    $> sudo scp /var/lib/rabbitmq/.erlang.cookie rabbitmq-3:/var/lib/rabbitmq/.erlang.cookie

<h2> Step 4: Reset RabbitMQ on Node2 </h2>

Reconfigure RabbitMQ on Node 2 and join it to the cluster.

Restart RabbitMQ service

    sudo systemctl restart rabbitmq-server

Stop application

    $ sudo rabbitmqctl stop_app
    Stopping rabbit application on node rabbit@mq2 ...

Reset rabbitmq

    $ sudo rabbitmqctl reset
    Resetting node rabbit@mq2 ...

Join the node to cluster

    $ sudo rabbitmqctl join_cluster rabbit@rabbitmq-1
    Clustering node rabbit@mq2 with rabbit@rabbitmq-1

Start the application process

    $ sudo rabbitmqctl start_app
    Starting node rabbit@mq2 ...  completed with 0 plugins.

Check Cluster Status:

    root@rabbitmq-1:~# rabbitmqctl cluster_status

   

