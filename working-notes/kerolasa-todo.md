# Introduction

This is Sami Kerola's very rough technical plan / todo list.  This markup
file exists to keep track of what might be good idea to implement.  This is
not a spec, but an ideas dumping groud with strong emphasis on everything
that is said in this document is subject of reconsideration, debate, and
moving into graveyard of ideas.

# Seamless components

## ISC Bind and/or unbound

We should assume people run a standard DNS service such as ISC Bind or
Unbound by NL Netlabs.  The code making Domain Connect to work ought to
demonstrate how these components can be implemented to work Domain Connect
compliant ways without requiring anything else but configuration changes.

The anticipated configuration changes will enable Dynamic DNS (DDNS)
updates.  Domain Connect demo service will need to inter-act with the DDNS
interface.

## Domain Connect Service Provider

Golang version of the Service Provider (SP) would be nice.  The
implementation needs to be split the way that the SP functionality is
separate from web styling.  We want to have as close to a drop-in service
provider implementation as possible.

Implementation needs to be able to use both sync and async (oauth)
interfaces.

## Domain Connect DNS Provider

Golang version of the DNS Provider (DP) would be nice.  The implementation
will need to have

1. Be able to receive update request, that knows what to do with
   1. The Service Provider identifier
   2. Template in question
   3. Template variables
   4. And update validation signature
2. Implement both sync and async update interfaces
3. Have configuration for
   1. Which templates the DP supports
   2. OAuth config for async interface
   3. DNS Provider API url
   4. DNS Provider settings content

## Seamless Service

The Domain Connect as service.  Aimed for people and bots who want to be
able to effect DNS Providers with minimal effort. See the
[Domain Connect Enhancements for Machine-Native DNS Operations](Working Proposal: Domain Connect Enhancements for Machine-Native DNS Operations.md)
for Brian Toresdahl's write up what is needed.

On top of the Brian's requirements I would add and/or clarify.  The Seamless
Service will need to

1. Be able to make Service Provider requests
2. Have a mechanism to coordinate multiple DNS Providers (DP) to perform an
   update that together form the update as whole
3. Validate the update actually happen successfully
4. Be a job queue, because
   1. DP updates can take a while, when including change validation
   2. Coordinating multiple DPs can take unpredictable amount of time
5. Because failure is always possibility the service needs to also
   1. Track update state
   2. Be able to rollback partial apply on a partial failure
6. Since rollbacks are required
   1. People need to have a way to rollback a change entirely
   2. Rollback retention period must be long enough (such as 10 years, or
      preferrably forever)
   3. Word 'people' refers to domain owners, Service Provider admins, DNS
      Provider admins, and Seamless Admins
7. Transaction log
   1. Contains requests (failed or not)
   2. Verification results (failed or not)
   3. Automation status changes (such as rollback on timeout, or retries)
   4. One must not need anything else but looking the transaction log when
      explaining what happen.
   5. The log is append only.
8. Transaction log must be re-playable
   1. Re-play will generate new transactions
   2. Transactions from different Seamless Service instances can be merged
      (to a limit)
9. Strong authentication and authorization
   1. Both people and bots must use strong identification. In short only
      webauthn is allowed.
   2. All Seamless logins belong to: organization units, and groups
   3. Access controls are determined in two levels
      1. What organizations are allowed to do
      2. What organization allows groups to do
   4. Operating system access is at least as strict as application access
10. OpenTelemetry support
   1. Or at least end-to-end Request ID support
