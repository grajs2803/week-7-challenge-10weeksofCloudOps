# Kubernetes Docker Registry for DevOps

## Overview
This project addresses the need to streamline the build and deployment process in an evolving organization. As a DevOps engineer, you've been tasked with deploying a local Docker image registry on Kubernetes within the "democloudops" namespace. The project meets several key requirements:

## Cluster Provisioning
- Provision a Kubernetes regional cluster with at least three worker nodes.
- Ensure node distribution across different availability zones.
- employed AWS EKS using AWS CLI and EKSCTL to provision the cluster nodes with requirements provided

## Sample Deployment
- Deploy the Docker registry with three replicas, each scheduled on a separate node.
- The Docker registry runs in a container with the image "registry:2.8.2" and is named "registry-cloudops".
- Images are stored under "/var/lib/10weeksofcloudops".

## Storage
- Configure a Persistent Volume (PV) with a minimum request of 50Gi storage space.
- Ensure data stored in the PV survives pod restarts.

## Container Configuration
- The application container exposes itself on port 5000.
- Kubelet performs health checks on "registry-cloudops" and restarts the pod if it doesn't respond on TCP port 5000 for 30 seconds. An initial 15-second wait precedes the first health check.
- The container requests 1Gi Memory and has a limit of 2Gi.

## Init Container
- An init container is implemented within the "registry-cloudops" deployment to ensure the presence of the required data directory.
- The init container uses an Alpine-based image and creates the data directory "/var/lib/10weeksofcloudops" if it doesn't exist.

## Service Exposure
- A Kubernetes load balancer service is created to expose the application externally.
- The service maps an external port to port 5000 on the pods.

## Network Policies
- Network Policies are implemented to restrict incoming and outgoing network traffic for the Docker registry pods.
- Necessary communication is allowed while enforcing security practices.

## Secrets and ConfigMap
- Sensitive information, such as authentication credentials for the Docker registry, is stored in Kubernetes Secrets.
- Configuration settings for the Docker registry deployment are managed using a ConfigMap.

## Backup and Recovery
- A backup strategy is defined for the data stored in the Persistent Volume.
- Options for restoring the registry in case of data loss or pod failures are explored.
- used VELERO solution to implement backup and restoration of nodes and pv 

## Cluster Upgrade
- Perform a cluster upgrade to the next major version.
- Ensure the cluster's health, including the workload, after the upgrade.

This project helps streamline your organization's build and deployment processes by providing a robust and scalable Docker image registry on Kubernetes within the "democloudops" namespace.

Feel free to contribute to this project, report issues, or suggest improvements. Happy DevOps-ing!
