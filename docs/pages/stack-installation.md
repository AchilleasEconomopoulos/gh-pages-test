---
layout: default
title: Stack Installation
nav_order: 4
permalink: /stack-installation
---

# OASEES Stack Installation

Install the OASEES stack on your master node.

## Installation Steps

### 1. Install oasees-sdk

```bash
pip install oasees-sdk
```

#### Virtual Environment (Optional but Recommended)

In some cases, you might need to install the oasees-sdk in a virtual environment:

```bash
# Install venv package
sudo apt install python3.12-venv  # or your Python version

# Create virtual environment
python -m venv env

# Activate environment
source env/bin/activate

# Install SDK
pip install oasees-sdk
```

### 2. Provision the Stack

{: .important }
> Find your master node's IP address first. This is crucial for accessing the OASEES portal from a browser.

Initialize the OASEES stack:

```bash
oasees-sdk init --expose-ip <YOUR_MASTER_IP>
```

Replace `<YOUR_MASTER_IP>` with your actual master node IP address.

### 3. Wait for Deployment

All components of the stack may take approximately **5 minutes** to deploy.

Check the status with kubectl:

```bash
kubectl get pods -A
```

All pods should eventually show a `Running` status.

### 4. Access the Portal

Once the portal is up, access it at:

```
http://<YOUR_MASTER_IP>:30000
```

You can login with your blockchain account if you have configured Metamask correctly.

### 5. Get Join Token

Generate a token for worker nodes to join the cluster:

```bash
oasees-sdk get-token
```

Save this token - you'll need it to join devices in the next step.

## Verification

Your OASEES portal should be accessible and showing the login screen. You should see a deployment similar to this when running `kubectl get pods -A`:

```
NAMESPACE              NAME                                         READY   STATUS
cert-manager           cert-manager-5d74b46f7-xjqh9                1/1     Running
cert-manager           cert-manager-cainjector-34cb92f7b7-wdnhq    1/1     Running
cert-manager           cert-manager-webhook-554dff7db45-wwrjt      1/1     Running
default                cluster-backend-85c4bzdfc5f-tkcti          1/1     Running
default                grafana-5b4b7cb74d-vbqzm                    1/1     Running
default                mlops-data-notifier-dqlxg                   1/1     Running
default                oasees-data-reader-849fc9c-6hrv2            1/1     Running
default                oasees-device-plugin-m7znq                  1/1     Running
default                oasees-ipfs-59fff7d6sfa-8p5v6               1/1     Running
default                oasees-portal-56d68f93667-dzspg             1/1     Running
default                oasees-portal-7bdd5f9575-cdmdb              1/1     Running
default                oasees-solidty-ide-d979867b5-fsnb6          1/1     Running
default                oasees-telemetry-api-76db7cb678-qqsjd       1/1     Running
default                otlp-collector-collector-6bb5c4f66d-q275j   1/1     Running
default                prometheus-server-56b87f-fxmxl              1/1     Running
default                thanos-receive-0                            1/1     Running
kube-system            coredns-5bb8b6b7fde-mhzv9                   1/1     Running
kube-system            coredns-5bb8b6b7fde-x4vwr                   1/1     Running
```

## Troubleshooting

- If pods are stuck in `Pending`, check cluster resources
- If pods are in `CrashLoopBackOff`, check pod logs: `kubectl logs <pod-name>`
- Ensure VPN connection is active
- Verify sufficient disk space and memory

## Related Video

- [1.provision-stack-on-master.mp4](https://nocncsrd.sharepoint.com/:v:/r/sites/OASEES2/Shared%20Documents/WP5/OASEES%20STACK%20%26%20SDK%20GUIDE/1.provision-stack-on-master.mp4?csf=1&web=1&e=ckB4su)


## Next Steps

With the stack running and your join token ready, proceed to [Joining Devices](joining-devices).
