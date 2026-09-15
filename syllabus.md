# CKA Full Course — Syllabus & Progress Tracker

**Source:** Certified Kubernetes Administrator Full Course For Beginners (CKA 2026) — Tech Tutorials with Piyush
**Total:** 59 videos | Last updated May 5, 2026

---

## Module 1 — Docker Foundations
*Days 0–3 · Skippable if you already know Docker*

| Day | Topic                      | Notes | Labs | LinkedIn |
| --- | -------------------------- | :---: | :--: | :------: |
| 0   | Course intro + CKA roadmap |  [ ]  | [ ]  |   [ ]    |
| 1   | Docker fundamentals        |  [ ]  | [ ]  |   [ ]    |
| 2   | Dockerizing a project      |  [ ]  | [ ]  |   [ ]    |
| 3   | Multi-stage Docker builds  |  [ ]  | [ ]  |   [ ]    |

---

## Module 2 — Kubernetes Core Concepts
*Days 4–10*

| Day | Topic                                                        | Notes | Labs | LinkedIn |
| --- | ------------------------------------------------------------ | :---: | :--: | :------: |
| 4   | Why Kubernetes is used                                       |  [ ]  | [ ]  |   [ ]    |
| 5   | Kubernetes architecture                                      |  [ ]  | [ ]  |   [ ]    |
| 6   | Multi-node cluster setup with Kind                           |  [ ]  | [ ]  |   [ ]    |
| 7   | Pods, YAML, imperative vs declarative                        |  [ ]  | [ ]  |   [ ]    |
| 8   | Deployments, ReplicationController, ReplicaSet               |  [ ]  | [ ]  |   [ ]    |
| 9   | Services: ClusterIP / NodePort / LoadBalancer / ExternalName |  [ ]  | [ ]  |   [ ]    |
| 10  | Namespaces                                                   |  [ ]  | [ ]  |   [ ]    |

---

## Module 3 — Workloads & Pod Design
*Days 11–12*

| Day | Topic                                 | Notes | Labs | LinkedIn |
| --- | ------------------------------------- | :---: | :--: | :------: |
| 11  | Multi-container pods: sidecar vs init |  [ ]  | [ ]  |   [ ]    |
| 12  | DaemonSets, Jobs, CronJobs            |  [ ]  | [ ]  |   [ ]    |

---

## Module 4 — Scheduling & Resource Management
*Days 13–18*

| Day | Topic                                              | Notes | Labs | LinkedIn |
| --- | -------------------------------------------------- | :---: | :--: | :------: |
| 13  | Static pods, manual scheduling, labels & selectors |  [ ]  | [ ]  |   [ ]    |
| 14  | Taints and tolerations                             |  [ ]  | [ ]  |   [ ]    |
| 15  | Node affinity                                      |  [ ]  | [ ]  |   [ ]    |
| 16  | Requests and limits                                |  [ ]  | [ ]  |   [ ]    |
| 17  | Autoscaling: HPA vs VPA                            |  [ ]  | [ ]  |   [ ]    |
| 18  | Health probes: liveness vs readiness               |  [ ]  | [ ]  |   [ ]    |

---

## Module 5 — Security
*Days 19–26*

| Day | Topic                                   | Notes | Labs | LinkedIn |
| --- | --------------------------------------- | :---: | :--: | :------: |
| 19  | ConfigMaps and Secrets                  |  [ ]  | [ ]  |   [ ]    |
| 20  | How SSL/TLS works                       |  [ ]  | [ ]  |   [ ]    |
| 21  | Managing TLS certificates, CSR workflow |  [ ]  | [ ]  |   [ ]    |
| 22  | Authentication and authorization        |  [ ]  | [ ]  |   [ ]    |
| 23  | RBAC: Roles and RoleBindings            |  [ ]  | [ ]  |   [ ]    |
| 24  | ClusterRoles and ClusterRoleBindings    |  [ ]  | [ ]  |   [ ]    |
| 25  | Service accounts                        |  [ ]  | [ ]  |   [ ]    |
| 26  | Network policies                        |  [ ]  | [ ]  |   [ ]    |

