# EKS

```shell
eksctl create cluster \
	--profile devwithrico \
	--name test-eks \
	--region us-east-1 \
	--version 1.25 \
	--vpc-public-subnets subnet-03d19c83b26ead933,subnet-05e0d706717e3f048 \
	--nodegroup-name test-eks-node-group \
	--node-type t2.large \
	--nodes 1 \
	--nodes-min 1 \
	--nodes-max 3 \
	--node-volume-type gp2 \
	--node-volume-size 30 \
	--ssh-access \
	--ssh-public-key devwithrico \
	--asg-access
```