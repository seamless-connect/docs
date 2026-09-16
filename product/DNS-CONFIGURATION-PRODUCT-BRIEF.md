# Product Brief: DNS Configuration

Status: Draft

## Product vision

Seamless Connect is a coordination service between Service Providers, domain owners, and DNS Providers.

It gives Service Providers a consistent way to request DNS operations while allowing DNS Providers to integrate through their existing APIs and retain control over authorization and execution.

Seamless Connect should support conventional SaaS and cloud services as well as agentic systems acting for domain owners.

## Problem

Service Providers currently rely on a mix of Domain Connect, manual instructions, and provider-specific DNS integrations.

This creates several problems:

- DNS Providers must support different integration models.
- Service Providers must build integrations with individual providers.
- Domain owners must often configure records manually.
- Complex operations need authorization, status tracking, verification, and failure handling.
- Enterprise processes may require approvals or scheduled execution.
- Agentic systems need a safe and deterministic way to request DNS changes.

Seamless Connect should absorb this coordination complexity without requiring DNS Providers to replace their existing APIs or build new workflow systems.

## Product approach

Seamless Connect provides two request paths.

### Standard Domain Connect

Service Providers can send standard Domain Connect requests with 1:1 protocol compatibility.

These requests use standard Domain Connect templates. Seamless Connect validates the request, obtains authorization, translates the template into provider API operations, coordinates execution, verifies the result, and returns a Domain Connect-compatible outcome.

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
3. An adapter to the provider's existing DNS API.

The DNS Provider remains responsible for:

- Authenticating the domain owner.
- Determining what access may be granted.
- Defining supported templates and operations.
- Deciding whether to accept non-templated requests.
- Applying changes through its existing infrastructure.

Seamless Connect remains responsible for:

- Accepting and validating requests.
- Maintaining authorization and delegation context.
- Planning provider operations.
- Managing synchronous and asynchronous workflows.
- Coordinating related operations.
- Tracking retries and partial outcomes.
- Verifying DNS state.
- Reporting results and retaining operation history.

DNS Providers should not need to build new queues, approval systems, schedulers, callback systems, or asynchronous control planes to participate.

## Authorization and delegation

Operations may be initiated by:

- Domain owners.
- Conventional Service Providers.
- Enterprise change-management systems.
- Agentic systems.
- Other authorized delegates.

A mutating operation executes only after the domain owner has provided authorization or a valid delegation recognized by the DNS Provider.

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
- Long-running provider operations.
- Coordinated operations.
- Agentic systems that submit work and monitor completion.

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

## Templates and provider policy

Seamless Connect will have a role in managing the Domain Connect template approval process. Submission, approval, governance, publication, versioning, and deprecation will be covered in a separate product requirements document.

For DNS configuration:

- Standard Domain Connect requests use supported standard templates.
- Advanced requests may also reference supported templates.
- Seamless Connect verifies that the DNS Provider accepts the referenced template.
- A rejected template is not silently converted into a non-templated request.
- DNS Providers choose whether to accept non-templated requests.
- Seamless Connect enforces the provider's declared policy before requesting authorization or attempting execution.

## Agentic systems

Agentic systems are an important product audience, but their executable requests must remain deterministic.

An agentic request must reference a pre-approved:

- Template.
- Policy.
- Ruleset.

The complete DNS operation must be deterministically derived from that artifact and its permitted inputs.

An agent may select among authorized operations and supply values within approved constraints. It may not invent new record structures, operations, targets, or authorization scope at runtime.

Free-form model output or reasoning is not itself an executable DNS request.

Every agentic operation must identify:

- The agent.
- The person or organization represented by the agent.
- The governing template, policy, or ruleset.
- The delegation authorizing the operation.
- The inputs used to derive the final operation.

## Product principles

- Preserve standard Domain Connect compatibility.
- Use existing DNS Provider APIs and authorization systems.
- Keep the DNS Provider integration surface small.
- Preserve domain-owner authority.
- Preserve DNS Provider policy control.
- Support both human and machine initiators.
- Keep agentic execution deterministic.
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
3. Translation of a supported template into provider API operations.
4. Domain-owner authorization with appropriately scoped access.
5. A DNS Provider declaring its policy for non-templated operations.
6. At least one accepted non-templated operation.
7. Rejection of a non-templated operation when provider policy does not permit it.
8. Synchronous and asynchronous request handling.
9. An enterprise request pending approval or a scheduled change window.
10. An agentic request derived deterministically from a pre-approved artifact.
11. Verification and reporting of successful, failed, partial, and ambiguous outcomes.
12. An operation history showing who requested, authorized, executed, and observed the change.

## Success indicators

The initial product should demonstrate that:

- A DNS Provider can integrate without replacing its existing DNS infrastructure.
- A standard Domain Connect Service Provider can use Seamless Connect without proprietary client behavior.
- Asynchronous workflows do not require new DNS Provider workflow infrastructure.
- Authorized advanced operations can be executed without affecting unrelated DNS data.
- Agentic operations remain within their approved policies and delegations.
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
