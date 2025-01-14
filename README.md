# ArgoCD Terraform module

[<img src="https://lablabs.io/static/ll-logo.png" width=350px>](https://lablabs.io/)

We help companies build, run, deploy and scale software and infrastructure by embracing the right technologies and principles. Check out our website at <https://lablabs.io/>

---

[![Terraform validate](https://github.com/lablabs/terraform-aws-eks-argocd/actions/workflows/validate.yaml/badge.svg)](https://github.com/lablabs/terraform-aws-eks-argocd/actions/workflows/validate.yaml)
[![pre-commit](https://github.com/lablabs/terraform-aws-argocd/actions/workflows/pre-commit.yml/badge.svg)](https://github.com/lablabs/terraform-aws-eks-argocd/actions/workflows/pre-commit.yml)

## Description

A Terraform module to deploy the ArgoCD on Amazon EKS cluster.

## Related Projects

Check out other [terraform kubernetes addons](https://github.com/orgs/lablabs/repositories?q=terraform-aws-eks&type=public&language=&sort=).

## Deployment methods

This module deploys ArgoCD in two different ways:
1. A Helm release that is further managed by Helm
2. A Helm release along with ArgoCD Application CRD which allows Argo to self-manage itself.

### 1. Helm
Deploy Helm chart via Helm resource (default method, set `enabled = true`)

### 2.1. Argo Kubernetes
Deploy Helm chart as ArgoCD Application via Kubernetes manifest resource (set `enabled = true` and `argo_enabled = true`)

> **Warning**
>
> When deploying with ArgoCD application, Kubernetes terraform provider requires access to Kubernetes cluster API during plan time. This introduces potential issue when you want to deploy the cluster with this addon at the same time, during the same Terraform run.
>
> To overcome this issue, the module deploys the ArgoCD application object using the Helm provider, which does not require API access during plan. If you want to deploy the application using this workaround, you can set the `argo_helm_enabled` variable to `true`.

### 2.2. Argo Helm
Deploy Helm chart as ArgoCD Application via Helm resource (set `enabled = true`, `argo_enabled = true` and `argo_helm_enabled = true`)

## ArgoCD self-managed mode

This module provides an option to deploy in self-managed mode. If `self_managed` is set, the module will make an initial deployment of ArgoCD with Helm and then proceed to deploy ArgoCD Application object, so you're able to manage ArgoCD from ArgoCD. The helm release has a lifecycle ignore_changes rules set on its resource, so no further changes are made to the release. It is only used for the initial ArgoCD deployment and only the newly deployed, self-managed object is used.

> **Warning**
>
> Changing the `self_managed` variable after ArgoCD was already deployed will result in its re-creation.

## AWS IAM resources

To disable of creation IRSA role, set `irsa_role_create = false`.

## Examples

See [Basic example](examples/basic/README.md) for further information.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 4.19.0 |
| <a name="requirement_helm"></a> [helm](#requirement\_helm) | >= 2.6.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.16.0 |
| <a name="requirement_utils"></a> [utils](#requirement\_utils) | >= 0.17.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_iam_role.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy_attachment.this_additional](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [helm_release.argo_application](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [helm_release.self_managed](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [helm_release.this](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [kubernetes_manifest.this](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/manifest) | resource |
| [aws_iam_policy_document.this_irsa](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [utils_deep_merge_yaml.argo_helm_values](https://registry.terraform.io/providers/cloudposse/utils/latest/docs/data-sources/deep_merge_yaml) | data source |
| [utils_deep_merge_yaml.values](https://registry.terraform.io/providers/cloudposse/utils/latest/docs/data-sources/deep_merge_yaml) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cluster_identity_oidc_issuer"></a> [cluster\_identity\_oidc\_issuer](#input\_cluster\_identity\_oidc\_issuer) | The OIDC Identity issuer for the cluster | `string` | n/a | yes |
| <a name="input_cluster_identity_oidc_issuer_arn"></a> [cluster\_identity\_oidc\_issuer\_arn](#input\_cluster\_identity\_oidc\_issuer\_arn) | The OIDC Identity issuer ARN for the cluster that can be used to associate IAM roles with a service account | `string` | n/a | yes |
| <a name="input_argo_apiversion"></a> [argo\_apiversion](#input\_argo\_apiversion) | ArgoCD Appliction apiVersion | `string` | `"argoproj.io/v1alpha1"` | no |
| <a name="input_argo_destination_server"></a> [argo\_destination\_server](#input\_argo\_destination\_server) | Destination server for ArgoCD Application | `string` | `"https://kubernetes.default.svc"` | no |
| <a name="input_argo_enabled"></a> [argo\_enabled](#input\_argo\_enabled) | If set to true, the module will be deployed as ArgoCD application, otherwise it will be deployed as a Helm release | `bool` | `false` | no |
| <a name="input_argo_helm_enabled"></a> [argo\_helm\_enabled](#input\_argo\_helm\_enabled) | If set to true, the ArgoCD Application manifest will be deployed using Kubernetes provider as a Helm release. Otherwise it'll be deployed as a Kubernetes manifest. See Readme for more info | `bool` | `false` | no |
| <a name="input_argo_helm_values"></a> [argo\_helm\_values](#input\_argo\_helm\_values) | Value overrides to use when deploying argo application object with helm | `string` | `""` | no |
| <a name="input_argo_info"></a> [argo\_info](#input\_argo\_info) | ArgoCD info manifest parameter | <pre>list(object({<br>    name  = string<br>    value = string<br>  }))</pre> | <pre>[<br>  {<br>    "name": "terraform",<br>    "value": "true"<br>  }<br>]</pre> | no |
| <a name="input_argo_kubernetes_manifest_computed_fields"></a> [argo\_kubernetes\_manifest\_computed\_fields](#input\_argo\_kubernetes\_manifest\_computed\_fields) | List of paths of fields to be handled as "computed". The user-configured value for the field will be overridden by any different value returned by the API after apply. | `list(string)` | <pre>[<br>  "metadata.labels",<br>  "metadata.annotations",<br>  "metadata.finalizers"<br>]</pre> | no |
| <a name="input_argo_kubernetes_manifest_field_manager_force_conflicts"></a> [argo\_kubernetes\_manifest\_field\_manager\_force\_conflicts](#input\_argo\_kubernetes\_manifest\_field\_manager\_force\_conflicts) | Forcibly override any field manager conflicts when applying the kubernetes manifest resource | `bool` | `false` | no |
| <a name="input_argo_kubernetes_manifest_field_manager_name"></a> [argo\_kubernetes\_manifest\_field\_manager\_name](#input\_argo\_kubernetes\_manifest\_field\_manager\_name) | The name of the field manager to use when applying the kubernetes manifest resource. Defaults to Terraform | `string` | `"Terraform"` | no |
| <a name="input_argo_kubernetes_manifest_wait_fields"></a> [argo\_kubernetes\_manifest\_wait\_fields](#input\_argo\_kubernetes\_manifest\_wait\_fields) | A map of fields and a corresponding regular expression with a pattern to wait for. The provider will wait until the field matches the regular expression. Use * for any value. | `map(string)` | `{}` | no |
| <a name="input_argo_metadata"></a> [argo\_metadata](#input\_argo\_metadata) | ArgoCD Application metadata configuration. Override or create additional metadata parameters<pre>{<br>   "annotations" : {}<br>   "labels" : {}<br>   "finalizers" : [<br>     "resources-finalizer.argocd.argoproj.io"<br>   ]<br>}</pre> | `any` | `{}` | no |
| <a name="input_argo_namespace"></a> [argo\_namespace](#input\_argo\_namespace) | Namespace to deploy ArgoCD application CRD to | `string` | `"argo"` | no |
| <a name="input_argo_project"></a> [argo\_project](#input\_argo\_project) | ArgoCD Application project | `string` | `"default"` | no |
| <a name="input_argo_skip_crds"></a> [argo\_skip\_crds](#input\_argo\_skip\_crds) | If set, no CRDs will be installed when deploying argo application | `bool` | `false` | no |
| <a name="input_argo_spec"></a> [argo\_spec](#input\_argo\_spec) | ArgoCD Application spec configuration. Override or create additional spec parameters | `any` | `{}` | no |
| <a name="input_argo_sync_policy"></a> [argo\_sync\_policy](#input\_argo\_sync\_policy) | ArgoCD syncPolicy manifest parameter | `any` | `{}` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Variable indicating whether deployment is enabled | `bool` | `true` | no |
| <a name="input_helm_atomic"></a> [helm\_atomic](#input\_helm\_atomic) | If set, installation process purges chart on fail. The wait flag will be set automatically if atomic is used | `bool` | `false` | no |
| <a name="input_helm_chart_name"></a> [helm\_chart\_name](#input\_helm\_chart\_name) | Helm chart name to be installed | `string` | `"argo-cd"` | no |
| <a name="input_helm_chart_version"></a> [helm\_chart\_version](#input\_helm\_chart\_version) | Version of the Helm chart | `string` | `"4.10.8"` | no |
| <a name="input_helm_cleanup_on_fail"></a> [helm\_cleanup\_on\_fail](#input\_helm\_cleanup\_on\_fail) | Allow deletion of new resources created in this helm upgrade when upgrade fails | `bool` | `false` | no |
| <a name="input_helm_create_namespace"></a> [helm\_create\_namespace](#input\_helm\_create\_namespace) | Create the namespace if it does not yet exist | `bool` | `true` | no |
| <a name="input_helm_dependency_update"></a> [helm\_dependency\_update](#input\_helm\_dependency\_update) | Runs helm dependency update before installing the chart | `bool` | `false` | no |
| <a name="input_helm_description"></a> [helm\_description](#input\_helm\_description) | Set helm release description attribute (visible in the history) | `string` | `""` | no |
| <a name="input_helm_devel"></a> [helm\_devel](#input\_helm\_devel) | Use helm chart development versions, too. Equivalent to version '>0.0.0-0'. If version is set, this is ignored | `bool` | `false` | no |
| <a name="input_helm_disable_openapi_validation"></a> [helm\_disable\_openapi\_validation](#input\_helm\_disable\_openapi\_validation) | If set, the installation process will not validate rendered helm templates against the Kubernetes OpenAPI Schema | `bool` | `false` | no |
| <a name="input_helm_disable_webhooks"></a> [helm\_disable\_webhooks](#input\_helm\_disable\_webhooks) | Prevent helm chart hooks from running | `bool` | `false` | no |
| <a name="input_helm_force_update"></a> [helm\_force\_update](#input\_helm\_force\_update) | Force helm resource update through delete/recreate if needed | `bool` | `false` | no |
| <a name="input_helm_keyring"></a> [helm\_keyring](#input\_helm\_keyring) | Location of public keys used for verification. Used only if helm\_package\_verify is true | `string` | `"~/.gnupg/pubring.gpg"` | no |
| <a name="input_helm_lint"></a> [helm\_lint](#input\_helm\_lint) | Run the helm chart linter during the plan | `bool` | `false` | no |
| <a name="input_helm_package_verify"></a> [helm\_package\_verify](#input\_helm\_package\_verify) | Verify the package before installing it. Helm uses a provenance file to verify the integrity of the chart; this must be hosted alongside the chart | `bool` | `false` | no |
| <a name="input_helm_postrender"></a> [helm\_postrender](#input\_helm\_postrender) | Value block with a path to a binary file to run after helm renders the manifest which can alter the manifest contents | `map(any)` | `{}` | no |
| <a name="input_helm_recreate_pods"></a> [helm\_recreate\_pods](#input\_helm\_recreate\_pods) | Perform pods restart during helm upgrade/rollback | `bool` | `false` | no |
| <a name="input_helm_release_max_history"></a> [helm\_release\_max\_history](#input\_helm\_release\_max\_history) | Maximum number of release versions stored per release | `number` | `0` | no |
| <a name="input_helm_release_name"></a> [helm\_release\_name](#input\_helm\_release\_name) | Helm release name | `string` | `"argocd"` | no |
| <a name="input_helm_render_subchart_notes"></a> [helm\_render\_subchart\_notes](#input\_helm\_render\_subchart\_notes) | If set, render helm subchart notes along with the parent | `bool` | `true` | no |
| <a name="input_helm_replace"></a> [helm\_replace](#input\_helm\_replace) | Re-use the given name of helm release, only if that name is a deleted release which remains in the history. This is unsafe in production | `bool` | `false` | no |
| <a name="input_helm_repo_ca_file"></a> [helm\_repo\_ca\_file](#input\_helm\_repo\_ca\_file) | Helm repositories cert file | `string` | `""` | no |
| <a name="input_helm_repo_cert_file"></a> [helm\_repo\_cert\_file](#input\_helm\_repo\_cert\_file) | Helm repositories cert file | `string` | `""` | no |
| <a name="input_helm_repo_key_file"></a> [helm\_repo\_key\_file](#input\_helm\_repo\_key\_file) | Helm repositories cert key file | `string` | `""` | no |
| <a name="input_helm_repo_password"></a> [helm\_repo\_password](#input\_helm\_repo\_password) | Password for HTTP basic authentication against the helm repository | `string` | `""` | no |
| <a name="input_helm_repo_url"></a> [helm\_repo\_url](#input\_helm\_repo\_url) | Helm repository | `string` | `"https://argoproj.github.io/argo-helm"` | no |
| <a name="input_helm_repo_username"></a> [helm\_repo\_username](#input\_helm\_repo\_username) | Username for HTTP basic authentication against the helm repository | `string` | `""` | no |
| <a name="input_helm_reset_values"></a> [helm\_reset\_values](#input\_helm\_reset\_values) | When upgrading, reset the values to the ones built into the helm chart | `bool` | `false` | no |
| <a name="input_helm_reuse_values"></a> [helm\_reuse\_values](#input\_helm\_reuse\_values) | When upgrading, reuse the last helm release's values and merge in any overrides. If 'helm\_reset\_values' is specified, this is ignored | `bool` | `false` | no |
| <a name="input_helm_set_sensitive"></a> [helm\_set\_sensitive](#input\_helm\_set\_sensitive) | Value block with custom sensitive values to be merged with the values yaml that won't be exposed in the plan's diff | `map(any)` | `{}` | no |
| <a name="input_helm_skip_crds"></a> [helm\_skip\_crds](#input\_helm\_skip\_crds) | If set, no CRDs will be installed before helm release | `bool` | `false` | no |
| <a name="input_helm_timeout"></a> [helm\_timeout](#input\_helm\_timeout) | Time in seconds to wait for any individual kubernetes operation (like Jobs for hooks) | `number` | `300` | no |
| <a name="input_helm_wait"></a> [helm\_wait](#input\_helm\_wait) | Will wait until all helm release resources are in a ready state before marking the release as successful. It will wait for as long as timeout | `bool` | `false` | no |
| <a name="input_helm_wait_for_jobs"></a> [helm\_wait\_for\_jobs](#input\_helm\_wait\_for\_jobs) | If wait is enabled, will wait until all helm Jobs have been completed before marking the release as successful. It will wait for as long as timeout | `bool` | `false` | no |
| <a name="input_irsa_additional_policies"></a> [irsa\_additional\_policies](#input\_irsa\_additional\_policies) | Map of the additional policies to be attached to default role. Where key is arbitrary id and value is policy arn. | `map(string)` | `{}` | no |
| <a name="input_irsa_role_create"></a> [irsa\_role\_create](#input\_irsa\_role\_create) | Whether to create IRSA role and annotate service account | `bool` | `true` | no |
| <a name="input_irsa_role_name_prefix"></a> [irsa\_role\_name\_prefix](#input\_irsa\_role\_name\_prefix) | The IRSA role name prefix for argo-cd | `string` | `"argocd-irsa"` | no |
| <a name="input_irsa_tags"></a> [irsa\_tags](#input\_irsa\_tags) | IRSA resources tags | `map(string)` | `{}` | no |
| <a name="input_namespace"></a> [namespace](#input\_namespace) | The K8s namespace in which the argo-cd service account has been created | `string` | `"argo"` | no |
| <a name="input_self_managed"></a> [self\_managed](#input\_self\_managed) | If set to true, the module will create ArgoCD Application manifest in the cluster and abandon the Helm release | `bool` | `true` | no |
| <a name="input_service_accounts"></a> [service\_accounts](#input\_service\_accounts) | The k8s argo-cd service accounts | <pre>object({<br>    controller = object({<br>      create = bool<br>      name   = string<br>    })<br>    applicationSet = object({<br>      create = bool<br>      name   = string<br>    })<br>    server = object({<br>      create = bool<br>      name   = string<br>    })<br>    dex = object({<br>      create = bool<br>      name   = string<br>    })<br>    repoServer = object({<br>      create = bool<br>      name   = string<br>    })<br>  })</pre> | <pre>{<br>  "applicationSet": {<br>    "create": true,<br>    "name": "argocd-applicationset-controller"<br>  },<br>  "controller": {<br>    "create": true,<br>    "name": "argocd-application-controller"<br>  },<br>  "dex": {<br>    "create": true,<br>    "name": "argocd-dex-server"<br>  },<br>  "repoServer": {<br>    "create": true,<br>    "name": "argocd-repo-server"<br>  },<br>  "server": {<br>    "create": true,<br>    "name": "argocd-server"<br>  }<br>}</pre> | no |
| <a name="input_settings"></a> [settings](#input\_settings) | Additional helm sets which will be passed to the Helm chart values, see https://artifacthub.io/packages/helm/argo/argo-cd?modal=values | `map(any)` | `{}` | no |
| <a name="input_values"></a> [values](#input\_values) | Additional yaml encoded values which will be passed to the Helm chart, see https://artifacthub.io/packages/helm/argo/argo-cd?modal=values | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_helm_release_application_metadata"></a> [helm\_release\_application\_metadata](#output\_helm\_release\_application\_metadata) | ArgoCD application helm release attributes |
| <a name="output_helm_release_metadata"></a> [helm\_release\_metadata](#output\_helm\_release\_metadata) | Helm release attributes |
| <a name="output_helm_release_self_managed_metadata"></a> [helm\_release\_self\_managed\_metadata](#output\_helm\_release\_self\_managed\_metadata) | Helm release attributes |
| <a name="output_iam_role_attributes"></a> [iam\_role\_attributes](#output\_iam\_role\_attributes) | ArgoCD IAM role attributes |
| <a name="output_kubernetes_application_attributes"></a> [kubernetes\_application\_attributes](#output\_kubernetes\_application\_attributes) | ArgoCD kubernetes manifest attributes |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Contributing and reporting issues

Feel free to create an issue in this repository if you have questions, suggestions or feature requests.

### Validation, linters and pull-requests

We want to provide high quality code and modules. For this reason we are using
several [pre-commit hooks](.pre-commit-config.yaml) and
[GitHub Actions workflow](.github/workflows/). A pull-request to the
master branch will trigger these validations and lints automatically. Please
check your code before you will create pull-requests. See
[pre-commit documentation](https://pre-commit.com/) and
[GitHub Actions documentation](https://docs.github.com/en/actions) for further
details.


## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
