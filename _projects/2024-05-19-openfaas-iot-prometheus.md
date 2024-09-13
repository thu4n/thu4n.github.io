---
title: Deploy and monitor Multi-node AKS cluster running OpenFaaS service (with MQTT integration)
date: 2024-05-19 18:40:00 +0700
author: [thu4n,r1anl3]
layout: project
---

This project outlines the steps involved in deploying and monitoring OpenFaaS, a serverless framework for providing Function as a Service (FaaS), on a multi-node Azure Kubernetes Service (AKS) cluster. By leveraging OpenFaaS, we can streamline the development and deployment of microservices within a Kubernetes environment on Azure. 

In addition, we have configured a TLS certificate for the gateway, integrated MQTT for function triggering, and implemented webhook events to enhance the system's responsiveness and versatility. The deployment also includes auto-scaling to ensure efficient resource utilization, and a comprehensive monitoring system using Prometheus and Grafana to provide real-time insights and performance metrics.

- GitHub Repository: [https://github.com/thu4n/NT533-AKS-OpenFaaS](https://github.com/thu4n/NT533-AKS-OpenFaaS)
