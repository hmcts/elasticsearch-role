Role Name
=========

Installs and configures Elasticsearch 5.x and associated user security roles.  Current assumption is that all nodes are both master and data nodes.

When Azure subscription setup is complete the Azure discovery plugin should be used: https://www.elastic.co/guide/en/elasticsearch/plugins/5.x/discovery-azure-classic-usage.html

Requirements
------------

None.

Role Variables
--------------

* `elasticsearch_sysctl_vm_max_map_count` - vm.max_map_count sysctl variable
* `elasticsearch_sysctl_vm_swappiness` - vm.swappiness sysctl variable
* `elasticsearch_java_opts` - List of command line arguments to pass to Java, typically used to set heapsize
* `elasticsearch_node_name` - Descriptive name for Elasticsearch node
* `elasticsearch_cluster_name` - Descriptive name of Elasticsearch cluster
* `elasticsearch_discovery_zen_ping_unicast_hosts` - List of master nodes for cluster discovery
* `elasticsearch_discovery_zen_minimum_master_nodes` - Minimum number of master nodes to complete election
* `elasticsearch_gateway_recover_after_nodes` - Start recovery when this many data or master nodes have joined the cluster

** Refer to [https://www.elastic.co/guide/en/elasticsearch/reference/master/heap-size.html] for setting the Java options.

Dependencies
------------

OpenJDK is installed via the reform.java role.

Example Playbook
----------------

Example three node cluster with 8GiB RAM per node

    - hosts: servers
      vars:
        elasticsearch_java_opts:
          - Xms4g
          - Xmx4g
        elasticsearch_discovery_zen_minimum_master_nodes: 3
        elasticsearch_gateway_recover_after_nodes: 3
        elasticsearch_discovery_zen_ping_unicast_hosts:
          - es1
          - es2
          - es3
      roles:
         - role: reform.elasticsearch

molecule test

Security
--------

The index level security is handled via roles.yml and the associated files.  File level security configuration overrides the API level security.  Regular users are defined by the mapping are only allowed read-only access to the specified indices.
