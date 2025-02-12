
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# vmem

Monitor Type: `vmem` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/vmem))

**Accepts Endpoints**: No

**Multiple Instances Allowed**: **No**

## Overview

Collects information specific to the virtual memory subsystem of the
kernel.  For general memory statistics, see the [memory monitor](./memory.md).

On Linux hosts, this monitor relies on the `/proc` filesystem.
If the underlying host's `/proc` file system is mounted somewhere other than
/proc please specify the path using the top level configuration `procPath`.

```yaml
procPath: /proc
monitors:
 - type: vmem
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: vmem
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `counterRefreshInterval` | no | `int64` | (Windows Only) The frequency that wildcards in counter paths should be expanded and how often to refresh counters from configuration. This is expressed as a duration. (**default:** `60s`) |
| `printValid` | no | `bool` | (Windows Only) Print out the configurations that match available performance counters.  This used for debugging. (**default:** `false`) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.splunk.com/observability/admin/subscription-usage/monitor-imm-billing-usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - `vmpage.swap.in_per_second` (*gauge*)<br>    (Windows Only)
 - `vmpage.swap.out_per_second` (*gauge*)<br>    (Windows Only)
 - `vmpage.swap.total.per_second` (*gauge*)<br>    (Windows Only)
 - `vmpage_faults.majflt` (*cumulative*)<br>    (Linux Only) Number of major page faults on the system
 - `vmpage_faults.minflt` (*cumulative*)<br>    (Linux Only) Number of minor page faults on the system
 - `vmpage_io.memory.in` (*cumulative*)<br>    (Linux Only) Page Ins for Memory
 - `vmpage_io.memory.out` (*cumulative*)<br>    (Linux Only) Page Outs for Memory
 - ***`vmpage_io.swap.in`*** (*cumulative*)<br>    (Linux Only) Page Ins for Swap
 - ***`vmpage_io.swap.out`*** (*cumulative*)<br>    (Linux Only) Page Outs for Swap
 - `vmpage_number.free_pages` (*gauge*)<br>    (Linux Only) Number of free memory pages
 - `vmpage_number.mapped` (*gauge*)<br>    (Linux Only) Number of mapped pages
 - `vmpage_number.shmem_pmdmapped` (*gauge*)<br>    (Linux Only) The amount of shared (shmem/tmpfs) memory backed by huge pages

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.



