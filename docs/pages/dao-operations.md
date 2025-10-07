---
layout: default
title: DAO Operations
nav_order: 8
permalink: /dao-operations
---

# DAO Operations

Create and manage Decentralized Autonomous Organizations (DAOs) for your device network.

## Creating a DAO

### Using the Solidity IDE

![Solidity-IDE](/assets/solidity-ide.png)

From the OASEES portal, you can create and deploy a DAO using the integrated Solidity IDE:

1. Navigate to the **Solidity IDE** section in the portal
2. Follow the **DAO Creation Steps**:
   - **Governance**: Configure voting mechanisms
   - **Vote Token**: Set up voting tokens
   - **Time Lock**: Define time-based constraints
   - **Action**: Configure action parameters
3. Click **Deploy DAO** with your chosen parameters:
   - **DAO Name**: Choose a descriptive name
   - **DAO Description**: Document your DAO's purpose
   - **Minimum Delay (Blocks)**: Time before proposal execution
   - **Quorum Percentage**: Minimum participation required

### Example Configuration

```
DAO Name: Sensor DAO
Description: A decentralized load balancing DAO
Minimum Delay: 1 block
Quorum Percentage: 4%
Percentage of total supply needed for votes to pass: 50%
```

## Interacting with Your DAO

After deployment, interact with your DAO from the **Home** page of the portal.

![Home-DAO](/assets/home-dao.png)

### DAO Overview

You'll see:
- DAO name and member count
- Connected devices in the network
- Available actions
- Proposal history

## Managing Device Membership

### Adding Devices to the DAO

1. Navigate to your DAO in the portal
2. Click **Manage** to access the management interface
3. View **Available devices** section
4. For each device, assign **Vote Tokens**:
   - Click the token amount field
   - Enter the desired number of tokens
   - Click **Save**

![DAO Management Interface](../assets/dao-management.png)

{: .important }
> Vote tokens determine a device's voting power in the DAO. Distribute tokens based on your governance model.

### Vote Token Distribution

Example distribution:

| Device Name | Account | Tokens | Status |
|------------|---------|---------|---------|
| device1 | 0x4dAb...BcbF | 10 | Active |
| device2 | 0x8bB5...4d09 | 15 | Active |
| device3 | 0x9c5c3...Da88 | 20 | Active |
| device4 | 0x4de6...5756 | 25 | Active |
| intel-nuc | 0x7e8...704a | 30 | Active |

## Creating Proposals

Proposals can be created through:

1. **Portal Interface**: Manually create proposals through the UI
2. **Agent Configuration**: Automatic proposals based on metrics (see [DAO Workflow](dao-workflow))

### Manual Proposal Creation

1. Navigate to your DAO
2. Click **Create Proposal**
3. Fill in:
   - **Title**: Brief description
   - **Description**: Detailed explanation
   - **Action**: What should happen if approved
4. Submit the proposal

## Viewing Proposals

All proposals are visible in the DAO interface:

- **Active Proposals**: Currently open for voting
- **Passed Proposals**: Approved and executed
- **Failed Proposals**: Did not meet quorum or approval threshold
- **Pending Execution**: Approved but waiting for time lock

## Voting on Proposals

Devices with vote tokens can vote on proposals:

- Vote weight is proportional to token holdings
- Votes can be: **For**, **Against**, or **Abstain**
- Voting period is defined in DAO configuration

## DAO Configuration Parameters

### Quorum

Minimum percentage of tokens that must participate:

```
Quorum = (Total Votes Cast / Total Token Supply) × 100
```

### Approval Threshold

Percentage of participating votes needed for approval:

```
Approval = (For Votes / Total Votes Cast) × 100
```

### Time Lock

Delay between proposal approval and execution (in blocks):

- Allows time for review
- Provides security against malicious proposals
- Can be set to 0 for immediate execution

## Best Practices

1. **Token Distribution**:
   - Distribute tokens based on device importance or capacity
   - Consider equal distribution for democratic governance
   - Reserve tokens for future devices

2. **Quorum Settings**:
   - Higher quorum = more participation required
   - Lower quorum = faster decision making
   - Balance between security and efficiency

3. **Time Locks**:
   - Use longer delays for critical actions
   - Shorter delays for routine operations
   - Consider emergency override mechanisms

4. **Proposal Descriptions**:
   - Be clear and specific
   - Include expected outcomes
   - Document any risks

## Troubleshooting

### Proposal Not Executing

- Check if time lock period has passed
- Verify quorum was reached
- Ensure approval threshold was met
- Check blockchain transaction status

### Device Can't Vote

- Verify device has vote tokens
- Check device is DAO member
- Ensure Metamask is properly configured
- Verify blockchain connection

## Related Video

- [5.Dao-creation-device-membership.mp4](https://nocncsrd.sharepoint.com/:v:/r/sites/OASEES2/Shared%20Documents/WP5/OASEES%20STACK%20%26%20SDK%20GUIDE/6.Dao-creation-device-membership.mp4?csf=1&web=1&e=SXqElh)

## Next Steps

Learn how to configure automated agent behavior based on metrics in [DAO Workflow](dao-workflow).
