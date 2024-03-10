### Ingress Controller

Reference
-----------
https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.7/
-----------

1. eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eks-spot-cluster \
    --approve

2. curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.7.0/docs/install/iam_policy.json


3. aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json


4. eksctl create iamserviceaccount \
--cluster=eks-spot-cluster \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::612400101384:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve


5. helm repo add eks https://aws.github.io/eks-charts

6. helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=eks-spot-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller

