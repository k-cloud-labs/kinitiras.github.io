---
title: Advanced Usage
weight: 2
---

{{< toc >}}


{{< hint type=tip >}}
Can reference `http` api, current resource `ownerReference`, other resource from current `k8s cluster` as value for override and validate.
{{< /hint >}}

# OverridePolicy

# ClusterValidatePolicy

## PodAvailableBadge

```yaml
apiVersion: policy.kcloudlabs.io/v1alpha1
kind: ClusterValidatePolicy
metadata:
  name: test-delete-protection
  labels:
    kinitiras.kcloudlabs.io/webhook: enabled
spec:
  resourceSelectors:
    - apiVersion: v1
      kind: Pod # match all Pod | can add label to test in part of Pods
  validateRules:
    - targetOperations:
        - DELETE
      template:
        type: pab
        podAvailableBadge:
          maxUnavailable: 60%
#          In most case, no need to set replica reference since webhook will try to get replica from
#          current pod owner reference spec.replica and status.replica.
#          Only when pod running with custom workload or no k8s workload(bromo) then set replica reference.
#          replicaReference:
#            from: k8s
#            currentReplicaPath: "/status/replica"
#            targetReplicaPath: "/spec/replica"
#            k8s:
#              apiVersion: apps.ecp.shopee.io/v1
#              kind: Workload
#              namespace: "{{metadata.namespace}}"
#              name: "{{metadata.annotation.workload-name}}"
#            from: http
#            currentReplicaPath: "body.data.current"
#            targetReplicaPath: "body.data.replica"
#            http:
#              url: xxx
#              method: get
#              params:
#                app: "{{metadata.annotations.app}}"
```