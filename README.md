This repository contains Kubernetes manifests to deploy the [S3_file_management](https://github.com/Maksonik/S3_file_management) application. It uses **kustomize** with a common `base` configuration and environment-specific overlays.

## Repository layout

```
project/
├── base/               # Manifests shared by all environments
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── logging/        # Loki and Promtail
│   └── monitoring/     # Prometheus and Grafana
└── overlays/
    ├── stage/          # Staging configuration
    └── prod/           # Production configuration
```

- **base** provides common resources for every deployment.
- **overlays** apply patches and parameters for each environment.

## Requirements

- `kubectl` with kustomize support
- Access to your Kubernetes cluster
- A `secret.env` file with the application environment variables. Create this file in the appropriate overlay directory (`project/overlays/<env>/secret.env`). See the S3_file_management repository for the expected variables.

## Deployment

1. Clone this repository and change into it.
2. Create `secret.env` inside the overlay for the target environment and fill in the variables.
3. Apply the manifests:
   ```bash
   kubectl apply -k project/overlays/<env>
   ```
   Replace `<env>` with `stage` or `prod`.
4. Confirm that the pods are running and the service is reachable.

The deployment includes the following components:

- The **S3_file_management** application (Deployment, Service, Ingress, HPA)
- Monitoring stack (**Prometheus**, **Grafana**)
- Logging stack (**Loki**, **Promtail**)

## Updating configuration

Modify the manifests under `project/base` or the patches inside `project/overlays/<env>` and reapply them with `kubectl apply -k` for the desired environment.

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
