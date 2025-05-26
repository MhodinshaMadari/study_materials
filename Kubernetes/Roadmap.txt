Kubernetes Learning Roadmap


---

1. Prerequisites

Linux basics

Docker and containerization

YAML syntax

Basic networking (DNS, ports, IPs, firewalls)


References:

Docker: https://docs.docker.com/get-started/

YAML: https://learnxinyminutes.com/docs/yaml/



---

2. Kubernetes Fundamentals

Goal: Understand basic concepts and architecture

Topics:

What is Kubernetes?

Cluster architecture: Control Plane & Nodes

Core objects: Pods, Deployments, Services

Namespaces, ConfigMaps, Secrets


References:

https://kubernetes.io/docs/concepts/

https://kubernetes.io/docs/tutorials/kubernetes-basics/

https://github.com/kelseyhightower/kubernetes-the-hard-way



---

3. Hands-On Practice

Goal: Get familiar with tools and commands

Tools:

Minikube: https://minikube.sigs.k8s.io/docs/

Kind: https://kind.sigs.k8s.io/

Kubectl: https://kubernetes.io/docs/reference/kubectl/cheatsheet/


Practice:

Deploy sample apps (Nginx, etc.)

Manage deployments and services

Use YAML manifests


Platforms:

https://labs.play-with-k8s.com/

https://www.katacoda.com/courses/kubernetes



---

4. Networking & Service Discovery

Topics:

ClusterIP, NodePort, LoadBalancer

Ingress & Ingress Controllers

CoreDNS and service discovery


References:

https://kubernetes.io/docs/concepts/cluster-administration/networking/

https://kubernetes.github.io/ingress-nginx/



---

5. Advanced Concepts

Topics:

Volumes and Persistent Storage

StatefulSets, DaemonSets, Jobs

Taints, Tolerations, Affinities

RBAC, Service Accounts


References:

https://kubernetes.io/docs/concepts/storage/

https://kubernetes.io/docs/reference/access-authn-authz/rbac/



---

6. Observability & Debugging

Topics:

Logs, Events, kubectl describe

Liveness & Readiness Probes

Prometheus, Grafana, Lens


References:

https://kubernetes.io/docs/tasks/debug/

https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/



---

7. CI/CD and GitOps

Topics:

Helm, Kustomize

ArgoCD, Flux

GitOps workflows


References:

https://helm.sh/docs/

https://kubectl.docs.kubernetes.io/references/kustomize/

https://argo-cd.readthedocs.io/



---

8. Cloud & Production Readiness

Topics:

Deploying to GKE, EKS, AKS

Network Policies

Security best practices


References:

https://cloud.google.com/kubernetes-engine/docs/quickstart

https://kubernetes.io/docs/concepts/security/overview/



---

9. Certification (Optional)

CKA: Certified Kubernetes Administrator

CKAD: Certified Kubernetes Application Developer


Resources:

https://killer.sh/

https://training.linuxfoundation.org/

https://www.udemy.com/user/mumshadmannambeth/


