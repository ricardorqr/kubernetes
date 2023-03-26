# IAM OIDC provider

Creates IAM OIDC provider.

```shell
eksctl utils associate-iam-oidc-provider --cluster test-eks --approve --profile devwithrico --region us-east-1
```

Checks the OIDC.

```shell
aws iam list-open-id-connect-providers --profile devwithrico --region us-east-1 | grep AA09C6D22CC3426706B34B3DEAE52DAF
```

#### References

- https://repost.aws/knowledge-center/eks-troubleshoot-oidc-and-irsa
- https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html