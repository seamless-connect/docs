# Product Brief: DNS Configuration

Status: Draft

## Product vision

Seamless Connect is a coordination service between Service Providers, Domain Owners, and DNS Providers.

It gives Service Providers a consistent way to request DNS operations while allowing DNS Providers to integrate through their existing APIs and retain control over authorization and execution.

Seamless Connect should support conventional SaaS and cloud services as well as Agents acting for Domain Owners.

## Problem

Service Providers currently rely on a mix of Domain Connect, manual instructions, and DNS Provider-specific integrations.

This creates several problems:

- DNS Providers must support different integration models.
- Service Providers must build integrations with individual DNS Providers.
- Domain Owners must often configure records manually.
- Complex operations need authorization, status tracking, verification, and failure handling.
- Enterprise processes may require approvals or scheduled execution.
- Agents need a safe and deterministic way to request DNS changes.

Seamless Connect should absorb this coordination complexity without requiring DNS Providers to replace their existing APIs or build new workflow systems.

## Product approach

Seamless Connect provides two request paths.

### Standard Domain Connect

Service Providers can send standard Domain Connect requests with 1:1 protocol compatibility.

These requests use standard Domain Connect templates. Seamless Connect validates the request, obtains authorization, translates the template into DNS Provider API operations, coordinates execution, verifies the result, and returns a Domain Connect-compatible outcome.

A Service Provider using this path should not need Seamless-specific behavior.

### Advanced DNS operations

Service Providers and other authorized initiators can request create, read, update, and delete operations through a Seamless Connect API.

An advanced request may:

- Reference a supported Domain Connect template.
- Reference another approved policy or ruleset.
- Contain explicit non-templated DNS operations when the DNS Provider permits them.

Each DNS Provider decides whether it accepts non-templated requests and may restrict them by operation, record type, domain, initiator, or authorization scope.

## DNS Provider integration

The target integration should be as simple as:

1. An OAuth authorization service.
2. Configuration describing supported capabilities and policies.
3. An adapter to the DNS Provider's existing DNS API.

The DNS Provider remains responsible for:

- Authenticating the Domain Owner.
- Determining what access may be granted.
- Defining supported templates and operations.
- Deciding whether to accept non-templated requests.
- Applying changes through its existing infrastructure.

Seamless Connect remains responsible for:

- Accepting and validating requests.
- Maintaining authorization and delegation context.
- Planning DNS Provider operations.
- Managing synchronous and asynchronous workflows.
- Coordinating related operations.
- Tracking retries and partial outcomes.
- Verifying DNS state.
- Reporting results and retaining operation history.

DNS Providers should not need to build new queues, approval systems, schedulers, callback systems, or asynchronous control planes to participate.

## Conflict handling and synchronous authorization

Seamless Connect does not eliminate the synchronous Domain Connect flow. It may provide the authorization and conflict-resolution experience on behalf of a DNS Provider, reducing the DNS Provider's UI and workflow burden.

Seamless Connect may detect and classify conflicts, present their consequences, and offer resolution options. Conflict detection does not authorize a disruptive change. When a request could replace or disable an existing service, the Domain Owner or an authorized delegate must approve that outcome. A Service Provider request does not provide this authorization.

Deterministic policy may be used for consistent technical handling after the Domain Owner's intent is known. Scoped authorization may permit applying part of a template, but only when the remaining configuration is coherent and the result is reported accurately as partial.

Some DNS Providers may not expose a stable zone view because of behavior such as CNAME flattening. Each DNS Provider integration should therefore describe its supported RR types, visible DNS state, conflict-detection limits, and verification capabilities. Seamless Connect should disclose uncertainty and verify the resulting state rather than present incomplete conflict analysis as definitive.

## Authorization and delegation

Operations may be initiated by:

- Domain Owners.
- Conventional Service Providers.
- Enterprise change-management systems.
- Agents.
- Other authorized delegates.

A mutating operation executes only after the Domain Owner has provided authorization or a valid delegation recognized by the DNS Provider.

Authorization and delegation must be:

- Explicit.
- Limited to defined domains and operations.
- Associated with an identifiable initiator.
- Time-bounded where appropriate.
- Revocable.
- Auditable.

An initiator cannot approve or expand its own authority.

## Synchronous and asynchronous operation

Service Providers may submit requests synchronously or asynchronously.

Synchronous handling is appropriate when authorization, execution, and verification can complete during an interactive session.

Asynchronous handling is appropriate for:

- Enterprise approvals.
- Separation of duties.
- Scheduled change windows.
- Long-running DNS Provider operations.
- Coordinated operations.
- Agents that submit work and monitor completion.

Seamless Connect manages the asynchronous lifecycle even when the DNS Provider exposes only an ordinary synchronous API.

An asynchronous request receives a stable operation identifier and an observable status such as:

- Pending authorization.
- Pending approval.
- Scheduled.
- Applying.
- Verifying.
- Succeeded.
- Partially applied.
- Failed.
- Canceled.
- Expired.
- Outcome unknown.

## Templates and DNS Provider policy

