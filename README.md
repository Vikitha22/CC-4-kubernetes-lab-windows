\# Kubernetes Lab on Windows



\## Cluster Deployment, Scaling and Self-Healing



\## 1. Project Overview



This project demonstrates fundamental Kubernetes operations using a local multi-node cluster on Windows. The experiment uses kind (Kubernetes in Docker), Docker Desktop, `kubectl`, and NGINX to explore application deployment, replica scaling, and Pod replacement.



\## 2. Objectives



\- Set up a local Kubernetes cluster with three nodes.

\- Deploy an NGINX application using a Deployment.

\- Monitor Pod status and node placement.

\- Scale the application from three replicas to six replicas.

\- Delete a Pod and observe Kubernetes replacement behavior.

\- Record the results and investigate any failures.



\## 3. Technologies Used



\- \*\*Operating System:\*\* Windows 11

\- \*\*Container Runtime:\*\* Docker Desktop

\- \*\*Cluster Tool:\*\* kind

\- \*\*Command-Line Tool:\*\* kubectl

\- \*\*Application:\*\* NGINX

\- \*\*Version Control:\*\* Git and GitHub



\## 4. Cluster Architecture



The cluster consists of one control-plane node and two worker nodes.



| Node | Role |

|---|---|

| kind-control-plane | Control plane |

| kind-worker | Worker node |

| kind-worker2 | Worker node |



The cluster configuration is stored in `kind-config.yaml`.



\## 5. Experiments Performed



\### Experiment 1: Cluster Creation



Created a three-node Kubernetes cluster using the kind configuration file.



```powershell

kind create cluster --config kind-config.yaml

kubectl get nodes

```



\### Experiment 2: NGINX Deployment



Deployed NGINX with three replicas.



```powershell

kubectl create deployment web --image=nginx --replicas=3

kubectl get deployments

kubectl get pods -o wide

```



\*\*Result:\*\* All three Pods reached the Running state.



\### Experiment 3: Scaling



Scaled the deployment from three replicas to six.



```powershell

kubectl scale deployment web --replicas=6

kubectl get pods -o wide

```



\*\*Result:\*\* All six Pods reached Running with readiness status `1/1`.



\### Experiment 4: Pod Deletion and Replacement



Deleted one Pod to observe Kubernetes reconciliation.



```powershell

kubectl delete pod <pod-name>

kubectl get pods -o wide

```



Replace `<pod-name>` with the actual Pod name.



\*\*Result:\*\* Kubernetes created a replacement Pod. However, the replacement encountered `ImagePullBackOff`. Full recovery remains to be verified after troubleshooting the image-pull issue.



\## 6. Useful Verification Commands



| Command | Purpose |

|---|---|

| `kubectl get nodes` | Check node status |

| `kubectl get pods -o wide` | Inspect Pod status and placement |

| `kubectl get deployments` | Check deployment replicas |

| `kubectl describe pod <pod-name>` | Investigate Pod details and events |

| `kubectl get events --sort-by=.metadata.creationTimestamp` | Inspect recent events |



\## 7. Results Summary



| Test | Observation |

|---|---|

| Three-node cluster | Created successfully |

| Three NGINX replicas | All running |

| Six NGINX replicas | All running |

| Pod deletion | Completed |

| Replacement Pod creation | Observed |

| Complete self-healing recovery | Pending investigation |



\## 8. Project Structure



```text

k8s-lab/

├── kind-config.yaml

└── README.md

```



\## 9. Future Improvements



\- Resolve the replacement Pod's `ImagePullBackOff` error.

\- Verify that the replacement Pod reaches Running.

\- Add screenshots documenting each experiment.

\- Explore rolling updates and additional recovery scenarios.



\## 10. Conclusion



This experiment demonstrates Kubernetes cluster setup, application deployment, scaling, and replacement Pod creation. It also highlights the importance of investigating image-pull errors when validating application recovery.



