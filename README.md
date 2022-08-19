# Policy Generator Lab

Demo lab to test Policy Generator features and integration with RHACM.

## Testing builds on a local machine

The Policy Generator plugin must be already installed locally. Follow the instructions below to install it 
on the local machine.
```
$ mkdir -p ${HOME}/.config/kustomize/plugin/policy.open-cluster-management.io/v1/policygenerator
$ export POLICYGEN_RELEASE=v1.8.0
$ curl -L \
  -o ${HOME}/.config/kustomize/plugin/policy.open-cluster-management.io/v1/policygenerator/PolicyGenerator \
  https://github.com/stolostron/policy-generator-plugin/releases/download/${POLICYGEN_RELEASE}/linux-amd64-PolicyGenerator
chmod +x ${HOME}/.config/kustomize/plugin/policy.open-cluster-management.io/v1/policygenerator/PolicyGenerator
```

To test the policy build generation locally, run the following command:
```
$ kustomize build --enable-alpha-plugins <TARGET_DIR>
```

Or, alternatively:
```
$ kubectl kustomize --enable-alpha-plugins <TARGET_DIR>
```

## Deploying policies on RHACM

On Red Hat Advanced Cluster Management for Kubernetes polices can either be created manually (by applying the locally
generated manifests) or by enforcing a GitOps approach using the Application Lifecycle Management feature.
When applying policies, a real application is not necessarily created, since the enforcing will be related to the 
Policy manifests, along with PlacementBindings and PlacementRules. However, this approach provides an effective and reliable
way to dinamically generate the policies on the cluster since RHACM already includes the Policy Generator plugin
natively.

The following example deploys a new Image Config policy on the Hub cluster:
```
$ kubectl apply -f rhacm_gitops_integration/image-config-policy-deploy.yaml
```

The policy is enforced on all cluster defined inside the generated PlacementRule.

## Futher Readings
- https://cloud.redhat.com/blog/generating-governance-policies-using-kustomize-and-gitops
- https://github.com/stolostron/policy-generator-plugin

