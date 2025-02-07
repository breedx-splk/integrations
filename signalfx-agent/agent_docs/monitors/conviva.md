
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# conviva

Monitor Type: `conviva` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/conviva))

**Accepts Endpoints**: No

**Multiple Instances Allowed**: Yes

## Overview

This monitor uses version 2.4 of the Conviva Experience Insights REST APIs to pull
`Real-Time/Live` video playing experience metrics from Conviva.

Only `Live` conviva metrics listed
[here](https://community.conviva.com/site/global/apis_data/experience_insights_api/index.gsp#metrics)
are supported. All metrics are gauges. The Conviva metrics are converted to SignalFx metrics with dimensions
named account and filter. The account dimension is the name of the Conviva account and the filter dimension
is the name of the Conviva filter applied to the metric. In the case of MetricLenses, the constituent
MetricLens metrics and MetricLens dimensions are included. The values of the MetricLens dimensions are
derived from the values of the associated MetricLens dimension entities.

Below is a sample YAML configuration showing the most basic configuration of the Conviva monitor
using only the required fields. For this configuration the monitor will default to fetching quality MetricLens
metrics for all dimensions from the default Conviva account using the `All Traffic` filter.

```
monitors:
- type: conviva
 pulseUsername: <username>
 pulsePassword: <password>
```

Individual metrics are configured as a list of metricConfigs as shown in sample configuration below. The
metrics a fetched using the specified metricParameter. Find the list of metric parameters [below](#conviva-monitor-metric-parameters-and-metrics).
The Conviva metrics reported to SignalFx are prefixed by `conviva.`, `conviva.quality_metriclens.` and
`conviva.audience_metriclens.` accordingly. The metric names are the `titles` of the metrics, which correspond to the Conviva
`metric parameters` [here](https://community.conviva.com/site/global/apis_data/experience_insights_api/index.gsp#metrics).
Where an account is not provided the default account is fetched and used. Where no filters are specified the
`All Traffic` filter is used. Where MetricLens dimensions are not specified all MetricLens dimensions
are fetched and used. The `_ALL_` keyword means all. MetricLens dimension configuration applies only to MetricLenses.
If specified for a regular metric they will be ignored. MetricLens dimensions listed in `excludeMetricLensDimensions`
will be excluded.

```
monitors:
- type: conviva
 pulseUsername: <username>
 pulsePassword: <password>
 metricConfigs:
   - account: c3.NBC
     metricParameter: quality_metriclens
     filters:
       - All Traffic
     metricLensDimensions:
       - Cities
   - metricParameter: avg_bitrate
     maxFiltersPerRequest: 99
     filters:
       - _ALL_
   - metricParameter: concurrent_plays
   - metricParameter: audience_metriclens
     filters:
       - All Traffic
     metricLensDimensions:
       - _ALL_
     excludeMetricLensDimensions:
       - CDNs
```

Add the extra dimension metric_source as shown in sample configuration below for the convenience of searching
for your metrics in SignalFx using the metric_source value you specify. Also, version 2.4 of the Conviva Experience
Insights REST APIs limits the number of filters per request to 99. Specify the maximum number of filters per request
using `maxFiltersPerRequest` as shown in the example above in order to limit the number of filters per request.

```
monitors:
- type: conviva
 pulseUsername: <username>
 pulsePassword: <password>
 extraDimensions:
   metric_source: conviva
```

## Conviva Monitor Metric Parameters and Metrics

Metric Parameters are conviva monitor metricParameter configuration values. Metrics are the metrics that get reported to SignalFx

|Metric Parameters|Metrics|Description|
|----------------|------|-----------|
|attempts|conviva.<br/>attempts|Attempts time-series|
|avg_bitrate|conviva.<br/>avg_bitrate|Average bitrate time-series|
|concurrent_plays|conviva.<br/>concurrent_plays|Concurrent plays time-series|
|connection_induced<br/>_rebuffering_ratio|conviva.<br/>connection_induced<br/>_rebuffering_ratio|Connection induced rebuffering ratio simple-series|
|connection_induced<br/>_rebuffering_ratio<br/>_timeseries|conviva.<br/>connection_induced<br/>_rebuffering_ratio<br/>_timeseries|Connection induced rebuffering ratio time-series|
|duration_connection<br/>_induced_rebuffering<br/>_ratio_distribution|conviva.<br/>duration_connection<br/>_induced_rebuffering<br/>_ratio_distribution|Duration vs. connection induced rebuffering ratio distribution label-series|
|exits_before<br/>_video_star|conviva.<br/>exits_before<br/>_video_start|Exits before video start time-series|
|ended_plays|conviva.<br/>ended_plays|Ended plays simple-series|
|ended_plays<br/>_timeseries|conviva.<br/>ended_plays<br/>_timeseries|Ended plays time-series|
|plays|conviva.<br/>plays|Plays time-series|
|play_bitrate<br/>_distribution|conviva.<br/>play_bitrate<br/>_distribution|Play bitrate distribution label-series|
|play_buffering<br/>_ratio_distribution|conviva.<br/>play_buffering<br/>_ratio_distribution|Play buffering ratio distribution label-series|
|play_connection<br/>_induced_rebuffering<br/>_ratio_distribution|conviva.<br/>play_connection<br/>_induced_rebuffering<br/>_ratio_distribution|Play connection induced rebuffering ratio distribution label-series|
|quality_summary|conviva.<br/>quality_summary|Quality summary label-series|
|rebuffered_plays|conviva.<br/>rebuffered_plays|Rebuffered plays time-series|
|rebuffering_ratio|conviva.<br/>rebuffering_ratio|Rebuffering ratio time-series|
|top_assets_15_mins|conviva.<br/>top_assets_15_mins|Top assets over last 15 minutes simple-table|
|top_assets_summary|conviva.<br/>top_assets_summary|Top assets summary label-series|
|video_playback<br/>_failures|conviva.<br/>video_playback<br/>_failures|Video playback failures simple-series|
|video_playback<br/>_failures_timeseries|conviva.<br/>video_playback<br/>_failures_timeseries|Video playback failures time-series|
|video_playback<br/>_failures_distribution|conviva.<br/>video_playback<br/>_failures_distribution|Video playback failures distribution label-series|
|video_restart<br/>_time|conviva.<br/>video_restart<br/>_time|Video restart time simple-series|
|video_restart<br/>_time_timeseries|conviva.<br/>video_restart<br/>_time_timeseries|Video restart time time-series|
|video_restart<br/>_time_distribution|conviva.<br/>video_restart_time<br/>_distribution|Video restart time distribution label-series|
|video_start<br/>_failures|conviva.<br/>video_start<br/>_failures|Video start failures time-series|
|video_start<br/>_failures_errornames|conviva.<br/>video_start<br/>_failures_errornames|Video start failures by error names simple-table|
|video_startup_time|conviva.<br/>video_startup_time|Video startup time label-series|
|quality_metriclens|conviva.<br/>quality_metriclens.<br/>total_attempts|Attempts|
||conviva.<br/>quality_metriclens.<br/>video_start<br/>_failures_percent|Video Start Failures(VSF) (%)|
||conviva.<br/>quality_metriclens.<br/>exits_before<br/>_video_start<br/>_percent|Exits Before Video Starts (EBVS) (%)|
||conviva.<br/>quality_metriclens.<br/>plays_percent|Plays (%)|
||conviva.<br/>quality_metriclens.<br/>video_startup<br/>_time_sec|Video Startup Time (sec)|
||conviva.<br/>quality_metriclens.<br/>rebuffering_ratio<br/>_percent|Rebuffering Ratio (%)|
||conviva.<br/>quality_metriclens.<br/>average_bitrate<br/>_kbps|Average Bitrate (bps). This metric can be returned in kbps with the ab_units=kbps parameter. Unless this parameter is specified, average bitrate is bps.|
||conviva.<br/>quality_metriclens.<br/>video_playback<br/>_failures_percent|Video Playback Failures (%)|
||conviva.<br/>quality_metriclens.<br/>ended_plays|Ended Plays|
||conviva.<br/>quality_metriclens.<br/>connection_induced<br/>_rebuffering_ratio<br/>_percent|Connection Induced ReBuffering Ratio (%)|
||conviva.<br/>quality_metriclens.<br/>video_restart_time|Video Restart Time|
|audience_metriclens|conviva.<br/>audience_metriclens.<br/>concurrent_plays|Concurrent Plays|
||conviva.<br/>audience_metriclens.<br/>plays|Plays|
||conviva.<br/>audience_metriclens.<br/>ended_plays|Ended Plays|


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: conviva
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `pulseUsername` | **yes** | `string` | Conviva Pulse username required with each API request. |
| `pulsePassword` | **yes** | `string` | Conviva Pulse password required with each API request. |
| `timeoutSeconds` | no | `integer` |  (**default:** `10`) |
| `metricConfigs` | no | `list of objects (see below)` | Conviva metrics to fetch. The default is quality_metriclens metric with the "All Traffic" filter applied and all quality_metriclens dimensions. |


The **nested** `metricConfigs` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `account` | no | `string` | Conviva customer account name. The default account is fetched used if not specified. |
| `metricParameter` | no | `string` |  (**default:** `quality_metriclens`) |
| `filters` | no | `list of strings` | Filter names. The default is `All Traffic` filter |
| `metricLensDimensions` | no | `list of strings` | MetricLens dimension names. The default is names of all MetricLens dimensions of the account |
| `excludeMetricLensDimensions` | no | `list of strings` | MetricLens dimension names to exclude. |
| `maxFiltersPerRequest` | no | `integer` | Max number of filters per request. The default is the number of filters. Multiple requests are made if the number of filters is more than maxFiltersPerRequest (**default:** `0`) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.splunk.com/observability/admin/subscription-usage/monitor-imm-billing-usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - ***`conviva.video_startup_time`*** (*gauge*)<br>    Video startup time label-series

#### Group attempts
All of the following metrics are part of the `attempts` metric group. All of
the non-default metrics below can be turned on by adding `attempts` to the
monitor config option `extraGroups`:
 - ***`conviva.attempts`*** (*gauge*)<br>    Attempts time-series

#### Group audience_metriclens
All of the following metrics are part of the `audience_metriclens` metric group. All of
the non-default metrics below can be turned on by adding `audience_metriclens` to the
monitor config option `extraGroups`:
 - ***`conviva.audience_metriclens.concurrent_plays`*** (*gauge*)<br>    Concurrent Plays
 - ***`conviva.audience_metriclens.ended_plays`*** (*gauge*)<br>    Ended Plays
 - ***`conviva.audience_metriclens.plays`*** (*gauge*)<br>    Plays

#### Group avg_bitrate
All of the following metrics are part of the `avg_bitrate` metric group. All of
the non-default metrics below can be turned on by adding `avg_bitrate` to the
monitor config option `extraGroups`:
 - ***`conviva.avg_bitrate`*** (*gauge*)<br>    Average bitrate time-series

#### Group concurrent_plays
All of the following metrics are part of the `concurrent_plays` metric group. All of
the non-default metrics below can be turned on by adding `concurrent_plays` to the
monitor config option `extraGroups`:
 - ***`conviva.concurrent_plays`*** (*gauge*)<br>    Concurrent plays time-series

#### Group connection_induced_rebuffering_ratio
All of the following metrics are part of the `connection_induced_rebuffering_ratio` metric group. All of
the non-default metrics below can be turned on by adding `connection_induced_rebuffering_ratio` to the
monitor config option `extraGroups`:
 - ***`conviva.connection_induced_rebuffering_ratio`*** (*gauge*)<br>    Connection induced rebuffering ratio simple-series

#### Group connection_induced_rebuffering_ratio_timeseries
All of the following metrics are part of the `connection_induced_rebuffering_ratio_timeseries` metric group. All of
the non-default metrics below can be turned on by adding `connection_induced_rebuffering_ratio_timeseries` to the
monitor config option `extraGroups`:
 - ***`conviva.connection_induced_rebuffering_ratio_timeseries`*** (*gauge*)<br>    Connection induced rebuffering ratio time-series

#### Group duration_connection_induced_rebuffering_ratio_distribution
All of the following metrics are part of the `duration_connection_induced_rebuffering_ratio_distribution` metric group. All of
the non-default metrics below can be turned on by adding `duration_connection_induced_rebuffering_ratio_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.duration_connection_induced_rebuffering_ratio_distribution`*** (*gauge*)<br>    Duration vs. connection induced rebuffering ratio distribution label-series

#### Group ended_plays
All of the following metrics are part of the `ended_plays` metric group. All of
the non-default metrics below can be turned on by adding `ended_plays` to the
monitor config option `extraGroups`:
 - ***`conviva.ended_plays`*** (*gauge*)<br>    Ended plays simple-series

#### Group ended_plays_timeseries
All of the following metrics are part of the `ended_plays_timeseries` metric group. All of
the non-default metrics below can be turned on by adding `ended_plays_timeseries` to the
monitor config option `extraGroups`:
 - ***`conviva.ended_plays_timeseries`*** (*gauge*)<br>    Ended plays time-series

#### Group exits_before_video_start
All of the following metrics are part of the `exits_before_video_start` metric group. All of
the non-default metrics below can be turned on by adding `exits_before_video_start` to the
monitor config option `extraGroups`:
 - ***`conviva.exits_before_video_start`*** (*gauge*)<br>    Exits before video start time-series

#### Group play_bitrate_distribution
All of the following metrics are part of the `play_bitrate_distribution` metric group. All of
the non-default metrics below can be turned on by adding `play_bitrate_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.play_bitrate_distribution`*** (*gauge*)<br>    Play bitrate distribution label-series

#### Group play_buffering_ratio_distribution
All of the following metrics are part of the `play_buffering_ratio_distribution` metric group. All of
the non-default metrics below can be turned on by adding `play_buffering_ratio_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.play_buffering_ratio_distribution`*** (*gauge*)<br>    Play buffering ratio distribution label-series

#### Group play_connection_induced_rebuffering_ratio_distribution
All of the following metrics are part of the `play_connection_induced_rebuffering_ratio_distribution` metric group. All of
the non-default metrics below can be turned on by adding `play_connection_induced_rebuffering_ratio_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.play_connection_induced_rebuffering_ratio_distribution`*** (*gauge*)<br>    Play connection induced rebuffering ratio distribution label-series

#### Group plays
All of the following metrics are part of the `plays` metric group. All of
the non-default metrics below can be turned on by adding `plays` to the
monitor config option `extraGroups`:
 - ***`conviva.plays`*** (*gauge*)<br>    Plays time-series

#### Group quality_metriclens
All of the following metrics are part of the `quality_metriclens` metric group. All of
the non-default metrics below can be turned on by adding `quality_metriclens` to the
monitor config option `extraGroups`:
 - ***`conviva.quality_metriclens.average_bitrate_kbps`*** (*gauge*)<br>    Average Bitrate (bps). This metric can be returned in kbps with the ab_units=kbps parameter. Unless this parameter is specified, average bitrate is bps
 - ***`conviva.quality_metriclens.connection_induced_rebuffering_ratio_percent`*** (*gauge*)<br>    Connection Induced ReBuffering Ratio (%)
 - ***`conviva.quality_metriclens.ended_plays`*** (*gauge*)<br>    Ended Plays
 - ***`conviva.quality_metriclens.exits_before_video_start_percent`*** (*gauge*)<br>    Exits Before Video Starts (EBVS) (%)
 - ***`conviva.quality_metriclens.plays_percent`*** (*gauge*)<br>    Plays (%)
 - ***`conviva.quality_metriclens.rebuffering_ratio_percent`*** (*gauge*)<br>    Rebuffering Ratio (%)
 - ***`conviva.quality_metriclens.total_attempts`*** (*gauge*)<br>    Attempts
 - ***`conviva.quality_metriclens.video_playback_failures_percent`*** (*gauge*)<br>    Video Playback Failures (%)
 - ***`conviva.quality_metriclens.video_restart_time`*** (*gauge*)<br>    Video Restart Time
 - ***`conviva.quality_metriclens.video_start_failures_percent`*** (*gauge*)<br>    Video Start Failures(VSF) (%)
 - ***`conviva.quality_metriclens.video_startup_time_sec`*** (*gauge*)<br>    Video Startup Time (sec)

#### Group quality_summary
All of the following metrics are part of the `quality_summary` metric group. All of
the non-default metrics below can be turned on by adding `quality_summary` to the
monitor config option `extraGroups`:
 - ***`conviva.quality_summary`*** (*gauge*)<br>    Quality summary label-series

#### Group rebuffered_plays
All of the following metrics are part of the `rebuffered_plays` metric group. All of
the non-default metrics below can be turned on by adding `rebuffered_plays` to the
monitor config option `extraGroups`:
 - ***`conviva.rebuffered_plays`*** (*gauge*)<br>    Rebuffered plays time-series

#### Group rebuffering_ratio
All of the following metrics are part of the `rebuffering_ratio` metric group. All of
the non-default metrics below can be turned on by adding `rebuffering_ratio` to the
monitor config option `extraGroups`:
 - ***`conviva.rebuffering_ratio`*** (*gauge*)<br>    Rebuffering ratio time-series

#### Group top_assets_15_mins
All of the following metrics are part of the `top_assets_15_mins` metric group. All of
the non-default metrics below can be turned on by adding `top_assets_15_mins` to the
monitor config option `extraGroups`:
 - ***`conviva.top_assets_15_mins`*** (*gauge*)<br>    Top assets over last 15 minutes simple-table

#### Group top_assets_summary
All of the following metrics are part of the `top_assets_summary` metric group. All of
the non-default metrics below can be turned on by adding `top_assets_summary` to the
monitor config option `extraGroups`:
 - ***`conviva.top_assets_summary`*** (*gauge*)<br>    Top assets summary label-series

#### Group video_playback_failures
All of the following metrics are part of the `video_playback_failures` metric group. All of
the non-default metrics below can be turned on by adding `video_playback_failures` to the
monitor config option `extraGroups`:
 - ***`conviva.video_playback_failures`*** (*gauge*)<br>    Video playback failures simple-series

#### Group video_playback_failures_distribution
All of the following metrics are part of the `video_playback_failures_distribution` metric group. All of
the non-default metrics below can be turned on by adding `video_playback_failures_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.video_playback_failures_distribution`*** (*gauge*)<br>    Video playback failures distribution label-series

#### Group video_playback_failures_timeseries
All of the following metrics are part of the `video_playback_failures_timeseries` metric group. All of
the non-default metrics below can be turned on by adding `video_playback_failures_timeseries` to the
monitor config option `extraGroups`:
 - ***`conviva.video_playback_failures_timeseries`*** (*gauge*)<br>    Video playback failures time-series

#### Group video_restart_time
All of the following metrics are part of the `video_restart_time` metric group. All of
the non-default metrics below can be turned on by adding `video_restart_time` to the
monitor config option `extraGroups`:
 - ***`conviva.video_restart_time`*** (*gauge*)<br>    Video restart time simple-series

#### Group video_restart_time_distribution
All of the following metrics are part of the `video_restart_time_distribution` metric group. All of
the non-default metrics below can be turned on by adding `video_restart_time_distribution` to the
monitor config option `extraGroups`:
 - ***`conviva.video_restart_time_distribution`*** (*gauge*)<br>    Video restart time distribution label-series

#### Group video_restart_time_timeseries
All of the following metrics are part of the `video_restart_time_timeseries` metric group. All of
the non-default metrics below can be turned on by adding `video_restart_time_timeseries` to the
monitor config option `extraGroups`:
 - ***`conviva.video_restart_time_timeseries`*** (*gauge*)<br>    Video restart time time-series

#### Group video_start_failures
All of the following metrics are part of the `video_start_failures` metric group. All of
the non-default metrics below can be turned on by adding `video_start_failures` to the
monitor config option `extraGroups`:
 - ***`conviva.video_start_failures`*** (*gauge*)<br>    Video start failures time-series

#### Group video_start_failures_errornames
All of the following metrics are part of the `video_start_failures_errornames` metric group. All of
the non-default metrics below can be turned on by adding `video_start_failures_errornames` to the
monitor config option `extraGroups`:
 - ***`conviva.video_start_failures_errornames`*** (*gauge*)<br>    Video start failures by error names simple-table

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.



