# Autoscaling

#### This setting still under development.

```shell
aws iam create-policy \
  --policy-name AmazonEKSClusterAutoscalerPolicy \
  --policy-document file://cluster-autoscaler-policy.json \
  --profile devwithrico --region us-east-1
```

This step needs to be checked. For now, use the [AWS Management Console](https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html).

```shell
aws iam create-role --role-name AmazonEKSClusterAutoscalerRole --assume-role-policy-document file://cluster-autoscaler-policy.json \
  --region us-east-1 --profile devwithrico


aws iam attach-role-policy --policy-arn arn:aws:iam::092369361076:policy/AmazonEKSClusterAutoscalerPolicy \
  --role-name AmazonEKSClusterAutoscalerRole --region us-east-1 --profile devwithrico


# Or use the command below


eksctl create iamserviceaccount \
  --cluster test-eks \
  --namespace kube-system \
  --name cluster-autoscaler \
  --attach-policy-arn arn:aws:iam::092369361076:policy/AmazonEKSClusterAutoscalerPolicy \
  --override-existing-serviceaccounts \
  --approve \
  --profile devwithrico \
  --region us-east-1
```

```shell
kubectl apply -f cluster-autoscaler-autodiscover.yaml
```

```shell
kubectl annotate serviceaccount cluster-autoscaler \
  -n kube-system \
  eks.amazonaws.com/role-arn=arn:aws:iam::092369361076:role/AmazonEKSClusterAutoscalerRole
```

```shell
kubectl patch deployment cluster-autoscaler \
  -n kube-system \
  -p '{"spec":{"template":{"metadata":{"annotations":{"cluster-autoscaler.kubernetes.io/safe-to-evict": "false"}}}}}'
```

```shell
            - '--expander=least-waste'
            - '--node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/test-eks'
            - '--balance-similar-node-groups'
            - '--skip-nodes-with-system-pods=false'
```

```shell
kubectl set image deployment cluster-autoscaler \
  -n kube-system \
  cluster-autoscaler=registry.k8s.io/autoscaling/cluster-autoscaler:v1.25.0
```

#### References

- https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html