---

## Module 6 — Cluster Setup & Storage
*Days 27–29*

| Day | Topic                                           | Notes | Labs | LinkedIn |
| --- | ----------------------------------------------- | :---: | :--: | :------: |
| 27  | Multi-node cluster with kubeadm                 |  [ ]  | [ ]  |   [ ]    |
| 28  | Docker volumes, bind mounts, persistent storage |  [ ]  | [ ]  |   [ ]    |
| 29  | PersistentVolumes, PVCs, StorageClasses         |  [ ]  | [ ]  |   [ ]    |

---

## Module 7 — Networking
*Days 30–33*

| Day | Topic                      | Notes | Labs | LinkedIn |
| --- | -------------------------- | :---: | :--: | :------: |
| 30  | DNS fundamentals           |  [ ]  | [ ]  |   [ ]    |
| 31  | CoreDNS in Kubernetes      |  [ ]  | [ ]  |   [ ]    |
| 32  | Cluster networking and CNI |  [ ]  | [ ]  |   [ ]    |
| 33  | Ingress                    |  [ ]  | [ ]  |   [ ]    |

---

## Module 8 — Cluster Maintenance
*Days 34–36*

| Day | Topic                                       | Notes | Labs | LinkedIn |
| --- | ------------------------------------------- | :---: | :--: | :------: |
| 34  | Upgrading a multi-node cluster with kubeadm |  [ ]  | [ ]  |   [ ]    |
| 35  | etcd backup and restore                     |  [ ]  | [ ]  |   [ ]    |
| 36  | Logging and monitoring                      |  [ ]  | [ ]  |   [ ]    |

---

## Module 9 — Troubleshooting & Exam Prep
*Days 37–42*

| Day | Topic                                    | Notes | Labs | LinkedIn |
| --- | ---------------------------------------- | :---: | :--: | :------: |
| 37  | Application failure troubleshooting      |  [ ]  | [ ]  |   [ ]    |
| 38  | Control plane failure troubleshooting    |  [ ]  | [ ]  |   [ ]    |
| 39  | Worker node failure troubleshooting      |  [ ]  | [ ]  |   [ ]    |
| 40  | JSONPath and advanced kubectl            |  [ ]  | [ ]  |   [ ]    |
| 41  | CKA exam tips and strategy               |  [ ]  | [ ]  |   [ ]    |
| 42  | Hosting a private Docker registry on K8s |  [ ]  | [ ]  |   [ ]    |

---

## Module 10 — Advanced / Post-40 Additions
*Days 43–55 · Not optional — see notes*

| Day | Topic                                                        | Notes | Labs | LinkedIn |
| --- | ------------------------------------------------------------ | :---: | :--: | :------: |
| 43  | Helm charts                                                  |  [ ]  | [ ]  |   [ ]    |
| 44  | Kustomize                                                    |  [ ]  | [ ]  |   [ ]    |
| 45  | StatefulSets                                                 |  [ ]  | [ ]  |   [ ]    |
| 46  | Pod priority and preemption                                  |  [ ]  | [ ]  |   [ ]    |
| 47  | Gateway API vs Ingress                                       |  [ ]  | [ ]  |   [ ]    |
| 48  | Migrating Ingress to Gateway API                             |  [ ]  | [ ]  |   [ ]    |
| 49  | Custom Resource Definitions (CRD, CR)                        |  [ ]  | [ ]  |   [ ]    |
| 50  | Operators                                                    |  [ ]  | [ ]  |   [ ]    |
| 51  | Admission controllers                                        |  [ ]  | [ ]  |   [ ]    |
| 52  | Storage classes: static vs dynamic provisioning              |  [ ]  | [ ]  |   [ ]    |
| 53  | Installing cri-dockerd runtime                               |  [ ]  | [ ]  |   [ ]    |
| 54  | Pod Security Standards, Linux capabilities, security context |  [ ]  | [ ]  |   [ ]    |
| 55  | Multi-master cluster with load balancer                      |  [ ]  | [ ]  |   [ ]    |

---

