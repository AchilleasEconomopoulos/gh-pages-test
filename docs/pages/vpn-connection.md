---
layout: default
title: VPN Connection
parent: Prerequisites
nav_order: 1
permalink: /prerequisites/vpn-connection
---

# VPN Connection Guidelines

All devices must be connected to the VPN to access the OASEES infrastructure.

## Requirements

{: .important }
> VPN must run on **ALL** devices, including the machine with the browser used to access the portal.

## Connecting to VPN

Run OpenVPN with your configuration file in daemon mode:

```bash
sudo openvpn --config path_to_ovpn_file --daemon
```

This will run the VPN connection in the background.

## Verify Connection

Confirm network connection with NCSRD premises:

```bash
ping <BLOCKCHAIN_IP>
```

You should receive responses if the connection is successful.

## Troubleshooting

If you cannot reach `<BLOCKCHAIN_IP>`:

1. Check that the VPN daemon is running
2. Verify your `.ovpn` configuration file path is correct
3. Check firewall settings
4. Ensure you have proper VPN credentials

## Next Steps

[Back to Prerequisites](prerequisites) | [Continue to Metamask Configuration](metamask)
