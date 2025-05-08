---
title: 'using Pod Security Standards for baremetal workloads'
description: 'This article describes how to use Pod Security Settings in Harvester.'
authors:
  - name: Gaurav Mehta
    title: Principal Software Engineer
    url: https://github.com/ibrokethecloud
tags: [security]
hide_table_of_contents: false
---

Harvester provides experimental support for running [baremetal container workloads](https://docs.harvesterhci.io/v1.5/rancher/rancher-integration#harvester-baremetal-container-workload-support-experimental)

Users wishing to prevent privilege escalation and other security issues can leverage Pod Security Standards(PSS) on Harvester.

Currently the  [baseline](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline) works on Harvester v1.5.0 for most workloads, except VM's with device passthrough. 

VM's using device passthrough, such as pcidevices, usbdevices, vgpudevices will fail to start, as they need SYS_RESOURCE capability. This is being tracked viw Github [issue-8218](https://github.com/harvester/harvester/issues/8218). A fix should be available for the same shortly.

To enable PSS a user simply needs to label their workload namespace as follows:

```
kubectl label --overwrite ns <namespace>  pod-security.kubernetes.io/enforce=baseline
```

:::note

Please do not apply PSS to the system namespaces, as they need privileged escalation to manage cluster resources

:::