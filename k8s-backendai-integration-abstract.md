Kubernetes is becoming the "de-facto OS of AI", according to CNCF. However, AI-native platform features like fine-grained accelerator sharing, topology-aware workload placement and distributed job semantics are not yet supported first-class in Kubernetes. Third-party AI platforms cover these gaps. But operators still need a way to deploy them onto Kubernetes.

The first approach, Lagrange, is a Virtual Kubelet-based bridge. It projects capacity managed by the external platform into Kubernetes as virtual nodes, while the external platform keeps scheduling authority over the accelerator workloads.

The second approach, DooD, keeps Kubernetes as the host of the control-plane components, but runs accelerator workloads outside the Kubernetes execution path via a host-level Docker-out-of-Docker mechanism.

Neither is a clean win. Lagrange preserves clear ownership boundaries but forces operators to run and reconcile two schedulers. DooD collapses the cluster surface to one system but opens hard problems in networking coexistence, storage propagation, and security boundaries.
