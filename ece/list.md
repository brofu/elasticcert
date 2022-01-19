# ECE Knowledges


## Lists

1. Install and Configuration
2. Indexing
3. Queries
4. Aggregations
5. Mappings and Text Analysis
6. Cluster Administration/Management

## 1. Install and Configuration
####  1.1 Deploy and start an ElasticSearch cluster that satisfies a given set of requirements

* Operations

```
// install es.
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.13.4-x86_64.rpm
yun install elasticsearch-7.13.4-x86_64.rpm

// update  /etc/elasticsearch/elasticsearch.yml
cluster.name: cluster1 
node.name: node1
node.master: true 
node.data: true 
node.ingest: true
node.ml: false 
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
