# SKube

## Table of Contents
1. [Overview](#overview)
2. [Objectives](#objectives)
   - [Current](#current-objectives)
   - [Out there](#out-there-objectives)
3. [Hardware](#hardware)
4. [Networking](#networking)
5. [Software and Tools](#software-and-tools)

## Overview
This organization's purpose is to serve as a place where I can add repositories for my personal home lab/side projects which I would like to deploy using my hardware. The central component of my infrastructure is a BOSGAME P4 Plus Mini PC found [here](https://www.amazon.ca/BOSGAME-P4-Plus-Computers-Desktop/dp/B0DBYB71GJ?pd_rd_w=s3y4O&content-id=amzn1.sym.1d3fa88f-aa61-4d59-895c-470dda2309ea&pf_rd_p=1d3fa88f-aa61-4d59-895c-470dda2309ea&pf_rd_r=76XDDHZHR6N9PVQZ9NBP&pd_rd_wg=k20Qc&pd_rd_r=ea8d74c6-a2b0-4a14-ba1b-4ae3d51365e4&ref_=sspa_dk_detail_1&sp_csd=d2lkZ2V0TmFtZT1zcF9kZXRhaWxfdGhlbWF0aWM&th=1). Leveraging this hardware I intend to explore and deepen my understanding of the skills I've learned over my education and professional career thus far. Specific things I want to learn are creating a full end-to-end [Github actions](https://github.com/features/actions) pipeline and an MLOps setup leveraging [Dagster](https://dagster.io/). I also intend to leverage the cluster to explore other open-source tools to make my life easier. The cluster is set up using [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/) and is running [Ubuntu Server Jammy Jellyfish](https://releases.ubuntu.com/jammy/).

## Objectives

### Current Objectives
- Stay Consistent With Documentation
- Set Up Kubernetes
- Install MetalLB for Load Balancing
- Set Up Metrics API
- Set Up Kubernetes Dashboard
- Set Up TailScale
- Set Up MiniIO
- Set Up a Self-Hosted Github Actions Runner
- Start Migrating Side Projects To Cluster
- Set Up Dagster Instance

### Out there objectives
- Set up Cloudflare Tunnels
- Train a Model
- Find a Way to Train the Model On My Personal Rig and Then Store it on Mini IO
- Automate a Dagster Pipeline Which Trains and Deploys an API Which Uses my Model (incorporate Actions pipelines for CI/CD)

## Hardware
### Kubernetes Cluster
The cluster is run on the BOSGAME P4 as mentioned above the reason I've chosen it is relatively simple, it's cheap. The cost per performance is very strong as it has a Ryzen 7, 32GB of DDR4 RAM (DDR5 would've been nice, but alas) and a 15-watt TDP which is nice. This gives me plenty of room to practise Kubernetes, adequate resources for running both data and DevOps pipelines, and deploying various services/projects

### Personal Rig
My current rig consists of an i7 13700k and RTX 4080. The justification for the hardware is gaming but it doubles as a powerful setup for training models and other data-intensive tasks, so I will be incorporating it into various data pipelines.

## Networking
Nothing will be port forwarded because A) I'm no security engineer and B) I don't want to be hacked so the optimal way to have these projects accessible will be to either keep them private (which will be most projects) and use [Cloudflare tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) (more on this later as I learn how to do this). To access the cluster when I'm away from home I'll be using [TailScale](https://tailscale.com/wireguard-vpn?utm_campaign=PMax-Wireguard-US&utm_medium=paid-search&utm_source=google&utm_content=:21886304591:::c:&gad_source=1&gad_campaignid=21890112415&gbraid=0AAAAACjm7b2K6A24PNlAVr0ZO956aRHvI&gclid=CjwKCAjw56DBBhAkEiwAaFsG-hsuYr8dgWz4sWA1ffMx4Xqs41eSfBCekbELypkUq2gBTxvUs3SMFxoCMKcQAvD_BwE) to protect the cluster. All services will be load balancers which use [MetalLB](https://metallb.io/) since I don't want to use node ports due to safety concerns (obviously unless it's to be used internally in which case I also may use cluster IP). 

## Software and Tools
| Software  | Version | Purpose |
| ------------- | ------------- | ------------- |
| [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)  | v1.33  | Kubernetes service  |
| [Containerd](https://containerd.io/)  | v2.1  | Container runtime for cluster  |
| [Flannel](https://github.com/flannel-io/flannel)  | v0.26.7  | CNI for cluster |
| [MetalLB](https://github.com/flannel-io/flannel)  | v0.14.9 | Load balancer for cluster|


