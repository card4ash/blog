Title: Creating MQTT Cluster 
Published: 08/15/2020
Tags:
    - Introduction
    - MQTT 
    - Cluster
    - Installation
---

# Emqtt Cluster Broker

Creating MQTT Cluster Broker
===============================

********************************
Emqtt Cluster Broker
********************************

 **Install  broker**

 #. Cd into emqttd (all command run from C:\emqttd\)
 #. Edit emqttd\etc\emq.conf file (possible line 21) change node.name = emqttd@192.168.11.9
 #. Repeat it for every node like node.name = emqttd@192.168.11.10 node.name =emqttd@192.168.11.11

 For every node

 #. Start the broker in console mode:``bin\emqttd console``
 #. Install emqttd serivce:``bin\emqttd install``
 #. Start emqttd serivce:``bin\emqttd start``

Start the three broker nodes, and ‘cluster join ‘ on node@213.136.77.177 and node@213.136.80.212 respectively as follows

 .. code-block:: shell
       
       ./bin/emqttd_ctl cluster join emqttd@213.136.80.212

**The Firewall**

Open 4369 port  used by epmd daemon, and a port segment for nodes’ communication.
here we open 20000-21000 for node communication

.. code-block:: shell

      [{kernel, [
            ...
            {inet_dist_listen_min, 20000},
            {inet_dist_listen_max, 21000}
      ]},

*****************************************************
Verneqm Cluster Broker (Alternative to emqtt)
*****************************************************

Installation
******************

Since emqtt cluster does not support shared subscription accross cluster. We tried to 
solve the problem using vernemq cluster. Vernemq cluster support shared subscription.

**Installing VerneMQ**

Download the binary package from https://vernemq.com/downloads/ 
we choose Xenial 1.3.0.
Once downloaded the binary package, execute the following command to install VerneMQ:


.. code-block :: shell

    sudo dpkg -i vernemq_1.3.0-1_amd64.deb

**Verify installation**

.. code-block :: shell

    dpkg -s vernemq | grep Status


vernemq nodes config for creating cluster

.. code-block :: shell
    
    sudo nano /etc/vernemq/vernemq.conf

.. code-block :: shell

        allow_anonymous = on
        allow_register_during_netsplit = on
        allow_publish_during_netsplit = on
        allow_subscribe_during_netsplit = on
        allow_unsubscribe_during_netsplit = on
        allow_multiple_sessions = off
        retry_interval = 10
        max_client_id_size = 512
        max_inflight_messages = 1000
        max_online_messages = -1
        max_offline_messages = -1
        max_message_size = 0
        upgrade_outgoing_qos = off
        listener.max_connections = 10000
        listener.nr_of_acceptors = 10
        listener.tcp.default = 192.168.100.123:1883
        listener.vmq.clustering = 192.168.100.123:44053
        listener.http.default = 127.0.0.1:8888
        listener.mountpoint = off
        systree_enabled = on
        systree_interval = 20000
        graphite_enabled = off
        graphite_host = localhost
        graphite_port = 2003
        graphite_interval = 20000
        shared_subscription_policy = random
        plugins.vmq_passwd = on
        plugins.vmq_acl = on
        plugins.vmq_diversity = off
        plugins.vmq_webhooks = off
        plugins.vmq_bridge = off
        vmq_acl.acl_file = /etc/vernemq/vmq.acl
        vmq_acl.acl_reload_interval = 10
        vmq_passwd.password_file = /etc/vernemq/vmq.passwd
        vmq_passwd.password_reload_interval = 10
        vmq_diversity.script_dir = /usr/share/vernemq/lua
        vmq_diversity.auth_postgres.enabled = off
        vmq_diversity.auth_mysql.enabled = off
        vmq_diversity.auth_mongodb.enabled = off
        vmq_diversity.auth_redis.enabled = off
        log.console = file
        log.console.level = info
        log.console.file = /var/log/vernemq/console.log
        log.error.file = /var/log/vernemq/error.log
        log.syslog = off
        log.crash = on
        log.crash.file = /var/log/vernemq/crash.log
        log.crash.maximum_message_size = 64KB
        log.crash.size = 10MB
        log.crash.rotation = $D0
        log.crash.rotation.keep = 5
        nodename = VerneMQ@192.168.100.123
        distributed_cookie = vmqaplomb
        erlang.async_threads = 64
        erlang.max_ports = 262144
        leveldb.maximum_memory.percent = 70

.. code-block :: shell

        allow_anonymous = on
        allow_register_during_netsplit = on
        allow_publish_during_netsplit = on
        allow_subscribe_during_netsplit = on
        allow_unsubscribe_during_netsplit = on
        allow_multiple_sessions = off
        retry_interval = 10
        max_client_id_size = 512
        max_inflight_messages = 1000
        max_online_messages = -1
        max_offline_messages = -1
        max_message_size = 0
        upgrade_outgoing_qos = off
        listener.max_connections = 10000
        listener.nr_of_acceptors = 10
        listener.tcp.default = 192.168.100.124:1883
        listener.vmq.clustering = 192.168.100.124:44053
        listener.http.default = 127.0.0.1:8888
        listener.mountpoint = off
        systree_enabled = on
        systree_interval = 20000
        graphite_enabled = off
        graphite_host = localhost
        graphite_port = 2003
        graphite_interval = 20000
        shared_subscription_policy = random
        plugins.vmq_passwd = on
        plugins.vmq_acl = on
        plugins.vmq_diversity = off
        plugins.vmq_webhooks = off
        plugins.vmq_bridge = off
        vmq_acl.acl_file = /etc/vernemq/vmq.acl
        vmq_acl.acl_reload_interval = 10
        vmq_passwd.password_file = /etc/vernemq/vmq.passwd
        vmq_passwd.password_reload_interval = 10
        vmq_diversity.script_dir = /usr/share/vernemq/lua
        vmq_diversity.auth_postgres.enabled = off
        vmq_diversity.auth_mysql.enabled = off
        vmq_diversity.auth_mongodb.enabled = off
        vmq_diversity.auth_redis.enabled = off
        log.console = file
        log.console.level = info
        log.console.file = /var/log/vernemq/console.log
        log.error.file = /var/log/vernemq/error.log
        log.syslog = off
        log.crash = on
        log.crash.file = /var/log/vernemq/crash.log
        log.crash.maximum_message_size = 64KB
        log.crash.size = 10MB
        log.crash.rotation = $D0
        log.crash.rotation.keep = 5
        nodename = VerneMQ@192.168.100.124
        distributed_cookie = vmqaplomb
        erlang.async_threads = 64
        erlang.max_ports = 262144
        leveldb.maximum_memory.percent = 70


**Activate VerneMQ node**

.. code-block :: shell

    sudo service vernemq start

Once our broker has started, you can initially check that it is running with the vernemq ping command:

.. code-block :: shell

    sudo vernemq ping

**Joining a Cluster**

.. code-block :: shell

    sudo vmq-admin cluster join discovery-node=VerneMQ@127.0.0.1

**Leaving a Cluster**

.. code-block :: shell

    vmq-admin cluster leave node=<NodeThatShouldGo> (only the first step!)

**Getting Cluster Status Information**

.. code-block :: shell

    sudo vmq-admin cluster show

**Notes**

Note: For a successful VerneMQ cluster setup, it is important to choose proper VerneMQ node names. In vernemq.conf change the nodename = VerneMQ@127.0.0.1 to something appropriate. We choose VerneMQ@192.168.100.124 and VerneMQ@192.168.100.123  Make sure that the node names are unique within the cluster. And choose distributed_cookie = vmqaplomb.

**Other Configure Related Info**

``allow_multiple_sessions = off``

Allows a client to logon multiple times using the same client id. If on shared subscribtion may not work properly.

``max_inflight_messages = 1000``

This option defines the maximum number of QoS 1 or 2 messages that can be in the process of being transmitted simultaneously.

``max_online_messages = -1``

The maximum number of messages to hold in the queue above those messages that are currently in flight. Defaults to 1000. Set to -1 for no limit. This option protects a client session from overload by dropping messages (of any QoS).Defaults to 1000 messages, use -1 for no limit. This parameter was named max_queued_messages in 0.10.*. Note that 0 will totally block message delivery from any queue!

``max_offline_messages = -1``

This option specifies the maximum number of QoS 1 and 2 messages to hold in the offline queue.Defaults to 1000 messages, use -1 for no limit, use 0 if no messages should be stored.

In contrast to the session based inflight window, max_online_messages and max_offline_messages serves as a protection of queues, on the outgoing side.

Need to study more about the behabior about message incoming rate and consumption rate behabior configuration. and control process.

Troubleshooting
******************

**In case cluster creation failed**

wiping all cluster metadata  by shutting down all the nodes and then remove the metadata (probably in /var/lib/vernemq/meta/). This will mean each node no longer has any knowledge of the cluster. Then restart each node and re-create the cluster using the vmq-admin cluster join command.

**Observing Log File**

.. code-block :: shell

    sudo nano /var/log/vernemq/console.log
    sudo nano /var/log/vernemq/error.log
    sudo nano /var/log/vernemq/crash.log


Varnemq Monitoring
***********************

**Observing Session**

``sudo vmq-admin session show``

**Clear session**

Example:
``sudo vmq-admin session disconnect client-id=testf9695b6e-555b-4b3d-8da6-7164d5274103 -c``

**Observing the metrics**

``sudo vmq-admin metrics show``

For qos2 message sum of  ``counter.mqtt_pubcomp_sent = 20000`` across the cluster should be equal to no of message published to the broker.

.. image :: /Images/qos2.png

Adn for qos2 subscription in our case the sum of ``counter.mqtt_pubcomp_received = 10023`` and ``counter.mqtt_pubcomp_received = 9977`` should be equal to all message published to broker in the specific topic which our mqtt services client shared subscribed.




**Reference Links:**

#. http://emqtt.io/docs/v2/install.html#installing-on-windows
#. http://emqttd-docs.readthedocs.io/en/latest/cluster.html

#. https://vernemq.com/docs/installation/
#. https://vernemq.com/docs/clustering/