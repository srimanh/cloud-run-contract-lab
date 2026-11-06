# Deployment interpretation

1. Local Docker proves only the tested local runtime contract. It does not prove an Artifact Registry push, cloud revision readiness, traffic allocation, IAM or a real service URL response.
2. The ingress container must honor injected PORT (default 8080, configurable) and bind 0.0.0.0. EXPOSE documents a port; it neither starts a listener nor publishes it.
3. A service is a stable named endpoint and traffic configuration. A revision is an immutable image/config snapshot. Container instances execute a revision and can scale independently.
4. The prebuilt path is build/test -> registry tag and authenticated push -> deploy with --image IMAGE_URL -> inspect readiness/traffic -> authorized URL request and logs. --source . instead builds source using the conventional Cloud Build/Artifact Registry path; it need not produce identical image bytes. Compare digest identity when relevant.
5. Ingress controls permitted network paths; invoker authorization controls who may call; runtime service identity controls what the workload may access downstream. HTTPS encrypts transport, not a promise of anonymous access.
6. Readiness does not prove business correctness. Check traffic allocation and real endpoint responses plus revision-scoped logs. An existing good revision may keep serving during a failed replacement, but a first deployment has no fallback. Rollback reroutes traffic; it does not undo database writes or guarantee instantaneous recovery.
7. Scale-to-zero is not a zero-total-cost guarantee. Free-tier use requires an eligible billing account; build, registry, networking and other services can add costs. This lab creates no cloud resources and uses local Docker only.
