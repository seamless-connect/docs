# Product Brief: DNS Configuration

Status: Draft

Audience: DNS Provider product and engineering teams, Service Providers, and Seamless Connect contributors.

## Summary

Seamless Connect gives DNS Providers a simple way to accept authorized DNS configuration requests without building a separate Service Provider interface or workflow system.

A DNS Provider integrates once with Seamless Connect through scoped authorization and its existing DNS API. Seamless Connect accepts requests from Service Providers, Agents, and other initiators; validates them against applicable templates and policies; coordinates conflict resolution and Domain Owner authorization; and sends the DNS Provider a deterministic operation plan for execution.

The DNS Provider retains control over which capabilities it supports and whether a request may execute.

## Why adopt Seamless Connect

DNS Providers should be able to offer a consistent configuration experience while minimizing new integration work.

Seamless Connect allows a DNS Provider to:

- Reuse its existing DNS API and authorization systems.
- Integrate once instead of supporting separate request interfaces for different Service Providers and Agents.
- Offload template handling, policy checks, conflict coordination, and synchronous and asynchronous workflows.
- Adopt only the capabilities it is ready to support.
- Retain final enforcement and execution within its own infrastructure.

For Domain Owners, this means fewer manual DNS instructions and a more consistent authorization experience. For Service Providers and Agents, it provides one predictable way to request DNS configuration across participating DNS Providers.

## How it works

```text
Service Provider, Agent, or other initiator
                    |
                    v
            Seamless Connect
  validate -> apply policy -> resolve conflicts
       -> obtain authorization -> create plan
                    |
                    v
               DNS Provider
          enforce -> execute -> report
```

From the DNS Provider's perspective, Seamless Connect is the requesting service. The DNS Provider does not need different integrations based on who initiated the request.

Seamless Connect still retains the initiator's identity and authorization context for policy enforcement, audit, and reporting. Actor abstraction must not become loss of attribution.

## Two DNS Provider adoption paths

### DNS Providers that support Domain Connect

An existing Domain Connect implementation remains useful. The DNS Provider may reuse its template-processing and DNS execution capabilities while delegating more of the Service Provider-facing experience to Seamless Connect, including:

- Request validation.
- Template coordination.
- Conflict detection and resolution workflows.
- Domain Owner authorization experiences.
- Synchronous and asynchronous request handling.
- Verification and result reporting.

Seamless Connect preserves standard Domain Connect compatibility for Service Providers while reducing the DNS Provider's UI and workflow burden.

### DNS Providers that do not support Domain Connect

A DNS Provider should not need to implement the complete Domain Connect protocol before adopting Seamless Connect.

The target integration consists of:

1. OAuth or equivalent scoped authorization.
2. An adapter to the DNS Provider's existing DNS API.
3. A capability profile describing supported record types, operations, templates, DNS behavior, and policy constraints.

Seamless Connect handles the Service Provider interface, Domain Connect request compatibility, coordination workflow, and translation into the DNS Provider's supported operations.

## Incremental capabilities

A DNS Provider can adopt Seamless Connect in stages. Each stage adds value without requiring every later capability.

| Capability | DNS Provider commitment | Seamless Connect responsibility |
| --- | --- | --- |
| Connect | Provide scoped authorization and access to an existing DNS API | Normalize the integration and maintain the capability profile |
| Standard Domain Connect | Support selected templates and required record operations | Accept compatible requests, validate templates, coordinate authorization, and translate operations |
| Seamless-hosted experience | Delegate request and conflict-resolution interactions | Present consistent Domain Owner authorization and conflict choices |
| Asynchronous operations | Continue exposing ordinary DNS operations and status where available | Manage approvals, scheduling, retries, callbacks, and operation status |
| Advanced operations | Declare permitted create, read, update, and delete operations | Enforce policy and submit deterministic operation plans |
| Agent requests | Accept authorized operation plans under declared policy | Constrain Agent requests to pre-approved, deterministic behavior |

The initial adoption target is the smallest useful integration: scoped authorization, an existing DNS API, and a capability profile. More advanced capabilities can follow.

## Responsibility boundaries

### Domain Owner

The Domain Owner authorizes access and makes any required decision about changes that could replace or disrupt an existing service. A valid delegation may allow another actor to act within a defined scope.

### Service Provider, Agent, or other initiator

The initiator requests an outcome and supplies permitted inputs. A request does not grant authority to change DNS or expand the initiator's scope.

