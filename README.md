# Apache HTTP server service for Kubernetes on Wodby

Build and run Apache HTTP server applications on Kubernetes with Wodby.

This repository defines the Wodby service manifests and operational
configuration for Apache HTTP server.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Start with a boilerplate

Use one of the boilerplates exposed by this service to start with compatible
build configuration and Wodby CI:

- [Next.js](https://github.com/wodby/nextjs-boilerplate)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `httpd` |
| Type | Application service |
| Versions | `2.4` by default |
| Workloads | `main` (Deployment), primary; scalable |
| Containers | `apache` using `wodby/apache`, build target |
| Endpoints | `http`: HTTP 80 (main) |
| Application build | Git source connection enabled; boilerplates: Next.js |
| Helm | chart `oci://registry-1.docker.io/wodby/httpd`; version `0.3.1` |
| Configuration | 1 settings, 2 configuration files |

## Use this service

Reference `httpd` from a Wodby stack to use this service.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
