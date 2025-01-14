---
aliases:
  - ../features/datasources/phlare/
  - ../features/datasources/grafana-pyroscope/
description: Horizontally-scalable, highly-available, multi-tenant continuous profiling
  aggregation system. OSS profiling solution from Grafana Labs.
keywords:
  - grafana
  - phlare
  - guide
  - profiling
  - pyroscope
labels:
  products:
    - cloud
    - enterprise
    - oss
title: Grafana Pyroscope
weight: 1150
---

# Grafana Pyroscope data source

Grafana Pyroscope is a horizontally scalable, highly available, multi-tenant, OSS, continuous profiling aggregation system. Add it as a data source, and you are ready to query your profiles in [Explore][explore].

## Configure the Grafana Pyroscope data source

To configure basic settings for the data source, complete the following steps:

1. Click **Connections** in the left-side menu.
1. Under Your connections, click **Data sources**.
1. Enter `Grafana Pyroscope` in the search bar.
1. Click **Grafana Pyroscope** to display the **Settings** tab of the data source.

1. Set the data source's basic configuration options:

   | Name           | Description                                                                                                                                                                                                                                                                                                  |
   | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
   | `Name`         | A name to specify the data source in panels, queries, and Explore.                                                                                                                                                                                                                                           |
   | `Default`      | The default data source will be pre-selected for new panels.                                                                                                                                                                                                                                                 |
   | `URL`          | The URL of the Grafana Pyroscope instance, for example, `http://localhost:4100`.                                                                                                                                                                                                                             |
   | `Basic Auth`   | Enable basic authentication to the data source.                                                                                                                                                                                                                                                              |
   | `User`         | User name for basic authentication.                                                                                                                                                                                                                                                                          |
   | `Password`     | Password for basic authentication.                                                                                                                                                                                                                                                                           |
   | `Minimal step` | Used for queries returning timeseries data. The Pyroscope backend, similar to Prometheus, scrapes profiles at certain intervals. To prevent querying at smaller interval, use Minimal step same or higher than your Pyroscope scrape interval. This prevents returning too many data points to the frontend. |

### Traces to profiles

You can link profile and tracing data using your Pyroscope data source with the Tempo data source.
For more information, refer to the [Traces to profile section][configure-tempo-data-source] of the Tempo data source documentation.

{{< youtube id="AG8VzfFMLxo" >}}

## Querying

You can query your profiling data using the query editor.

### Query editor

The query editor gives you access to a profile type selector, a label selector, and collapsible options.

![Query editor](/media/docs/pyroscope/query-editor/query-editor.png 'Query editor')

To access the query editor:

1. Sign into Grafana or Grafana Cloud.
1. Select your Pyroscope data source.
1. From the menu, choose **Explore**.

1. Select a profile type from the drop-down menu.

   {{< figure src="/media/docs/pyroscope/query-editor/select-profile.png" class="docs-image--no-shadow" max-width="450px" caption="Profile selector" >}}

1. Use the labels selector input to filter by labels. Pyroscope uses similar syntax to Prometheus to filter labels.
   Refer to [Pyroscope documentation](https://grafana.com/docs/pyroscope/latest/) for available operators and syntax.

   While the label selector can be left empty to query all profiles without filtering by labels, the profile type or app must be selected for the query to be valid.

   Grafana doesn't show any data if the profile type or app isn’t selected when a query runs.

   ![Labels selector](/media/docs/pyroscope/query-editor/labels-selector.png 'Labels selector')

1. Expand the **Options** section to view **Query Type** and **Group by**.
   ![Options section](/media/docs/pyroscope/query-editor/options-section.png 'Options section')

1. Select a query type to return the profile data which can be shown in the [Flame Graph][flame-graph], metric data visualized in a graph, or both. You can only select both options in a dashboard, because panels allow only one visualization.

**Group by** allows you to group metric data by a specified label. Without any **Group by** label, metric data is aggregated over all the labels into single time series. You can use multiple labels to group by. Group by has only an effect on the metric data and doesn't change the profile data results.

### Profiles query results

Profiles can be visualized in a flame graph. See the [Flame Graph documentation][flame-graph] to learn about the visualization and its features.

![Flame graph](/media/docs/pyroscope/query-editor/flame-graph.png 'Flame graph')

Pyroscope returns profiles aggregated over a selected time range.
The absolute values in the flame graph grow as the time range gets bigger while keeping the relative values meaningful.
You can zoom in on the time range to get a higher granularity profile up to the point of a single scrape interval.

### Metrics query results

Metrics results represent the aggregated sum value over time of the selected profile type.

![Metrics graph](/media/docs/pyroscope/query-editor/metric-graph.png 'Metrics graph')

This allows you to quickly see any spikes in the value of the scraped profiles and zoom in to a particular time range.

## Provision the Grafana Pyroscope data source

You can modify the Grafana configuration files to provision the Grafana Pyroscope data source. To learn more, and to view the available provisioning settings, see [provisioning documentation][provisioning-data-sources].

Here is an example configuration:

```yaml
apiVersion: 1

datasources:
  - name: Grafana Pyroscope
    type: grafana-pyroscope-datasource
    url: http://localhost:4040
    jsonData:
      minStep: '15s'
```

{{% docs/reference %}}
[explore]: "/docs/grafana/ -> /docs/grafana/<GRAFANA VERSION>/explore"
[explore]: "/docs/grafana-cloud/ -> /docs/grafana/<GRAFANA VERSION>/explore"

[flame-graph]: "/docs/grafana/ -> /docs/grafana/<GRAFANA VERSION>/panels-visualizations/visualizations/flame-graph"
[flame-graph]: "/docs/grafana-cloud/ -> /docs/grafana/<GRAFANA VERSION>/panels-visualizations/visualizations/flame-graph"

[provisioning-data-sources]: "/docs/grafana/ -> /docs/grafana/<GRAFANA VERSION>/administration/provisioning#datasources"
[provisioning-data-sources]: "/docs/grafana-cloud/ -> /docs/grafana/<GRAFANA VERSION>/administration/provisioning#datasources"

[configure-tempo-data-source]: "/docs/grafana/ -> /docs/grafana/<GRAFANA VERSION>/datasources/tempo/configure-tempo-data-source"
[configure-tempo-data-source]: "/docs/grafana-cloud/ -> docs/grafana-cloud/connect-externally-hosted/data-sources/tempo/configure-tempo-data-source"
{{% /docs/reference %}}
