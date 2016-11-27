# `tick-charts`

This is a collection of [Helm](https://github.com/kubernetes/helm) [Charts](https://github.com/kubernetes/charts) for the [InfluxData](https://influxdata.com/time-series-platform) TICK stack. This repo contains the following charts:

- [influxdb](/influxdb/README.md)
- [chronograf](/chronograf/README.md)
- [kapacitor](/kapacitor/README.md)
- [telegraf-daemonset](/telegraf-daemonset/README.md)
- [telegraf-single](/telegraf-single/README.md)

### Dependencies

To use the charts in this repo you need a working Helm CLI and a `tiller` container running in a Kubernetes or Minikube instance. To do this first [install the `helm` cli](https://github.com/kubernetes/helm/blob/master/docs/install.md). Then `tiller` can be installed in which ever cluster you have your `kubectl` configured to use by calling `helm init`. Once you have installed and initialized helm in your cluster you can use this repo.

### Usage

To package any of the charts for deployment:

```
$ helm package /path/to/chart
```

This will create a file named `{{ .Chart.Name }}-{{ .Chart.Version }}.tgz` that is the chart file to be deployed. The default configurations are listed in the `values.yaml` file in the root of each repo. To deploy the chart with some default values create your custom `values.yaml` file to change the default configuration or modify the `values.yaml` file at the root of the chart before packaging it:

```
$ helm install telegraf-single-0.1.0.tgz --name {{ .Release.Name }} --namespace {{ .Release.Namespace }} --values /path/to/my_values.yaml
```