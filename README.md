
### Awesome AWS Commandline with auto-completion of commands

```
docker run -it -v ~/.aws/:/root/.aws:ro saws
```

### Create a new Redis Cluster with 1 very small node
```
$ aws elasticache create-cache-cluster --engine redis --cache-cluster-id test03 --cache-node-type "cache.t2.micro" --num-cache-nodes 1

{
    "CacheCluster": {
        "Engine": "redis",
        "CacheParameterGroup": {
            "CacheNodeIdsToReboot": [],
            "CacheParameterGroupName": "default.redis5.0",
            "ParameterApplyStatus": "in-sync"
        },
        "CacheClusterId": "test03",
        "CacheSecurityGroups": [],
        "NumCacheNodes": 1,
        "AtRestEncryptionEnabled": false,
        "SnapshotWindow": "07:00-08:00",
        "AutoMinorVersionUpgrade": true,
        "CacheClusterStatus": "creating",
        "SnapshotRetentionLimit": 0,
        "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:",
        "TransitEncryptionEnabled": false,
        "CacheSubnetGroupName": "default",
        "EngineVersion": "5.0.6",
        "PendingModifiedValues": {},
        "PreferredMaintenanceWindow": "thu:10:00-thu:11:00",
        "CacheNodeType": "cache.t2.micro"
    }
}
```


### Query the status of all Elasticache clusters
```
$ aws elasticache describe-cache-clusters --query 'CacheClusters[*].{ID:CacheClusterId,Status:CacheClusterStatus,NodeType:CacheNodeType}' --output table

-------------------------------------------------------
|                DescribeCacheClusters                |
+---------------------+------------------+------------+
|         ID          |    NodeType      |  Status    |
+---------------------+------------------+------------+
|  multi-docker-redis |  cache.t2.micro  |  available |
|  test01             |  cache.t2.micro  |  available |
|  test03             |  cache.t2.micro  |  creating  |
+---------------------+------------------+------------+
```

