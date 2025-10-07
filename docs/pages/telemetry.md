---
layout: default
title: Telemetry
nav_order: 7
permalink: /telemetry
---

# Telemetry Utilities

Set up metrics collection from your deployed applications.

## Overview

Telemetry allows you to collect and monitor metrics from your deployed containers. All telemetry commands are available under:

```bash
oasees-sdk telemetry
```

## Metric Index Concept

A **metric index** groups metrics together so all devices can access each other's metrics.

- You can name the index whatever you want
- All collectors should use the same metric index
- The index is created automatically when you deploy your first collector

## Deploying Collectors

### Prerequisites

Check your application's deployment to find the metrics endpoints:

```bash
oasees-sdk get-app
```

Example output:

```
=====================================================================
OASEES APP DEPLOYMENT SUMMARY
=====================================================================
Service: dashboard        | Port: 31580 | Node: intel-nuc
Service: flow-sensor      | Port: 32691 | Node: device4
Service: pressure-sensor  | Port: 32690 | Node: device3
Service: thermal-sensor   | Port: 32688 | Node: device2
Service: vibration-sensor | Port: 32687 | Node: device1
=====================================================================
```

### Deploy Collector for Each Endpoint

Deploy a collector for each sensor, using the same metric index:

```bash
# For device4 (flow-sensor)
oasees-sdk telemetry deploy-collector -i swarm_metrics -s device4 \
  -se http://192.168.88.239:32691/metrics

# For device3 (pressure-sensor)
oasees-sdk telemetry deploy-collector -i swarm_metrics -s device3 \
  -se http://192.168.88.239:32690/metrics

# For device2 (thermal-sensor)
oasees-sdk telemetry deploy-collector -i swarm_metrics -s device2 \
  -se http://192.168.88.239:32688/metrics

# For device1 (vibration-sensor)
oasees-sdk telemetry deploy-collector -i swarm_metrics -s device1 \
  -se http://192.168.88.239:32687/metrics
```

Parameters:
- `-i`: Metric index name (e.g., `swarm_metrics`)
- `-s`: Source device name
- `-se`: Sensor endpoint URL

{: .note }
> The metric index name (`swarm_metrics` in this example) can be any name you choose. All collectors should use the same index.

## Verifying Collectors

### Check Metric Indices

List all metric indices:

```bash
oasees-sdk telemetry metric-index
```

### List Metrics by Index

View all metrics being collected for a specific index:

```bash
oasees-sdk telemetry metrics-list -i swarm_metrics
```

Example output:

```
swarm_metrics
flow_value [ device4 ]
forced_metrics [ device3 device4 device2 device1 ]
messages_received [ device3 device4 device2 device1 ]
pressure_value [ device3 ]
readings_count [ device3 device4 device2 device1 ]
thermal_value [ device2 ]
vibration_value [ device1 ]
```

This shows which devices are providing which metrics.

## Managing Collectors

### Delete a Collector

If a collector isn't operating correctly, you can delete it:

```bash
kubectl delete pod <collector-pod> --force
```

Find the collector pod name with:

```bash
kubectl get pods | grep collector
```

## Troubleshooting

### Collector Not Collecting Metrics

- Verify the sensor endpoint is accessible
- Check collector pod logs: `kubectl logs <collector-pod>`
- Ensure the metric index name is correct
- Verify the sensor is exposing metrics on the expected port

### Metrics Not Appearing

- Check that metrics endpoints return numeric values
- Avoid changing alphanumeric strings (like timestamps with date)
- Ensure sensor containers are running: `kubectl get pods`

## Best Practices

1. **Use Consistent Naming**: Use the same metric index name across all collectors
2. **Document Endpoints**: Keep track of which endpoints belong to which services
3. **Monitor Collector Health**: Periodically check collector pods are running
4. **Numeric Metrics Only**: Ensure your metrics endpoints return int/float values

## Related Video

- [4.deploy-collectors.mp4](https://nocncsrd.sharepoint.com/:v:/r/sites/OASEES2/Shared%20Documents/WP5/OASEES%20STACK%20%26%20SDK%20GUIDE/5.deploy-collectors.mp4?csf=1&web=1&e=P1e95v)

## Next Steps

With telemetry collecting metrics, proceed to [DAO Operations](dao-operations) to create and manage DAOs.
