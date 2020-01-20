---
layout: single
title: Kubernetes Setup
---

#### Kubernetes

<div class="mermaid">
graph LR
    id1[Kubernetes] --> id2[Kubelet]
    style id1 fill:#ddd,stroke:#333,stroke-width:2px, padding-top:10px;
    id2 --> oci[OCI]
    subgraph OCI Runtime
    oci -->|CRI-O| c1[RunC]
    oci -->|CRI-O| c2[RunC]
    oci -->|CRI-O| c3[RunC]
    end    
</div>

[https://computingforgeeks.com/docker-vs-cri-o-vs-containerd/](docker vs crio vs containerd)

Squeked in a PR [https://github.com/securekubernetes/cloud-shell-setup/pull/1](GCP Multiple billing accounts)

