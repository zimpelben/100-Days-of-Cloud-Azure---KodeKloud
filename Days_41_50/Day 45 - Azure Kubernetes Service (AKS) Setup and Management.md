The Nautilus DevOps team is tasked with preparing an AKS cluster to deploy a Kubernetes-based application. The team has the following requirements.

1. Create an AKS cluster named `xfusion-aks`.
2. The Kubernetes version must be `1.33.0`.
3. The AKS cluster endpoint access must be private.
4. Ensure the cluster is created in the `Central US` region.
5. Edit the `agentpool` Node pools (delete all other node pool if exists) and configure the cluster with the following properties:
	- Node size: `D2s v3`
	- Minimum node count: `1`
	- Maximum node count: `2`
- Disable the `Container Insights` for now and disable all kind of monitoring as well.

The AKS cluster must be configured with high availability and private endpoint access. Verify that the cluster meets the requirements and is ready for workloads. 

**Notes**:
- Create the resource only in the `Central US` region.
- Ensure that the Kubernetes version is `1.33.0`.

### Answer using Azure Portal

1. Sign into the portal with the credentials provided.
2. Click on Kubernetes Services.
3. Create a Kubernetes cluster.
4. Cluster details;
	- Cluster preset configuration = Dev/Test
	- Kubernetes cluster name = `xfusion-aks`
	- Kubernetes version = `1.33.0`
5. Node pools;
	- Edit the existing agentpool.
	- Change node size to `D2s v3`
	- Minimum node count = `1`
	- Maximum node count = `2`
6. Networking;
	- Enable private cluster = true
7. Monitoring;
	- Enable Container logs = false
	- Ensure all monitoring is off as well.
8. Leave the rest on default and click Review + create.
9. Create the cluster and wait for it to be fully deployed.
10. Once the cluster is deployed and running, click check to complete the task.
