# Profile

##### Awesome AWS Commandline with auto-completion of commands

```
docker run -it -v ~/.aws/:/root/.aws:ro saws
```

##### List Profile Currently In-Use

```
aws configure list

aws configure --profile vpcxTest01

```

##### List All User Profiles

```
cat ~/.aws/config
```

##### Using a different profile (once)

```
aws ec2 describe-instances --profile vpcxTest01
```

##### Change your default profile

```
export AWS_DEFAULT_PROFILE=vpcxTest01
```

# EC2

##### List EC2 Instance ID (simple)

```
aws ec2 describe-instances --query 'Reservations[*].Instances[0].{InstID:InstanceId}' --output table
```

##### EC2: List entire object for single instance

```
aws ec2 describe-instances --query 'Reservations[0].Instances[0]' --output json
```

##### List EC2 Instances (with Name Tag and Launch Time)

```
aws ec2 describe-instances --query 'Reservations[*].Instances[0].{VPCID:VpcId,InstnceID:InstanceId,Type:InstanceType,State:State.Name,LaunchTime:LaunchTime,Name:Tags[?Key==`Name`].Value | [0]}' --output table
```

##### List EC2 Instances (Same, but only running instances)

```
aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query 'Reservations[*].Instances[0].{VPCID:VpcId,InstnceID:InstanceId,Type:InstanceType,State:State.Name,LaunchTime:LaunchTime,Name:Tags[?Key==`Name`].Value | [0]}' --output table
```

##### Create EC2 Instance

```
List AMI's
aws ec2 describe-images --executable-users self --query 'Images[*].{Descr:Description,ImageID:ImageId,Platform:Platform,State:State}' --output table

List Security Groups
aws ec2 describe-security-groups --query 'SecurityGroups[*].{Description:Description,GroupName:GroupName,VpcId:VpcId,GroupId:GroupId}' --output table

List IAM Profiles
aws iam list-instance-profiles --query 'InstanceProfiles[*].{InstanceProfileId:InstanceProfileId,InstanceProfileName:InstanceProfileName,Arn:Arn}' --output table

Create New EC2 Instance
Works:
aws ec2 run-instances --image-id ami-090b610d06e31841e --count 1 --instance-type t2.small --security-group-ids sg-a9b221cc --iam-instance-profile Name=itx-afj-app-irisdashboard-development-5JGYU651CUIC --subnet-id subnet-d1747d97
New:
aws ec2 run-instances --image-id ami-090b610d06e31841e --count 1 --instance-type t2.small --security-group-ids sg-a9b221cc --iam-instance-profile Name=itx-afj-app-irisdashboard-development-5JGYU651CUIC --subnet-id subnet-d1747d97 --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=TestServer01},{Key=application,Value=APP000010003030},{Key=Environment,Value=Development}]'

```

# Elasticache (Redis)

##### Create a new Redis Cluster with 1 very small node

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

##### Query the status of all Elasticache clusters

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
