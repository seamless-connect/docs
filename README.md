# Seamless Connect documentation

**Open Infrastructure for Domain Operations and Agent Bootstrapping**

Seamless Connect is an open-source, cross-provider developer CLI for discrete, authorized domain operations and agent bootstrapping. It is maintained as a core Internet utility under nonprofit administration.

This repository contains product, design, decision, implementation, and integration documentation for the CLI and supporting provider interfaces.

Early industry participation includes Name.com (Identity Digital), Cloudflare, and DigiCert.

The initial focus is DNS configuration through Domain Connect. DNSSEC, Primary / Secondary DNS, Domain Registration, and Agent Bootstrapping describe planned or developing workflows; availability depends on the operation and participating provider integrations. Industry participation does not imply implemented integrations.

[Use Cases & Protocols](https://seamlessconnect.org/use-cases/) · [Canonical project description](https://github.com/seamless-connect/.github/blob/main/PROJECT-DESCRIPTION.md)

Standards-adjacent specifications and related work belong in the [`seamless-connect/spec`](https://github.com/seamless-connect/spec) repository.

## Project documentation

| Section | Purpose |
| --- | --- |
| [Product requirements](product/) | Requirements defining what the project is intended to deliver. |
| [Designs](designs/) | Technical and product designs describing how requirements will be implemented. |
| [Decisions](decisions/) | Project decisions, including their context and rationale. |
| [Working notes](working-notes/) | Non-normative, exploratory material that may reflect individual contributors' thinking. |
| [Integration catalog](catalog/) | Candidate control planes for domain, DNS, agent-registration, and bootstrap integrations. |

## Integration checklists

| Checklist | Purpose |
| --- | --- |
| [DNS Provider Integration Checklist](integration-checklists/dns-provider-integration-checklist.md) | Implementation and estimation guide for DNS updates and child-side DNSSEC responsibilities. |
| [Service Provider Integration Checklist](integration-checklists/service-provider-integration-checklist.md) | Implementation and estimation guide for Service Providers using Domain Connect or the Seamless Connect Operation API. |
| [Registrar Integration Checklist](integration-checklists/registrar-integration-checklist.md) | Implementation and estimation guide for the intentionally DNSSEC-only Registrar integration and pilot. |

## Community

- [Industry events calendar](community/events.md): an informational calendar of relevant domain, DNS, standards, cloud, and Internet-infrastructure events.

## Contributing

See the [contribution guide](Contributing.md) for the project's contribution process, licensing, and reporting guidance.
