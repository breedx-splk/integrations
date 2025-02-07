
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/observer-page.md.tmpl --->

# k8s-api

 Discovers pod endpoints and nodes running in a Kubernetes
cluster by querying the Kubernetes API server.  This observer by default
will only discover pod endpoints exposed on the same node that the agent is
running, so that the monitoring of services does not generate cross-node
traffic.  To know which node the agent is running on, you should set an
environment variable called `MY_NODE_NAME` using the downward API
`spec.nodeName` value in the pod spec. Our provided K8s DaemonSet resource
does this already and provides an example.

This observer also emits high-level `pod` targets that only contain a `host`
field but no `port`.  This allows monitoring ports on a pod that are not
explicitly specified in the pod spec, which would result in no normal
`hostport` target being emitted for that particular endpoint.

If `discoverAllPods` is set to `true`, then the observer will discover pods on all
nodes in the cluster (or namespace if specified).  By default, only pods on
the same node as the agent are discovered.

Note that this observer discovers exposed ports on pod containers, not K8s
Endpoint resources, so don't let the terminology of agent "endpoints"
confuse you.


Observer Type: `k8s-api`

[Observer Source Code](https://github.com/signalfx/signalfx-agent/tree/main/pkg/observers/kubernetes)

## Configuration

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `namespace` | no | `string` | If specified, only pods within the given namespace on the same node as the agent will be discovered. If blank, all pods on the same node as the agent will be discovered. |
| `kubernetesAPI` | no | `object (see below)` | Configuration for the K8s API client |
| `additionalPortAnnotations` | no | `list of strings` | A list of annotation names that should be used to infer additional ports to be discovered on a particular pod.  The pod's annotation value should be a port number.  This is useful for annotations like `prometheus.io/port: 9230`. |
| `discoverAllPods` | no | `bool` | If true, this observer will watch all Kubernetes pods and discover endpoints/services from each of them.  The default behavior (when `false`) is to only watch the pods on the current node that this agent is running on (it knows the current node via the `MY_NODE_NAME` envvar provided by the downward API). (**default:** `false`) |
| `discoverNodes` | no | `bool` | If `true`, the observer will discover nodes as a special type of endpoint.  You can match these endpoints in your discovery rules with the condition `target == "k8s-node"`. (**default:** `false`) |


The **nested** `kubernetesAPI` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `authType` | no | `string` | How to authenticate to the K8s API server.  This can be one of `none` (for no auth), `tls` (to use manually specified TLS client certs, not recommended), `serviceAccount` (to use the standard service account token provided to the agent pod), or `kubeConfig` to use credentials from `~/.kube/config`. (**default:** `serviceAccount`) |
| `skipVerify` | no | `bool` | Whether to skip verifying the TLS cert from the API server.  Almost never needed. (**default:** `false`) |
| `clientCertPath` | no | `string` | The path to the TLS client cert on the pod's filesystem, if using `tls` auth. |
| `clientKeyPath` | no | `string` | The path to the TLS client key on the pod's filesystem, if using `tls` auth. |
| `caCertPath` | no | `string` | Path to a CA certificate to use when verifying the API server's TLS cert.  Generally this is provided by K8s alongside the service account token, which will be picked up automatically, so this should rarely be necessary to specify. |




## Target Variables

The following fields are available on targets generated by this observer and
can be used in discovery rules.

| Name | Type | Description |
| ---  | ---  | ---         |
| `container_name` | `string` | The first and primary name of the container as it is known to the container runtime (e.g. Docker). |
| `has_port` | `string` | Set to `true` if the endpoint has a port assigned to it.  This will be `false` for endpoints that represent a host/container as a whole. |
| `ip_address` | `string` | The IP address of the endpoint if the `host` is in the from of an IPv4 address |
| `kubernetes_annotations` | `map of strings` | The set of annotations on the discovered pod or node. |
| `network_port` | `string` | An alias for `port` |
| `node_addresses` | `map of strings` | A map of the different Node addresses specified in the Node status object.  The key of the map is the address type and the value is the address string. The address types are `Hostname`, `ExternalIP`, `InternalIP`, `ExternalDNS`, `InternalDNS`.  Most likely not all of these address types will be present for a given Node. |
| `node_metadata` | `node_metadata` | The metadata about the Node, for `k8s-node` targets, with fields in TitleCase.  See [ObjectMeta v1 meta reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#objectmeta-v1-meta). |
| `node_spec` | `node_spec` | The Node spec object, for `k8s-node` targets.  See [the K8s reference on this resource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#nodespec-v1-core), but keep in the mind that fields will be in TitleCase due to passing through Go. |
| `node_status` | `node_status` | The Node status object, for `k8s-node` targets. See [the K8s reference on Node Status](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#nodestatus-v1-core) but keep in mind that fields will be in TitleCase due to passing through Go. |
| `pod_metadata` | `pod metadata` | The full pod metadata object, as represented by the Go K8s client library (client-go): https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#ObjectMeta. |
| `pod_spec` | `pod spec` | The full pod spec object, as represented by the Go K8s client library (client-go): https://godoc.org/k8s.io/api/core/v1#PodSpec. |
| `private_port` | `string` | The port that the service endpoint runs on inside the container |
| `public_port` | `string` | The port exposed outside the container |
| `alternate_port` | `integer` | Used for services that are accessed through some kind of NAT redirection as Docker does.  This could be either the public port or the private one. |
| `container_command` | `string` | The command used when running the container exposing the endpoint |
| `container_id` | `string` | The ID of the container exposing the endpoint |
| `container_image` | `string` | The image name of the container exposing the endpoint |
| `container_labels` | `map of string` | A map that contains container label key/value pairs. You can use the `Contains` and `Get` helper functions in discovery rules to make use of this. See [Endpoint Discovery](../auto-discovery.html#additional-functions). For containers managed by Kubernetes, this will be set to the pod's labels, as individual containers do not have labels in Kubernetes proper. |
| `container_names` | `list of string` | A list of container names of the container exposing the endpoint |
| `container_state` | `string` | The container state, will usually be "running" since otherwise the container wouldn't have a port exposed to be discovered. |
| `discovered_by` | `string` | The observer that discovered this endpoint |
| `host` | `string` | The hostname/IP address of the endpoint.  If this is an IPv6 address, it will be surrounded by `[` and `]`. |
| `id` | `string` |  |
| `name` | `string` | A observer assigned name of the endpoint. For example, if using the `k8s-api` observer, `name` will be the port name in the pod spec, if any. |
| `orchestrator` | `integer` |  |
| `port` | `integer` | The TCP/UDP port number of the endpoint |
| `port_labels` | `map of string` | A map of labels on the container port |
| `port_type` | `string` | TCP or UDP |
| `target` | `string` | The type of the thing that this endpoint directly refers to.  If the endpoint has a host and port associated with it (most common), the value will be `hostport`.  Other possible values are: `pod`, `container`, `host`.  See the docs for the specific observer you are using for more details on what types that observer emits. |

## Dimensions

These dimensions are added to all metrics that are emitted for this discovery
target.  These variables are also available to use as variables in discovery
rules.

| Name | Description |
| ---  | ---         |
| `container_id` | The container id of the container running this endpoint. |
| `container_image` | The image name (including tags) of the running container |
| `container_name` | The primary name of the running container -- Docker containers can have multiple names but this will be the first name, if any. |
| `container_spec_name` | The short name of the container in the pod spec, **NOT** the running container's name in the Docker engine |
| `kubernetes_namespace` | The namespace that the discovered service endpoint is running in. |
| `kubernetes_node` | For Node (`k8s-node`) targets, the name of the Node |
| `kubernetes_node_uid` | For Node (`k8s-node`) targets, the UID of the Node |
| `kubernetes_pod_name` | The name of the running pod that is exposing the discovered endpoint |
| `kubernetes_pod_uid` | The UID of the pod that is exposing the discovered endpoint |


