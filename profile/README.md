# SKube
A cheap, easy-to-set-up Kubernetes cluster set up for the sole purpose of learning how to make cool stuff.

## Table of Contents
1. [Overview](#overview)
2. [Objectives](#objectives)
   - [Completed](#completed-objectives)
   - [Current](#current-objectives)
   - [Out there](#out-there-objectives)
3. [Hardware](#hardware)
4. [Networking](#networking)
5. [CI/CD](#cicd)
6. [BoC Data Engineering Project](#boc-data-engineering-project)
7. [Software and Tools](#software-and-tools)

## Overview
This organization's purpose is to serve as a place where I can add repositories for my personal home lab/side projects, which I would like to deploy using my hardware. The central component of my infrastructure is a BOSGAME P4 Plus Mini PC found [here](https://www.amazon.ca/BOSGAME-P4-Plus-Computers-Desktop/dp/B0DBYB71GJ?pd_rd_w=s3y4O&content-id=amzn1.sym.1d3fa88f-aa61-4d59-895c-470dda2309ea&pf_rd_p=1d3fa88f-aa61-4d59-895c-470dda2309ea&pf_rd_r=76XDDHZHR6N9PVQZ9NBP&pd_rd_wg=k20Qc&pd_rd_r=ea8d74c6-a2b0-4a14-ba1b-4ae3d51365e4&ref_=sspa_dk_detail_1&sp_csd=d2lkZ2V0TmFtZT1zcF9kZXRhaWxfdGhlbWF0aWM&th=1). Leveraging this hardware I intend to explore and deepen my understanding of the skills I've learned over my education and professional career thus far. Specific things I want to learn are creating a full end-to-end [Github actions](https://github.com/features/actions) pipeline and an MLOps setup leveraging [Dagster](https://dagster.io/). I also intend to leverage the cluster to explore other open-source tools to make my life easier. The cluster is set up using [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/) and is running [Ubuntu Server Jammy Jellyfish](https://releases.ubuntu.com/jammy/). The BoC Classifier has been completed! The Dagster platform set up can be found [here](https://github.com/Sukhman-s-SKube/Dagster-Core), and the code location can be found [here](https://github.com/Sukhman-s-SKube/Dagster-Code-Location-1-BoC-Transformer). The data is trained on data stored on ClickHouse, which is my open-source alternative to Snowflake. The script can be found [here](https://github.com/Sukhman-s-SKube/BoC_Transformer_Worker). Finally, the model provides inference through a FastAPI server, which is found [here](https://github.com/Sukhman-s-SKube/boc-model-app). The next adventure I'm undertaking using this hardware is building a game which we will actually allow users to mess around with. 

## Objectives

### Completed Objectives
- Set Up Kubernetes
- Install MetalLB for Load Balancing
- Set Up Metrics API
- Set Up Kubernetes Dashboard
- Set up Bitnami sealed secrets
- Set Up TailScale
- Set Up a Self-Hosted GitHub Actions Runner
- Set up Harbor as a container registry to pull images for deployments
- Central cluster Postgres and MongoDB instances with PVCs (persistent volume claims)
- Deploy Hobb.Y on the cluster by pushing to harbor to test the feasibility of the pipeline
- Automate a CI/CD pipeline using GitHub Actions
- Set Up MiniO
- Start Migrating Side Projects To Cluster
- Set Up Dagster Instance
- Train a Model
- Find a Way to Train the Model on My Personal Rig and Then Store it on Minio
- Build an API which uses the model stored on Minio



### Current Objectives
- Stay Consistent With Documentation

### Out there objectives
- App has been completed!
- Make diagrams to explain processes better!


## Hardware
### Kubernetes Cluster
The cluster is run on the BOSGAME P4, as mentioned above, the reason I've chosen it is relatively simple, it's cheap. The cost per performance is very strong as it has a Ryzen 7, 32GB of DDR4 RAM (DDR5 would've been nice, but alas) and a 15-watt TDP, which is nice. This gives me plenty of room to practise Kubernetes, adequate resources for running both data and DevOps pipelines, and deploying various services/projects

### Personal Rig
My current rig consists of an i7 13700k and RTX 4080. The justification for the hardware is gaming, but it doubles as a powerful setup for training models and other data-intensive tasks, so I will be incorporating it into various data pipelines.

## Networking
Nothing will be port forwarded because A) I'm no security engineer and B) I don't want to be hacked so the optimal way to have these projects accessible will be to either keep them private (which will be most projects) and ~~use [Cloudflare tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) (more on this later as I learn how to do this)~~ Unfortunately after looking into cloudflare tunnels I've determined that it's just safer to not expose my projects to the internet and would probably be better off making demo videos of my projects and such instead. To access the cluster when I'm away from home I'll be using [TailScale](https://tailscale.com/wireguard-vpn?utm_campaign=PMax-Wireguard-US&utm_medium=paid-search&utm_source=google&utm_content=:21886304591:::c:&gad_source=1&gad_campaignid=21890112415&gbraid=0AAAAACjm7b2K6A24PNlAVr0ZO956aRHvI&gclid=CjwKCAjw56DBBhAkEiwAaFsG-hsuYr8dgWz4sWA1ffMx4Xqs41eSfBCekbELypkUq2gBTxvUs3SMFxoCMKcQAvD_BwE) to protect the cluster. All services will be load balancers which use [MetalLB](https://metallb.io/) since I don't want to use node ports due to safety concerns (obviously, unless it's to be used internally, in which case I also may use cluster IP). 

## CI/CD
I wanted to build a CI/CD pipeline which automated the process of development from end to end, so I leveraged GitHub Actions by using a self-hosted runner on my machine. I only have one runner because I'm the only person developing on the cluster, so from a needs perspective, this should be more than fine. I've built one reusable actions pipeline that all of my projects will be using within their own actions pipelines to build reusable pipelines that can be used in every repo going forward. The reusable pipeline can be found [Here](https://github.com/Sukhman-s-SKube/gh-actions).


## BoC data engineering project
The cluster was initially built around this project, the idea behind it being that we collect data daily using BoC Valet's API and FRED API and train weekly. The main goal I had was to enable multiple machines to work together to serve an API which could predict whether the Bank of Canada will cut, hold, or raise the rates. The data platform is built around Dagster's open source data orchestration platform, the core of which can be found [here](https://github.com/Sukhman-s-SKube/Dagster-Core). All of the code is written into a code location which can be found [here](https://github.com/Sukhman-s-SKube/Dagster-Code-Location-1-BoC-Transformer). The code location actually executes the pipelines, all of which are scheduled by the web UI and daemon, which were deployed using the Dagster Core mentioned earlier. The whole platform and almost everything deployed on the cluster have their own CI/CD pipeline, which can be found in the section above. The data flows as follows: at 8 pm daily, the Dagster Core schedules a job which is run on the code location. The job ingests CPI, Oil, Unemployment, and bond yield data and stores each asset on Minio and also adds it to my ClickHouse data warehouse. Once a week on Monday, the Dagster core schedules another job which pushes the task into a Redis queue, which is then read by an asynchronous worker. The worker then trains the model and stores it on MinIO, and then hits a route on the inference API to pull the latest model. The training script can be found [here](https://github.com/Sukhman-s-SKube/BoC_Transformer_Worker). The inference API can be found [here](https://github.com/Sukhman-s-SKube/boc-model-app). The reason why the asynchronous task queue architectural pattern was used for the training script is that I wanted to train the model using my gaming PC's GPU. We also used this structure of Dagster deployments because it enabled quick development, and only 1 job was scheduled at a time in this code location. If I was to account for more jobs running in the same code location at the same time, I would implement Kubernetes runlaunchers to have pods executing the code at their scheduled times. Some of the wins we managed to accomplish were cutting training times from around 5 minutes to under 1 minute by migrating the features from minio to a ClickHouse data warehouse. Another win we were able to accomplish was predicting the last rate cut using an ecosystem where I developed using my Mac, deployed and ran pipelines on my cluster, and trained on my gaming PC. The next decision, as of writing this, has been predicted to hold rates, so we will see!

## Software and Tools
| Software  | Version | Purpose/Comments |
| ------------- | ------------- | ------------- |
| [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)  | v1.33  | Kubernetes service  |
| [Kubectl](https://kubernetes.io/docs/reference/kubectl/)  | v1.29.15-1.1  | Kubernetes command line tool  |
| [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/#:~:text=The%20kubelet%20is%20the%20primary,object%20that%20describes%20a%20pod.) | v1.29.15-1.1  | Kubernetes node agent  |
| [Local path provisioner](https://github.com/rancher/local-path-provisioner)  | v0.0.31  | Used for provisioning PVCs for deployments |
| [Metrics Server](https://github.com/kubernetes-sigs/metrics-server)   | v0.7.2  | Scrapes pod and node metrics |
| [Containerd](https://containerd.io/)  | v2.1  | Container runtime for cluster  |
| [Flannel](https://github.com/flannel-io/flannel)  | v0.26.7  | CNI for cluster |
| [MetalLB](https://github.com/flannel-io/flannel)  | v0.14.9 | Load balancer for cluster|
| [Kubernetes dashboard](https://github.com/flannel-io/flannel)  | v7.12.0 | A nice dashboard for the cluster, that works with metrics API to provide information |
| [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)  | v0.2.9 | Used to seal opaque secrets on the cluster so that using the sealed secret manager they may be decrypted allowing these secrets to be published with no consequence. |
| [Tailscale](https://tailscale.com/kb/1017/install)  | v1.82.5 | A VPN service which is run on the cluster so I can access it when I'm not on my home network. Also has a nice UI and is very easy to set up |
| [Github Actions Runner](https://github.com/actions/runner)  | v2.324.0 | A VPN service which is run on the cluster so I can access it when I'm not on my home network. Also has a nice UI and is very easy to set up |
| [Harbor](https://github.com/goharbor/harbor)  | v2.12.3 | An open-source container registry which is pretty easy to set up and has a nice suite of features. |
| [MongoDB](https://github.com/bitnami/charts/tree/main/bitnami/mongodb) | v8.0.9 | Bitnami provides nice helm charts to deploy a single instance of MongoDB. Centralized NoSQL DB for all purposes |
| [Postgres](https://github.com/bitnami/charts/tree/main/.vib/prometheus) | v17.5.0 | Bitnami provides nice helm charts to deploy a single instance of Postgres. Centralized SQL DB for all purposes |
| [build-deploy](https://github.com/Sukhman-s-SKube/gh-actions) | N/A | An end-to-end ci/cd pipeline written for my self-hosted Github actions runner. Built to be a reusable workflow to make development faster, and make it easier to deploy my projects. |
| [Hobb.Y](https://github.com/Sukhman-s-SKube/hobb.Y) | N/A | A project that I initially built 2-3 years ago but I've changed it to be deployed on Kubernetes. Deployed to test the feasibility of deploying an image end-to-end onto the cluster. After checking the feasibility, I have now deployed it using my actions pipeline to allow for it to be deployed end-to-end throughout development.|
| [Minio](https://bitnami.com/stacks/minio) | 2025.5.24 | A simple Object storage which is fairly similar to AWS S3 but is opensource. I needed something to use for data staging/dumping so this was the best option for that. |

