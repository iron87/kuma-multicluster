# Kuma Multicluster Mesh

This repository provides a detailed guide for deploying [Kuma](https://kuma.io/) in a multicluster environment using Kubernetes. It includes step-by-step instructions for provisioning clusters, setting up the control planes, deploying a demo app, and applying service mesh policies.

## 📐 Architecture

The topology features:
- A **Global Control Plane** deployed in a dedicated cluster
- One or more **Zone Control Planes** deployed in other clusters
- Connectivity established using Kuma's built-in multizone support

  ![kuma_multizone-2](https://user-images.githubusercontent.com/11998279/195276898-21201217-4edf-4c7a-8527-0503634ba282.svg)

## 📘 Contents

This repository includes the following key components:

| File | Description |
|------|-------------|
| `00-gke-cluster-provisioning.md` | Instructions for provisioning GKE clusters |
| `01-central-control-plane.md` | Setup guide for the Global Control Plane |
| `02-zone-control-plane.md` | Guide for configuring Zone Control Planes |
| `03-counter-demo-app.md` | Deployment of a demo application |
| `04-traffic-permission.md` | Applying `TrafficPermission` policies |
| `05-locality-aware-lb.md` | Enabling locality-aware load balancing |
| `06-basic-auth.md` | Configuring basic auth |
| `07-external-service-traffic-disable.md` | Managing external service traffic |
| `08-kuma-on-vm.md` | Running Kuma on virtual machines |
| `09-prometheus.md` | Enabling Prometheus monitoring |
| `10-jager.md` | Jaeger tracing setup |
| `11-loki.md` | Loki logging integration |

## 🚀 Quick Start

1. **Provision Kubernetes Clusters**
   Follow the steps in `00-gke-cluster-provisioning.md` to create the necessary clusters.

2. **Deploy Global Control Plane**
   See `01-central-control-plane.md`.

3. **Deploy Zone Control Planes**
   Refer to `02-zone-control-plane.md`.

4. **Install the Demo Application**
   Guide available in `03-counter-demo-app.md`.

5. **Apply Mesh Policies**
   Configure traffic and observability using the remaining docs.

## 📊 Observability

This setup integrates with Prometheus, Jaeger, and Loki for full observability in a distributed mesh. Configuration files are available under:
- `remote-write.yaml`
- `scrape-config.yaml`

## 🧰 Requirements

- Kubernetes clusters (GKE recommended)
- `kubectl`, `helm`, and `kumactl` CLI tools
- Basic knowledge of service meshes and Kubernetes



---



