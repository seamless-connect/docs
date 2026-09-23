# Seamless Connect integration catalog

This catalog tracks systems whose control planes could integrate directly with Seamless Connect. It covers domain and DNS operations together with agent registration, discovery, and bootstrap.

## What counts as an integration target?

Include a system when Seamless Connect can directly integrate with its control plane to create, discover, authorize, modify, verify, or remove domain or DNS state, or agent registration or bootstrap state.

The catalog does not attempt to list every end user, standards body, or implementation dependency. A listing identifies a possible integration surface; it does not imply endorsement, partnership, or implementation commitment.

## Categories

| Category | Examples | Primary integration purpose |
| --- | --- | --- |
| [DNS control planes](dns-control-planes.md) | Cloudflare DNS, Route 53, deSEC | DNS CRUD, DNSSEC, Domain Connect |
| [Registrar control planes](registrar-control-planes.md) | Name.com, DNSimple, OpenSRS | DS records, nameservers, registration, transfers |
| [Hosting and cloud control planes](hosting-cloud-control-planes.md) | AWS, Azure, cPanel, Plesk | Domain and DNS state within broader infrastructure |
| [Enterprise DNS and DDI](enterprise-dns-ddi.md) | Infoblox, BlueCat, PowerDNS | Enterprise DNS automation and policy |
| [Orchestration and IaC](orchestration-iac.md) | Terraform, Pulumi, DNSControl | Provider-independent infrastructure operations |
| [Agent registry and discovery](agent-registry-discovery.md) | Agent registries, `.well-known`, DNS | Agent publication, discovery, and bootstrap |
| [Agentic developer tools](agentic-developer-tools.md) | Codex, Claude Code, Cursor | Portable Seamless Connect operations for agents |

## Status vocabulary

| Status | Meaning |
| --- | --- |
| `Research` | The target or integration pattern needs further investigation. |
| `Candidate` | A concrete integration target has been identified. |
| `Contacted` | Initial outreach has occurred. |
| `Interested` | The target has expressed interest in an integration. |
| `Pilot` | The target is participating in a pilot or implementation effort. |
| `Integrated` | A usable Seamless Connect integration exists. |

Statuses describe Seamless Connect's integration progress, not the maturity or quality of the target system. They may change as work advances.

## Catalog maintenance

The concise public tables are backed by a separate [maintainer evidence dataset](evidence/). It records discovery sources, API documentation status, delegated-authorization information, and DNSSEC support without adding operational detail to the public catalog.

DNS targets are discovered from active automation ecosystems and Domain Connect implementations. Registrar research prioritizes documented APIs, reseller platforms, and meaningful distribution reach; the full ICANN accreditation directory remains a research source rather than a list to import wholesale.
