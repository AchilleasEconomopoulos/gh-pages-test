---
layout: default
title: DAO Workflow
nav_order: 9
permalink: /dao-workflow
---

# DAO End-to-End Sample Flow

Configure devices to automatically propose and vote based on collected metrics.

## Overview

The "OASEES agent" (collector + blockchain agent) can:
- **Propose** actions based on metric conditions
- **Vote** on proposals based on metric thresholds
- **Execute** actions when proposals pass

## Configuration File

### Generate Template

Create a configuration template:

```bash
oasees-sdk telemetry gen-config
```

This generates `config.json` with the following structure:

```json
{
    "metric_index": " ",
    "propose_on": {
        "events": [" "],
        "proposal_contents": [" "],
        "positive_vote_on": [" "]
    },
    "actions_map": {
        " ": {
            "action_endpoint": "",
            "args": {}
        }
    },
    "leader": ""
}
```

### Configuration Fields

| Field | Description |
|-------|-------------|
| `metric_index` | Name of the metrics index (e.g., `swarm_metrics`) |
| `propose_on.events` | Conditions that trigger a proposal |
| `propose_on.proposal_contents` | Message and action value for the proposal |
| `propose_on.positive_vote_on` | Conditions for voting YES |
| `actions_map` | Maps DAO action values to actual API calls |
| `leader` (**Optional**) | Pick a single specific device to execute the API call |

{: .highlight }
> The three `propose_on` subfields are **lists with one-to-one binding**. They must have equal numbers of elements. The 2nd event binds to the 2nd proposal_content and 2nd positive_vote_on condition.

## Example Configuration

```json
{
    "metric_index": "swarm_metrics",
    "propose_on": {
        "events": [
            "thermal_value > 99",
            "pressure_value > 60"
        ],
        "proposal_contents": [
            {
                "msg": "High Temperature, cool-down needed",
                "action_value": 1
            },
            {
                "msg": "High Pressure, emergency",
                "action_value": 2
            }
        ],
        "positive_vote_on": [
            "flow_value > 7",
            "thermal_value < 50"
        ]
    },
    "actions_map": {
        "1": {
            "action_endpoint": "http://localhost:5000/send_message",
            "args": {
                "target_sensors": [
                    "thermal-sensor",
                    "pressure-sensor",
                    "flow-sensor"
                ],
                "action": "cool_down",
                "duration": 10
            }
        },
        "2": {
            "action_endpoint": "http://localhost:5000/send_message",
            "args": {
                "target_sensors": [
                    "thermal-sensor",
                    "pressure-sensor",
                    "flow-sensor"
                ],
                "action": "emergency",
                "duration": 15
            }
        }
    },
    "leader": "device_name"
}
```

## Configuration Interpretation

### Scenario 1: High Temperature

**Trigger**: Device detects `thermal_value > 99`

1. **Proposes**: "High Temperature, cool-down needed" with `action_value: 1`
2. **Voting**: Devices with `flow_value > 7` vote YES, others vote NO
3. **If Passed**: Execute action mapped to `"1"`:
   - POST to `http://localhost:5000/send_message`
   - With args: `action: "cool_down"`, `duration: 10`

### Scenario 2: High Pressure

**Trigger**: Device detects `pressure_value > 60`

1. **Proposes**: "High Pressure, emergency" with `action_value: 2`
2. **Voting**: Devices with `thermal_value < 50` vote YES, others vote NO
3. **If Passed**: Execute action mapped to `"2"`:
   - POST to `http://localhost:5000/send_message`
   - With args: `action: "emergency"`, `duration: 15`

## Deploying Configuration

### Apply Configuration to Agents

```bash
oasees-sdk telemetry configure-agents config.json
```

This command:
1. Reads your configuration
2. Deploys it to all blockchain agents
3. Agents begin monitoring metrics and responding accordingly

## Action Endpoints

{: .warning }
> Action endpoints must be **POST** endpoints that accept JSON payloads.

### Endpoint Requirements

- **Use-case specific**: Implement logic for your application
- **Accessible**: Must be reachable from Kubernetes pods
- **POST method**: Must accept POST requests with JSON body
- **Localhost allowed**: Endpoints can use `localhost` since agents run in Kubernetes

### Example Endpoint Structure

```python
@app.route('/send_message', methods=['POST'])
def send_message():
    data = request.get_json()
    target_sensors = data.get('target_sensors', [])
    action = data.get('action', 'stabilize')
    duration = data.get('duration', 30)

    # Implement your action logic here
    # ...

    return jsonify({'status': 'success'})
```

## Testing the Workflow

### Force Metrics for Testing

For the sample application, use the provided utility script:

```python
import requests

# Force metrics to trigger DAO flow
response = requests.post(
    'http://localhost:5000/force_metrics',
    json={
        'value': 42.5,
        'duration': 60
    }
)

print(response.json())
```

{: .note }
> Change `localhost:5000` to your actual sensor endpoint.

### Monitoring the Flow

1. **Check Metrics**: View metrics in the portal or via:
   ```bash
   oasees-sdk telemetry metrics-list -i swarm_metrics
   ```

2. **Watch Proposals**: Monitor DAO proposals in the portal

3. **Verify Voting**: Check that devices are voting based on their metrics

4. **Confirm Actions**: Verify actions are executed when proposals pass

## Advanced Configuration

### Complex Proposal / Voting Logic

Combine multiple metrics in conditions:

- **Same device metrics**: Use `and`/`or` operators (`and` takes precedence)
- **Different device metrics**: Separate with comma (`,`)

#### Example

- **Setup**: The [sample application](sample-application) with 2 devices as participants.
    - Device1 is responsible for the flow sensor
    - Device2 is responsible for the thermal and pressure sensors.

- **Conditions**:
    - Device1 votes yes on: flow_value > 10
    - Device2 votes yes on: thermal_value > 80 && pressure_value < 20

- **Config syntax**:
```json
"positive_vote_on": [
    "flow_value > 10, thermal_value > 80 and pressure_value < 20",
]
```

{: .warning}
> If a device's metric isn't included in the voting conditions, the current default behavior is to vote AGAINST.

## Troubleshooting

### Proposals Not Being Created

- Verify collector is ingesting metrics
- Check metric values meet event conditions
- Ensure configuration was applied: `oasees-sdk telemetry configure-agents`
- Review agent logs

### Actions Not Executing

- Verify endpoint is accessible
- Check endpoint accepts POST with JSON
- Ensure action_map value matches DAO action value
- Review application logs

### Voting Not Working

- Verify devices have vote tokens
- Check voting conditions match actual metric values
- Ensure DAO quorum is achievable
- Review blockchain transactions

## Best Practices

1. **Test Incrementally**: Start with simple conditions and expand
2. **Monitor Logs**: Keep track of agent behavior
3. **Document Actions**: Clearly document what each action does
4. **Set Realistic Thresholds**: Base metric thresholds on actual data
5. **Plan for Failures**: Include error handling in action endpoints

## Related Video

- [6.configure-agents-DAO-flow.mp4](https://nocncsrd.sharepoint.com/:v:/r/sites/OASEES2/Shared%20Documents/WP5/OASEES%20STACK%20%26%20SDK%20GUIDE/7.configure-agents-DAO-flow.mp4?csf=1&web=1&e=vUj6QD)

## Next Steps

[Back to DAO Operations](dao-operations) | [Continue to MLops & Federated Learning](mlops-federated-learning)