### Seamless Connect

Seamless Connect:

- Authenticates and attributes the initiator.
- Validates the request against supported templates, policies, and DNS Provider capabilities.
- Detects conflicts and coordinates their resolution.
- Obtains or verifies Domain Owner authorization or delegation.
- Produces a deterministic operation plan.
- Manages synchronous and asynchronous workflows.
- Verifies and reports the result.

Only a request that passes these checks is submitted for execution.

### DNS Provider

The DNS Provider:

- Authenticates the Domain Owner and grants scoped access.
- Declares supported capabilities and policy constraints.
- Enforces final authorization and operational limits.
- Executes permitted changes through its existing infrastructure.
- Reports execution results and available DNS state.

## Conflict handling and authorization

Conflict detection informs authorization; it does not replace it.

Seamless Connect may detect and classify conflicts, present their consequences, and coordinate resolution. When a proposed change could replace or disable an existing service, the Domain Owner or an authorized delegate must approve that outcome. A Service Provider request alone is not sufficient authorization.

DNS behavior differs across DNS Providers. Capability profiles should describe supported record types, visible DNS state, aliasing or CNAME-flattening behavior, conflict-detection limits, and verification capabilities. When the existing state is uncertain, Seamless Connect should disclose that uncertainty and verify the resulting state.

Partial application may be supported only when the remaining configuration is coherent, dependencies are preserved, and the result is reported accurately.

## Templates and advanced operations

Seamless Connect supports standard Domain Connect templates and compatible Service Provider requests. It will also participate in template approval and lifecycle management, which will be defined separately.

Advanced requests may reference a template, policy, or ruleset. A DNS Provider may also choose to accept explicit non-templated operations. Seamless Connect must enforce the DNS Provider's declared policy before requesting execution and must never silently convert a rejected templated request into a non-templated request.

The detailed policy model for advanced operations remains a product requirement to resolve. In particular, the project must define whether every advanced request must conform to a pre-approved policy or ruleset, and who may approve and submit each kind of request.

## Agents

Agents are a first-class initiator, but their executable requests must be deterministic.

An Agent may choose among authorized operations and provide inputs within approved constraints. It may not invent record structures, targets, operations, or authorization scope at runtime. Free-form model output is not an executable DNS request.

Agent requests should reference a pre-approved template, policy, or ruleset from which Seamless Connect can derive the complete operation plan.

## Adoption outcome

Successful adoption means that:

- A DNS Provider can integrate without replacing its DNS infrastructure or building a new workflow control plane.
- A DNS Provider with Domain Connect can reuse its investment and delegate coordination work to Seamless Connect.
- A DNS Provider without Domain Connect can participate through scoped authorization, an existing DNS API, and a capability profile.
- Service Providers can use standard Domain Connect or Seamless Connect capabilities without building DNS Provider-specific integrations.
- Domain Owners retain authority over disruptive changes.
- Requests from Service Providers, Agents, and other initiators are converted into attributable, authorized, and deterministic operation plans.

## Product principles

- Make DNS Provider adoption small and incremental.
- Preserve standard Domain Connect compatibility.
- Abstract the initiating actor from the DNS Provider without losing attribution.
- Preserve Domain Owner authority and DNS Provider policy control.
- Centralize coordination complexity in Seamless Connect.
- Keep Agent execution deterministic.
- Do not modify unrelated DNS records or silently broaden a request.
- Verify outcomes and report partial or uncertain results honestly.

## Further detail

This brief describes the product direction and adoption model. Detailed behavior belongs in follow-on documents so DNS Provider reviewers can understand the value and integration shape without first reading a full system specification.

Follow-on product requirements should define:

- Request and operation lifecycles.
- Template approval and lifecycle management.
- DNS Provider capability profiles.
- Conflict classification and authorization rules.
- Scoped authorization and delegation.
- Synchronous and asynchronous behavior.
- Advanced operation policies.
- Deterministic constraints for Agents.
- Verification, failure, partial outcome, and audit requirements.

System design documents should define the implementation architecture and DNS Provider adapter contract. Decisions that affect multiple integrations should be recorded separately.

Role-specific adoption steps are maintained in the existing integration checklists:

- [DNS Provider integration checklist](../integration-checklists/dns-provider-integration-checklist.md)
- [Service Provider integration checklist](../integration-checklists/service-provider-integration-checklist.md)
- [Registrar integration checklist](../integration-checklists/registrar-integration-checklist.md)
