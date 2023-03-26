# Amazon EBS CSI driver

```shell
aws iam create-policy --policy-name AmazonEKS_EBS_CSI_Driver_Policy --region us-east-1 --profile devwithrico \
	--policy-document file://example-iam-policy.json
```

```shell
aws iam create-role --role-name AmazonEKS_EBS_CSI_DriverRole --assume-role-policy-document file://"trust-policy.json" \
	--region us-east-1 --profile devwithrico
```

```shell
aws iam attach-role-policy --policy-arn arn:aws:iam::092369361076:policy/AmazonEKS_EBS_CSI_Driver_Policy \
	--role-name AmazonEKS_EBS_CSI_DriverRole --region us-east-1 --profile devwithrico
```

```shell
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"
```

```shell
kubectl delete pods \
	-n kube-system \
	-l=app=ebs-csi-controller
```

#### Test

```shell
git clone https://github.com/kubernetes-sigs/aws-ebs-csi-driver.git
```

```shell
cd aws-ebs-csi-driver/examples/kubernetes/dynamic-provisioning/
```

```shell
kubectl apply -f manifests/
```

#### References

- https://docs.aws.amazon.com/eks/latest/userguide/csi-iam-role.html
- https://repost.aws/knowledge-center/eks-persistent-storage
- https://repost.aws/knowledge-center/eks-troubleshoot-ebs-volume-mounts
- https://github.com/kubernetes-sigs/aws-ebs-csi-driver/blob/master/docs/example-iam-policy.json