Seamless Connect will have a role in managing the Domain Connect template approval process. Submission, approval, governance, publication, versioning, and deprecation will be covered in a separate product requirements document.

For DNS configuration:

- Standard Domain Connect requests use supported standard templates.
- Advanced requests may also reference supported templates.
- Seamless Connect verifies that the DNS Provider accepts the referenced template.
- A rejected template is not silently converted into a non-templated request.
- DNS Providers choose whether to accept non-templated requests.
- Seamless Connect enforces the DNS Provider's declared policy before requesting authorization or attempting execution.

## Agents

Agents are an important product audience, but their executable requests must remain deterministic.

An Agent request must reference a pre-approved:

- Template.
- Policy.
- Ruleset.

The complete DNS operation must be deterministically derived from that artifact and its permitted inputs.

An Agent may select among authorized operations and supply values within approved constraints. It may not invent new record structures, operations, targets, or authorization scope at runtime.

Free-form model output or reasoning is not itself an executable DNS request.

Every Agent operation must identify:

- The Agent.
- The person or organization represented by the Agent.
- The governing template, policy, or ruleset.
- The delegation authorizing the operation.
- The inputs used to derive the final operation.

## Product principles

- Preserve standard Domain Connect compatibility.
- Conflict detection informs authorization; it does not replace it.
- Seamless Connect may centralize synchronous authorization UX, but it must not eliminate the Domain Owner's decision.
- Handle DNS Provider-specific behavior consistently through declared capabilities.
- Preserve template dependencies and report partial template application accurately.
- Disclose uncertain pre-application state and verify the resulting DNS state.
- Use existing DNS Provider APIs and authorization systems.
- Keep the DNS Provider integration surface small.
- Preserve Domain Owner authority.
- Preserve DNS Provider policy control.
- Support both human and machine initiators.
- Keep Agent execution deterministic.
- Separate templated and non-templated requests clearly.
- Do not silently broaden an authorized request.
- Do not modify unrelated DNS records.
- Verify resulting DNS state before reporting success.
- Report partial or ambiguous outcomes honestly.
- Make every operation attributable and auditable.
- Put asynchronous workflow complexity in Seamless Connect rather than DNS Providers.

## Initial product scope

The initial implementation should demonstrate:

1. A DNS Provider integrating through OAuth and its existing DNS API.
2. A standard Domain Connect request handled with 1:1 compatibility.
3. Translation of a supported template into DNS Provider API operations.
4. Domain Owner authorization with appropriately scoped access.
5. A DNS Provider declaring its policy for non-templated operations.
6. At least one accepted non-templated operation.
7. Rejection of a non-templated operation when DNS Provider policy does not permit it.
8. Synchronous and asynchronous request handling.
9. An enterprise request pending approval or a scheduled change window.
10. An Agent request derived deterministically from a pre-approved artifact.
11. Verification and reporting of successful, failed, partial, and ambiguous outcomes.
12. An operation history showing who requested, authorized, executed, and observed the change.
13. A synchronous conflict flow hosted by Seamless Connect.
14. A conflict in which the Domain Owner chooses whether to replace an existing service.
15. A DNS Provider capability profile covering supported RR types and conflict-detection limitations.
16. Verification for a DNS Provider whose configured and authoritative DNS views may differ.
17. Safe partial application of a template, or rejection when partial application would be invalid.

## Success indicators

The initial product should demonstrate that:

- A DNS Provider can integrate without replacing its existing DNS infrastructure.
- A standard Domain Connect Service Provider can use Seamless Connect without proprietary client behavior.
- Asynchronous workflows do not require new DNS Provider workflow infrastructure.
- Authorized advanced operations can be executed without affecting unrelated DNS data.
- Agent operations remain within their approved policies and delegations.
- Unsupported requests are rejected before execution.
- Successful results reflect verified DNS state.
- Every operation can be explained from its history.

## Open product questions

1. Must every advanced CRUD request conform to a pre-approved template, policy, or ruleset?
2. If direct CRUD requests are permitted, which actors may submit them?
3. What qualifies as a pre-approved policy or ruleset?
4. Who can approve each type of policy or ruleset?
5. Which values may an initiator supply at runtime?
6. How do policy changes or revocation affect pending operations?
7. How should DNS Providers publish their template and non-template capabilities?
8. Which CRUD operations and record types belong in the initial release?
9. What OAuth scopes are needed for domains, records, operations, initiators, and duration?
10. Which operations require interactive authorization, and which may use standing delegation?
11. Which polling, callback, and event mechanisms are needed for asynchronous clients?
12. Which DNS Provider, Service Provider, and template should be used for the initial pilot?
13. Which conflicts always require an explicit Domain Owner decision?
14. Which non-disruptive conflicts may be handled through deterministic policy?
15. When is partial template application valid, and how are record dependencies represented?
16. What result should a Service Provider receive after partial application of a standard Domain Connect template?
17. What minimum DNS state must a DNS Provider expose for reliable conflict detection?
18. How should Seamless Connect behave when no stable pre-application zone view exists?
19. What capabilities and limitations must each DNS Provider integration publish?
