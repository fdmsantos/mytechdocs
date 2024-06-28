# Helm

## Application Lifecycle

```shell
helm install release-1 wordpress
helm upgrade release-1
helm rollback release-1
helm uninstall release-1
```

## Change Charts

```shell
helm list
helm pull --untar bitnami/wordpress
helm install release-4 ./wordpress
```

## Repo Management

```shell
helm search hub wordpress
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami/joomla
helm repo list
```

## Management Commands

```shell
helm env
helm version
helm --debug
```