# aws-cli-commands
Documentation for good AWS CLI commands


Query the status of all Elasticache clusters
```
aws elasticache describe-cache-clusters --query 'CacheClusters[*].{ID:CacheClusterId,Status:CacheClusterStatus,NodeType:CacheNodeType}' --output table

-------------------------------------------------------
|                DescribeCacheClusters                |
+---------------------+------------------+------------+
|         ID          |    NodeType      |  Status    |
+---------------------+------------------+------------+
|  multi-docker-redis |  cache.t2.micro  |  available |
|  test01             |  cache.t2.micro  |  available |
+---------------------+------------------+------------+
```
