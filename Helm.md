## Helm is a **Package Manager** for Kubernetes Cluster.

### Package Installation [Artifact Hub](https://artifacthub.io)

- Adding a repo

```helm repo add <your_choice_of_label_name_for_repo_URL> <repo_URL>```

- Install a package(chart) into a k8s cluster

```helm install <your_choice_of_label_name_for_chart> <given_label_name_for_repo_url/package_name_repo>```

- List releases of charts

```helm list```

- Listing registered repos

```helm repo list```

- Kubectl (Edit yaml configs of running pod, service ...)

```kubectl edit <of_kind_you_want_to_edit> <according_name_you_want_to_edit>```

## Version Control Helm Chart

- Pulling Chart's Source Code for Version Control

```helm pull <given_label_name_for_repo_url/package_name_repo>```

eg. ```helm pull prom-repo/kube-prometheus-stack```

- Install a package(chart) into a k8s cluster from locally pulled chart

```helm install <given_label_name_for_chart> <chart_folder>```

eg. ```helm install monitoring ./kube-prometheus-stack```

- Generate a k8s yaml config file from Helm

```helm template <your_choice_of_label_name_for_chart> <chart_folder> <--options>```

eg. ```helm template monitoring ./kube-prometheus-stack > monitoring-stack.yaml```

eg. ```helm template monitoring ./kube-prometheus-stack --values=./kube-prometheus-stack/myvalues.yaml > monitoring-stack.yaml```

## Working with Chart Values
A Chart is mostly going to have some configurations coming with it, and these are called values in a Helm Chart.

- Showing value of a chart

```helm show values <given_label_name_for_repo_url/package_name_repo>```

- Upgrading a chart's default value. (After Chart's Installation)

```helm upgrade <given_label_name_for_chart> <given_label_name_for_repo_url/package_name_repo> --set <hierarchy_value.(if present)><key>=<value>```

- Upgrading a chart's default value. (During Chart's Installation)

```helm install <given_label_name_for_chart> <given_label_name_for_repo_url/package_name_repo> --set <hierarchy_key.(if present)><key>=<value>```

eg. ```helm upgrade monitoring prom-repo/kube-prometheus-stack --set grafana.adminPassword=admin```

**During and After installation, if the set key is not present in the chart, the key will be created**
