# Integration catalog evidence

This directory contains maintainer evidence for the concise public catalog. It is not a compatibility matrix and does not imply that Seamless Connect has tested or endorsed a target.

`targets.csv` tracks one record for every DNS or Registrar row in the public catalog using these fields:

- `category`: the public catalog category.
- `target`: the exact public target name.
- `api_documentation`: where API or integration documentation was observed, or `Not recorded` when inclusion is based on distribution reach.
- `delegated_authorization`: the known authorization model; `Not assessed` means it still needs research.
- `dnssec_support`: whether relevant DNSSEC support has been confirmed for cataloging purposes; `Not assessed` is not a claim that support is absent.
- `source`: one or more source identifiers from the table below.

| Source identifier | Source |
| --- | --- |
| `project` | Existing Seamless Connect project relationship or pilot record |
| `dnscontrol` | [DNSControl supported providers](https://github.com/DNSControl/dnscontrol#supported-providers) |
| `octodns` | [octoDNS provider organization](https://github.com/octodns) |
| `domain-connect` | [Domain Connect live DNS Providers](https://www.domainconnect.org/dns-providers/) |
| `icann` | [ICANN list of accredited Registrars](https://www.icann.org/en/contracted-parties/accredited-registrars/list-of-accredited-registrars) |

Source identifiers joined with `+` indicate independent evidence from more than one source. Maintainers should replace secondary adapter documentation with official API documentation as each target is qualified.

## Normalization and exclusions

Discovery-source entries are normalized to the current public product or parent name where practical. For example, SoftLayer DNS is cataloged as IBM Cloud DNS, Google DNS as Google Cloud DNS, and EdgeCenter as Gcore DNS.

The DNSControl and octoDNS ecosystems also contain adapters that are outside the current authoritative DNS scope:

- AdGuard Home, FortiGate, MikroTik RouterOS, NetBird, NexDNS, and UniFi Network are local network or recursive DNS control surfaces.
- DNS-over-HTTPS is a transport rather than a DNS control plane.
- AXFR+DDNS is represented by the `BIND / RFC 2136` standards-based target rather than as a separate product.
- octoDNS file, environment, Fastly-source, and dynamic-address sources feed desired state into DNS automation but are not authoritative DNS targets.

These exclusions should be revisited if Seamless Connect expands into local or recursive DNS management.
