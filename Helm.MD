## Helm is a **Package Manager** for Kubernetes Cluster.

### Package Installation [Artifact Hub](https://artifacthub.io)

- Adding a repo

```helm repo add <your_choice_of_label_name_for_the_repo_URL> <repo_URL>```

- Install a package(chart) into a k8s cluster

```helm install <your_choice_of_label_name_for_the_chart> <name_of_the_package_from_the_repo>```

- Listing registered repos

```helm repo list```

- Kubectl (Edit yaml configs of running pod, service ...)

```kubectl edit <of_kind_you_want_to_edit> <according_name_you_want_to_edit>
