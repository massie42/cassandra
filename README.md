# Cassandra
Useful information about changing adding nodes to and removing nodes from a Cassandra cluster.

# Background information
For a project we needed to have more and bigger virtual machines in Micorsoft Azure to support the increase in load on the system. 
We wanted to move from a three node cluster to a five node cluster without downtime and without loosing data. 

To do this we decided to start adding nodes one by one and make it part of the cluster. So we went from a three node cluster to an eight node cluster. 
Once the eight node cluster was Up and Normal (UN) according to nodetool status, and we did not see any issues we started to decommission the original three nodes. 

# Seeds
In cassandra.yaml file there is a possibility to define the seed nodes of the cluster. 
Search for 'seed_provider' and check the list of seeds. This can be one or a comma-delimited list, of seed nodes.  

## But what are seed nodes? 
The addresses of hosts designated as contact points in the cluster. A joining node contacts one of the nodes in the -seeds list to learn the topology of the ring.
Basically seeds are used during startup to discover the cluster. The seed node designation has no purpose other than bootstrapping the gossip process for new nodes joining the cluster. Seed nodes are not a single point of failure, nor do they have any other special purpose in cluster operations beyond the bootstrapping of nodes.

# Adding nodes
The new nodes need to know the original seed nodes so the same seeds need to be used as from the original three nodes. That way the new nodes learn the topology of the ring and data will be replicated over the cluster. 

## How did we add the new nodes? 
First of all we installed the same version of the Cassandra software on the VM's with the same configuration settings as the original three nodes, i.e. seeds, rack, dc.
When installing the Cassandra was also started and joined the cluster automatically. We were able to see that by running nodetool status. It showed the new nodes as Joining. 

# Removing nodes 
There are a few ways to remove nodes, but one nice way to do this is via the decommission command. 

# Useful commands 

    nodetool status 
    nodetool refresh
    nodetool repair -full
    nodetool decommission


# Useful links 

https://docs.datastax.com/en/dse/5.1/dse-dev/datastax_enterprise/config/configCassandra_yaml.html
