# AAlternative install
https://technotim.live/posts/metal-as-a-service-packer/


# README: Adding Node Selector Labels for Proper Pod Scheduling


## Importance of Adding `ucp-control-plane=enabled` Label to Worker Nodes

In Kubernetes, pods can be scheduled on specific nodes using **node selectors**. These selectors ensure that pods are only scheduled on nodes that meet specific criteria, which can include labels assigned to those nodes.

### Why This Step is Critical

1. **Node Affinity Requirement**:
   Pods may specify a `nodeSelector` in their configuration that mandates them to only be scheduled on nodes with a certain label. In our case, the pods in the MAAS namespace required the label `ucp-control-plane=enabled`. Without this label on the worker nodes, Kubernetes won't be able to schedule the pods, leading to a "FailedScheduling" error.

2. **Avoid Control-Plane Scheduling**:
   The control-plane nodes are typically tainted to prevent workloads from running on them, as they are primarily responsible for managing the Kubernetes cluster. Applying the necessary label to the worker nodes ensures that workloads are appropriately scheduled on worker nodes and not the control-plane nodes.

3. **Improved Pod Scheduling**:
   By applying the `ucp-control-plane=enabled` label to worker nodes, it ensures that the scheduler can match the `nodeSelector` requirement, enabling the pods to run on the appropriate nodes. This step helps in properly balancing workloads and ensuring the stability of your control-plane nodes.

### Step-by-Step Guide

To apply the label to your worker nodes, use the following command:

```bash
kubectl label node <worker-node-name> ucp-control-plane=enabled
```

Example for three nodes:

```bash
kubectl label node rke2-worker-1-xl ucp-control-plane=enabled
kubectl label node rke2-worker-2-lg ucp-control-plane=enabled
kubectl label node rke2-worker-3-lg ucp-control-plane=enabled
```

After adding the label, verify it with:

```bash
kubectl get nodes --show-labels
```

Once the label is applied, Kubernetes will be able to schedule pods that require the `ucp-control-plane=enabled` label on these worker nodes.



# Unifyhub start
*               soft    nofile          65536
*               hard    nofile          65536
# Unifyhub end  