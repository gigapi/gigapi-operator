# <img src="https://github.com/user-attachments/assets/5b0a4a37-ecab-4ca6-b955-1a2bbccad0b4" />

# <img src="https://github.com/user-attachments/assets/74a1fa93-5e7e-476d-93cb-be565eca4a59" height=25 /> GigAPI Kubernetes Operator

A Kubernetes Operator for managing [GigAPI](https://github.com/gigapi/gigapi) clusters on Kubernetes.

## Overview

The GigAPI Operator automates the deployment and management of GigAPI server and querier components, including persistent storage, configuration, and optional Redis metadata support. It ensures your GigAPI cluster is always running according to your desired spec.

## Features
- Declarative management of GigAPI clusters via CRD
- Automated StatefulSet, Service, PVC, ConfigMap, and Secret management
- Optional Redis deployment for distributed metadata
- Flexible configuration via environment variables
- Supports rolling updates and scaling

## CRD Spec Summary
The `GigAPI` custom resource supports:
- **server/querier**: Replica count, resource requests/limits
- **storage**: Size, storage class, access modes
- **config**: All GigAPI environment variables (see below)
- **service**: Service type and ports
- **deployRedis**: Optionally deploy Redis for metadata

See [`config/samples/gigapi_v1alpha1_gigapi.yaml`](config/samples/gigapi_v1alpha1_gigapi.yaml) for a full example.

## Quickstart (Development)

### Prerequisites
- [Go](https://golang.org/dl/) (>=1.21)
- [kubebuilder](https://book.kubebuilder.io/quick-start.html#installation)
- [Docker](https://www.docker.com/)
- Access to a Kubernetes cluster (kind, minikube, or real cluster)

### 1. Install CRDs
```sh
make install
```

### 2. Run the Operator Locally
```sh
make run
```

### 3. Apply a Sample GigAPI Resource
```sh
kubectl apply -f config/samples/gigapi_v1alpha1_gigapi.yaml
```

### 4. Verify Resources
```sh
kubectl get gigapi
kubectl get statefulsets,pods,svc,pvc,configmap,secrets
```

You should see StatefulSets, Services, PVC, ConfigMap, and Secret for your GigAPI cluster, and Redis if enabled.

## Deploying in Cluster
To build and deploy the operator in your cluster:
```sh
make docker-build docker-push IMG=<your-repo>/gigapi-operator:latest
make deploy IMG=<your-repo>/gigapi-operator:latest
```

## Configuration Reference
All [GigAPI environment variables](https://github.com/gigapi/gigapi#settings) are supported via the `config` field in the CRD:
- `GIGAPI_ROOT`, `GIGAPI_MERGE_TIMEOUT_S`, `GIGAPI_SAVE_TIMEOUT_S`, `GIGAPI_NO_MERGES`, `GIGAPI_UI`, `GIGAPI_MODE`, `GIGAPI_METADATA_TYPE`, `GIGAPI_METADATA_URL`, `HTTP_PORT`, `HTTP_HOST`, `HTTP_BASIC_AUTH_USERNAME`, `HTTP_BASIC_AUTH_PASSWORD`, `FLIGHTSQL_PORT`, `FLIGHTSQL_ENABLE`, `LOGLEVEL`, `DUCKDB_MEM_LIMIT`, `DUCKDB_THREAD_LIMIT`

## Troubleshooting
- Check operator logs: `make run` or `kubectl logs deployment/gigapi-operator-controller-manager -n gigapi-operator-system`
- Ensure your cluster has a default StorageClass for PVCs
- If Redis is enabled, check for the Redis StatefulSet and Service

## Next Steps
- Add status reporting and health checks
- Tune resource templates for production
- Contribute improvements via PRs!

## Resources
- [GigAPI Project](https://github.com/gigapi/gigapi)
- [Kubebuilder Book](https://book.kubebuilder.io/)

---

_This project is under active development. Feedback and contributions welcome!_
