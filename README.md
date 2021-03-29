# Production Readiness Checklist

[Production Readiness Checklists](docs/references/production-readiness-checklist.md) and other reference [docs](/docs).


The checklists are divided into 2 phases:

- [ Design Checklist](/docs/references/design-checklist.md): the checklist you must meet before beginning development of microservice

- [ Pre-production Checklist](/docs/references/pre-production-checklist.md): the checklist you must meet before production deployment

The check items in each phase vary by its [Production Readiness Level](/docs/references/production-readiness-level.md) which is defined by its SLO. You can see the guide [Check Production Readiness](/docs/guides/check-production-readiness.md) to know checklist usage and its review process. 


Have design phase review before beginning development of your microservice and have pre-production phase review before rolling out production release.



# Design checklist

This checklist contains items are things that must be considered during the design phase and verified before the start of implementation.

## General

- [ ] [ Stateless server](docs/concepts/stateless-server.md) - All persistent data is stored outside of the container.
- [ ] [ Deploy order](docs/concepts/deploy-order.md) - Its deploy does not have strong order.
- [ ] [ Exclusive data ownership](docs/concepts/exclusive-data-ownership.md) - It is the only service that can access its data store.

## Security

- [ ] [ Authentication](#) - It is protected by an authentication service.
- [ ] [ Authorization](#) - Access is restricted to the appropriate level. Consider who should have access to each exposed API and what they are allowed to do.
- [ ] [ Transport Security](#) - It uses TLS to communicate with other services over the Internet.

## Sustainability

- [ ] [ No short-term transfer](#) - Its team members are not forced to move to another team in the short term.
- [ ] [ OnCall considered team](#) - Its team follows OnCall practices.
- [ ] [ Dependency SLA](#) - Its team knows SLA of the service dependencies.
- [ ] [ SLOs](docs/concepts/slo.md) - Its SLOs and SLOs owner are defined.


# Pre-production checklist

This checklist contains points that must be satisfied during implementation and verified prior to release.

It is recommended to ensure that your service is deployed in production (but not receiving production traffic) before requesting the PRC, as some of the points in the list below can only be validated (e.g. capacity estimation, dashboards, screenboards, alerting, profiling, ...) if the service is deployed in production and can receive some non-production traffic. This should be done only if your service will not impact other production services or datasets. Please let us know in the issue if you think this would be a problem for your service.


## Maintainability

- [ ] [ Unit test](#) - It has unit tests. And the unit tests are running in a CI system.
- [ ] [ Test coverage](https://docs.codecov.io/docs/about-code-coverage) - Its test coverage is reported to Codecov in CI system.
- [ ] [ High Test coverage](#) - Its test coverage is over 80%.
- [ ] [ Config in env-var](https://12factor.net/config) - Its config can be overridden via environment variable.
- [ ] [ dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file) - It has dockerignore to reduce the Docker image size.
- [ ] [ No latest tag](https://twitter.com/thockin/status/1085223284122087424) - Its Docker image tag is not latest or master.
- [ ] [ Dependabot](https://dependabot.com/) - Its dependencies are automatically updated.
- [ ] [ Automated build](docs/concepts/automated-build.md) - Its build process is automated (binary build and Docker image build is in this scope).
- [ ] [ Automatic build](docs/concepts/automatic-build.md) - Its automated build process is running in CI/CD system.
- [ ] [ Automated deploy](#) - Its deploy process is automated.
- [ ] [ Automatic deploy](#) - Its automated deploy process is running in CI/CD system.
- [ ] [ Gradual deploy](docs/concepts/gradual-deploy.md) - Its deploy can be gradual if you want.
- [ ] [ Automated rollback](docs/concepts/automated-rollback.md) - Its rollback process is automated.
- [ ] [ Automatic rollback](#) - Its rollback process is automatic.


## Observability

- [ ] [ Tracing](https://microservices.io/patterns/observability/distributed-tracing.html) - Its requests are traced.
- [ ] [ Monitoring](#) - Performance metrics and fault events reported to [prometheus](https://prometheus.io/).
- [ ] [ Actionable alert](#) - Alerts are actionable.
- [ ] [ Warning alert](#) - Its warning alerts are reported.
- [ ] [ Critical alert](#) - Its critical alerts are reported. (PagerDuty)
- [ ] [ OnCall rotation](#) - It has a PagerDuty team, escalation policy, schedules.
- [ ] [ OnCall playbooks ](#)- It has OnCall playbooks.
- [ ] [ Log to STDOUT ](#)- Its logs are output to STDOUT/STDERR.
- [ ] [ Log as JSON ](#)- Its logs are emitted in container log format.
- [ ] [ Log with annotation](#) - Its logs have Request ID annotation
- [ ] [ Profiling](#) - It is profiled by GCP Stackdriver Profiler.
- [ ] [ Error tracking](#) - Its errors are tracked by Jira/ServiceDesk.


## Reliability

- [ ] [ Auto Scale](docs/concepts/auto-scaling.md) - It automatically scales horizontally to handle fluctuating workloads, its HPA is set as described in the [Resource Requests and Limits](docs/concepts/resource-requests-and-limits.md) documentation, and can be scaled manually if needed.
- [ ] [ CPU req/limit](docs/concepts/resource-requests-and-limits.md) - Its CPU limit and request are set as described in the [Resource Requests and Limits](docs/concepts/resource-requests-and-limits.md) documentation.
- [ ] [ Memory req/limit ](docs/concepts/resource-requests-and-limits.md)- Its memory resource request value is as same as limit value.
- [ ] [ Capacity planning ](docs/concepts/capacity-planning.md)- It can handle the expected load: either load test has been performed, or the expected traffic is under control (e.g., by Gateway).
- [ ] [ Zero downtime deploy](docs/concepts/zero-downtime-deploy.md) - Its deploy process does not cause service degradation or downtime (e.g. error rate does not increase during deploy).
- [ ] [ Graceful shutdown](docs/concepts/graceful-shutdown.md) - It can stop gracefully.
- [ ] [ Graceful degradation](docs/concepts/graceful-degradation.md) - It keeps working, at least partially, while dependencies (e.g. other service or database) are not working partially or completely.
- [ ] [ PreStop](docs/concepts/container-lifecycle-hooks-pre-stop.md) - It has a preStop. See more on [Configure PreStop](docs/guides/configure-pre-stop.md).
- [ ] [ PDB](docs/concepts/pod-disruption-budget.md) - It has a PodDisruptionBudget set as described in the [Configure Pod Distription Budget](docs/guides/configure-pod-disruption-budget.md)
- [ ] [ Liveness Probe](docs/concepts/liveness-probe.md) - It has a health check (endpoint) for liveness probe. And liveness probe is configured. See more on [Configure Liveness Probe](docs/guides/configure-liveness-probe.md).
- [ ] [ Readiness Probe](docs/concepts/readiness-probe.md) - It has a health check (endpoint) for readiness probe. And readiness probe is configured.
- [ ] [ Timeout](#) - It sets an appropriate timeout for requests over a network.
- [ ] [ Smart retry](docs/concepts/smart-retries.md) - It performs smart retries when interacting with dependencies (e.g. other services or database).



## Security

- [ ] [ Security review](#) - It has completed the security design review by security team.
- [ ] [ Non-root user ](#)- Its docker container runs as non-root user
- [ ] [ Secrets](docs/concepts/secrets.md) - Its sensitive configuration is stored in Kubernetes secrets.
- [ ] [ Non-sensitive log](docs/concepts/non-sensitive-log.md) - It does not write sensitive information to app logs (STDOUT/STDERR).
- [ ] [ RBAC](https://en.wikipedia.org/wiki/Role-based_access_control) - Role-based access control
- [ ] [ SAST](docs/guides/configure-security-testing.md) - Perform Static Analysis Security Testing , i.e [dependabot](https://dependabot.com)
- [ ] [ DAST](docs/guides/configure-security-testing.md) - Perform Dynamic Analysis Security Testing , i.e [zaproxy](https://www.zaproxy.org/), [w3af](http://w3af.org)
- [ ] [ Secrets overrides](#) - Provide a mechanism to override default product credentials
- [ ] [ Checksum](#) - Provide a mechanism for verifying builds integrity.


## Accessibility

- [ ] [ Design Doc](docs/concepts/design-doc.md) - Its design doc is up to date with the implementation.
- [ ] [ Description](#) - It has service description.
- [ ] [ Contact](#) - It has contact info about the owners.
- [ ] [ Source repo](#) - It has links to source repo.
- [ ] [ Docs](#) - It has links to docs for users.
- [ ] [ SLOs](#) - Its dashboard shows SLOs.


## Data Storage

- [ ] [ Data Replication](#) - Its data is replicated  (if required).
- [ ] [ Minimal Operator Privileges](#) - Personnel has minimal access privileges and accesses are auditable.
- [ ] [ Recovery](#) - It can be recovered from backup; the procedure has been defined and tested.
- [ ] [ Fast Recovery](#) - It can be recovered from backup in less than 2 hours; the procedure is described in the OnCall playbook, and it is practiced every 6 months.
- [ ] [ PIT Recovery](#) - Point-in-time recovery from backup can be completed in less than 2 hours.


