# Installation and Configuration

## 1. Deploy and start an ElasticSearch cluster that satisfies a given set of requirements

* **Operations**

```
// install es.
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.13.4-x86_64.rpm
yun install elasticsearch-7.13.4-x86_64.rpm

// update  /etc/elasticsearch/elasticsearch.yml
cluster.name: cluster1 
node.roles: [master, data, voting_only] // 3 `master-eligible` nodes, and 1 `voting-only master-eligible` node
network.host: 172.17.0.17 // host ip, if docker network mode is `host`
http.port: 9200
discovery.seed_hosts: ["172.17.0.17:9300","172.17.0.17:9301"]
cluster.initial_master_nodes: ["172.17.0.17:9300","172.17.0.17:9301"]

// start 
systemctl start elasticsearch.service

// install kibana
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.13.4-x86_64.rpm
yun install kibana-7.13.4-x86_64.rpm

// update  /etc/kibana/kibana.yml
server.host: "10.143.250.194"
elasticsearch.hosts: ["http://10.143.254.53:9200", "http://10.143.250.194:9200"]

// start
systemctl start kibana.service
```

* API: https://www.elastic.co/guide/en/elasticsearch/reference/7.13/setup.html


## 2. Configure the nodes of a cluster to satisfy a given set of requirements

Doc https://www.elastic.co/guide/en/elasticsearch/reference/7.13/modules-node.html

* **Roles**

| Role | Purpose |
| :---: | --- |
| master | - eligible to be elected as the `master` node <br/> |
| data | - hold data <br/> - perform data related operations, such as CURD, search and aggregations. <br/> - can fill any of the specialised data roles
| ingest | - support to apply an ingest pipeline to a document into to `transform` or `enrich` the document before indexing. <br/> - Use dedicated ingest node <br/> - Not to have `master` or `data` role
| remote_cluster_client | - eligible to act a remote client
| ml | - Perform `machine learning` features. <br/> - There is must be at least one `ml` node in the cluster, if need `machine learning` features 
| transform | - Perform `transforms`. <br/> - There must be at least one `transform` node in the cluster, if need `transforms`

* **Coordinating node**
  * search request, 2 phase
    * scatter phase
    * gather phase
  * a node with explicit **empty** list of roles via `node.roles`  

* **Master-eligible nodes**
  * Responsible for **lightweight** cluster-wide operations. Such as 
    * creating or deleting index
    * tracking which nodes are part of the cluster.
    * deciding which shards to allocate to which node.
