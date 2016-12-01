# `tick-charts`

This is a collection of [Helm](https://github.com/kubernetes/helm) [Charts](https://github.com/kubernetes/charts) for the [InfluxData](https://influxdata.com/time-series-platform) TICK stack. This repo contains the following charts:

- [influxdb](/influxdb/README.md)
- [chronograf](/chronograf/README.md)
- [kapacitor](/kapacitor/README.md)
- [telegraf](/telegraf/README.md)

### Deploy the whole stack!

- Have your `kubectl` tool configured for the cluster where you would like to deploy the stack.
- Have `helm` and `tiller` installed and configured
  - Download and configure the `helm` cli
    * [link](https://github.com/kubernetes/helm/blob/master/docs/install.md)
  - Run `helm init` to install `tiller` in your cluster
    * [link](https://github.com/kubernetes/helm/blob/master/docs/install.md#installing-tiller)
- Package all the charts:
```bash
$ helm package chronograf influxdb kapacitor telegraf
```
- Install the charts:
```bash
$ helm install influxdb-0.1.0.tgz --name influxdb --namespace tick
$ helm install telegraf-0.1.0.tgz --name telegraf --namespace tick
$ helm install kapacitor-0.1.0.tgz --name kapacitor --namespace tick
$ helm install chronograf-0.1.0.tgz --name chronograf --namespace tick
```
- Wait for the IP for chronograf to appear:
```bash
$ kubectl get svc -w --namespace tick chronograf-chronograf
```
- Open chronograf in your browser and configure it
  - InfluxDB URL: `http://influxdb-influxdb.tick:8086`
  - Kapacitor URL: `http://kapacitor-kapacitor.tick:9092`

### Usage

To package any of the charts for deployment:

```bash
$ helm package /path/to/chart
```

This will create a file named `{{ .Chart.Name }}-{{ .Chart.Version }}.tgz` that is the chart file to be deployed. The default configurations are listed in the `values.yaml` file in the root of each repo. To deploy the chart with some default values create your custom `values.yaml` file to change the default configuration or modify the `values.yaml` file at the root of the chart before packaging it:

```bash
$ helm install telegraf-0.1.0.tgz --name {{ .Release.Name }} --namespace {{ .Release.Namespace }} --values /path/to/my_values.yaml
```

### NOTES

- Configmap changes - 
