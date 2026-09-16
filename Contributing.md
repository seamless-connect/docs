# Seamless DNS Contribution Guide

This guide explains how open-source work on Seamless DNS is conducted.

## Terminology

Use explicit actor names in documentation, issues, designs, diagrams, examples, and user-facing text.

Do not use **provider** or **providers** alone when referring to a project actor. Several actors provide services in different contexts, so the unqualified term is ambiguous.

Use the complete role name:

- **DNS Provider** for the operator of authoritative DNS service.
- **Service Provider** for a service requesting domain or DNS configuration.
- **Registrar** for the sponsoring registrar of a domain registration.
- **Registry** for the operator of a registration data repository and authoritative registration system.
- **Agent** for an agentic system acting for a person or organization.
- **Domain Owner** for the person or organization that controls a domain.

Use the explicit role in compound terms as well. Write **DNS Provider API**, **DNS Provider policy**, and **Service Provider request**, not **provider API**, **provider policy**, or **provider request**.

When a statement applies to multiple actor types, name them or define a clear collective term for that context, such as **participants** or **integrated systems**. Do not use **providers** as the collective term.

Capitalize these role names consistently when they refer to actors in the Seamless Connect system.

## Proposing a Feature

The project has a substantial backlog. New feature proposals should be submitted via GitHub Issues using the **New Feature** label. A strong feature request answers the following questions:

1. What does the feature do?
2. Who or what uses the feature?
   * Does a downstream or upstream user already exist?
3. How can the feature be tested *automatically* so the project can:
   * Confirm the feature is implemented correctly.
   * Ensure future changes do not cause regressions.

## Reporting and Fixing Issues

Resolving bugs takes priority over adding new features to avoid building on top of broken behavior.

When an issue is identified, create a GitHub ticket. A good issue report includes:

1. A clear description of what is wrong.
2. Steps to reproduce the issue.
3. An optional proposed fix.

Commits that fix an issue must reference the corresponding ticket in their commit message.

### Security Vulnerabilities

Please report security issues directly to Sami Kerola (`kerolasa@gmail.com`) and Pawel Kowalik (`pawel.kowalik@denic.de`). They will create a standard issue ticket, after which the normal bug-fixing process applies.

## Contribution Process

The `main` branch of all Seamless DNS projects must remain in a production-ready state at every commit. The only exception is within a multi-commit pull request, where intermediate commits might temporarily break builds.

Submit changes to maintainers via GitHub pull requests. Maintainers are not required to accept pull requests as-is. You may be asked to modify your code or improve related documentation, such as commit messages or inline comments.

Large contributions must be split into clear, readable commits. Maintainers will inform you if splitting is required and provide guidance on how to restructure your contribution.

Maintainers may amend a contribution before merging it. In these cases, maintainers may add a `Co-authored-by:` line, but they will not take primary authorship unless the contribution was almost entirely rewritten (in which case the original author will still receive credit for the idea).

Major features and bug fixes require corresponding issue tickets. Minor changes-such as typo fixes and minor refactoring-do not require a ticket.

### Releases

Releases occur on an as-needed basis (e.g., to ship a critical bug fix, complete a major feature, or consolidate small improvements). You can verify releases using GPG, SHA-256, and SHA-512 signatures.

All releases must support reproducible builds. Any change that breaks reproducible builds is treated as a security issue.

### Breaking Changes

Backwards compatibility is a high priority. Avoid breaking changes whenever possible. The only exceptions are:

* Security issues that strictly require a breaking change.
* Logically impossible scenarios where breaking compatibility is the only path forward.

Minor bugs that require a breaking change to fix will be treated as unintended features and left as-is. Clear documentation and warning messages regarding these quirks are appreciated.

### Licensing

Seamless DNS uses the following licenses:

* **Source Code:** Apache-2.0
* **Documentation:** CC-BY-4.0
* **Specifications:** Community-Spec-1.0
* **Data:** CDLA-Permissive-2.0

## Role of AI

Contributions must originate from human maintainers and contributors. Signing off on a commit indicates that you accept full responsibility for the change, regardless of whether AI tools were used. This responsibility includes ensuring the legal validity of authorship and licensing.

In short: using AI as an assistant is acceptable, but letting AI drive the development process is not.

Fully AI-generated spam—including low-quality feature requests, issue reports, security reports, or code changes—will be closed immediately without discussion.
