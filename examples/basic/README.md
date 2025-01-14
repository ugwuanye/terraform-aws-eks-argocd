# Basic example

The code in this example shows how to use the module with basic configuration and minimal set of other resources.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 4.19.0 |
| <a name="requirement_helm"></a> [helm](#requirement\_helm) | >= 2.6.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.11.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_argocd_disabled"></a> [argocd\_disabled](#module\_argocd\_disabled) | ../../ | n/a |
| <a name="module_argocd_helm"></a> [argocd\_helm](#module\_argocd\_helm) | ../../ | n/a |
| <a name="module_argocd_self_managed_helm"></a> [argocd\_self\_managed\_helm](#module\_argocd\_self\_managed\_helm) | ../../ | n/a |
| <a name="module_argocd_self_managed_kubernetes"></a> [argocd\_self\_managed\_kubernetes](#module\_argocd\_self\_managed\_kubernetes) | ../../ | n/a |
| <a name="module_eks_cluster"></a> [eks\_cluster](#module\_eks\_cluster) | cloudposse/eks-cluster/aws | 2.3.0 |
| <a name="module_eks_node_group"></a> [eks\_node\_group](#module\_eks\_node\_group) | cloudposse/eks-node-group/aws | 2.4.0 |
| <a name="module_vpc"></a> [vpc](#module\_vpc) | terraform-aws-modules/vpc/aws | 3.14.2 |

## Resources

| Name | Type |
|------|------|
| [aws_eks_cluster.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_cluster) | data source |
| [aws_eks_cluster_auth.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/eks_cluster_auth) | data source |

## Inputs

No inputs.

## Outputs

No outputs.
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
