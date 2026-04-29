# Virtual Machine Service Tech Preview

## Overview

This document describes procedures for configuring the VM Service Tech Preview for IBM Sovereign Core.

## Prerequisites

1. Mirror example Fedora image from `quay.io/containerdisks/fedora:latest` to control plane's registry.

2. Sign into the control plane's Account UI as the platform tenant and create a shared cluster with the `vm.sovereign.cloud.ibm.com/virtualization-enabled=true` label.

3. Login to the control plane's OpenShift cluster via the `oc` CLI.

## Procedure

1. Run `configure-control-plane.sh --shared_cluster_url=https://console-openshift-console.apps.<domain>` with shared cluster access details to install the `vm-service-broker` Helm chart into the control plane cluster

   This will populate a values overrides file and install the `vm-service-broker` Helm chart, create an IDP client to be used in the shared cluster's OAuth configuration, and create some local manifests to be used to configure Oauth for the shared cluster.

   If `shared_cluster_url` is not specified, this script will attempt to lookup details for the first managed cluster with the `vm.sovereign.cloud.ibm.com/virtualization-enabled=true` label.

2. Login to the shared cluster's UI.

3. Apply the generated `idp-client-secret.yaml` and `idp-root-ca.yaml` resources to the cluster.

4. From within the shared cluster's UI, access `Administration` -> `Cluster Settings` -> `Configuration` -> `OAuth`, and add a new `OpenID Connect` identity provider to the `cluster` OAuth resource.

5. Ensure there is a default storage class set for the shared cluster. This will be used for the Fedora sample container disk image as well as be the default for all created Virtual Machines.

6. Login to the control plane's MSP UI and set the VM Service Tech Preview as visible in the catalog to enable tenants to provision the service.