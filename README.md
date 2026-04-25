# mirror-Gokapi

Hardened container mirror of [Forceu/Gokapi](https://github.com/Forceu/Gokapi). THEKROLL's internal file-transfer image, published publicly as a reference build.

## What this repository does

Each night, a GitHub Actions pipeline in this repository:

1. Checks `Forceu/Gokapi` for a new release tag
2. If there is one, clones the upstream source, applies our `Dockerfile.override`, and rebuilds the image
3. Builds the image once, scans it with Trivy (SARIF to the Security tab), produces a CycloneDX SBOM
4. If no `CRITICAL` or `HIGH` vulnerabilities with an available fix are found: pushes to `ghcr.io/thekroll-ltd/gokapi` and opens a digest-pin PR
5. If findings are present: blocks the push, opens an issue, retains the full audit bundle for 90 days

The result is an image rebuilt on `gcr.io/distroless/static:nonroot` rather than the upstream Alpine base, with all artifacts a supply-chain audit requires.

## Image

`ghcr.io/thekroll-ltd/gokapi:<tag>` and `ghcr.io/thekroll-ltd/gokapi@sha256:<digest>`.

Tags track upstream Gokapi releases. Digests are the authoritative pin.

## No SLA

This is THEKROLL's own internal build, made public as a reference. There is no service-level agreement, no support commitment, no compatibility guarantee. Scheduling, retention, and availability may change without notice.

**For production-critical use**, fork the template and run your own pipeline: [THEKROLL-LTD/oss-mirror-build](https://github.com/THEKROLL-LTD/oss-mirror-build). Five minutes of setup gets you the same controls under your own ownership, with your own SLA.

## License

The build system in this repository — workflow YAML, Dockerfile override, documentation — is licensed under Apache-2.0. See [`LICENSE`](LICENSE).

The container images produced here contain Gokapi, which is licensed under AGPL-3.0. The images inherit AGPL-3.0. Operators who expose the image over a network must comply with AGPL §13 (Remote Network Interaction). See [`NOTICE.md`](NOTICE.md) for details.

## Related

- **Upstream:** [github.com/Forceu/Gokapi](https://github.com/Forceu/Gokapi) (AGPL-3.0)
- **Template this repo was forked from:** [THEKROLL-LTD/oss-mirror-build](https://github.com/THEKROLL-LTD/oss-mirror-build) (Apache-2.0)
- **Context and rationale:** *[Stop Pulling Random Docker Images](https://medium.thekroll.ltd/stop-pulling-random-docker-images-c19e94559cc6)* — the post that describes this pipeline and why we run it