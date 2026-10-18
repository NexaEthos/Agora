# Trust: how Nexa Agora keeps agents governed

Many agent products write a careful prompt, hand the model the keys and hope it behaves. Nexa Agora does not work
that way. **The model proposes; the platform decides what may happen.** Isolation, permissions, approvals and checks
are enforced by the database and the server, not asked of the model, so they still hold when a model is wrong,
confused or manipulated.

This page describes what is built and running today. Last reviewed: 18 October 2026.

## Workspaces are isolated in the database

- **Row-level security on every workspace table.** More than a hundred tables carry PostgreSQL row-level security
  with one policy shape: a row is visible and writable only when it belongs to the workspace bound to the current
  transaction. The policy fails closed: with no workspace bound, no row is visible.
- **The workspace is bound per transaction.** It is set at the start of each database transaction and ends with it,
  so a pooled connection cannot carry one workspace's context into another's request.
- **The application cannot bypass the policy.** It connects with a role that has no `BYPASSRLS`, no `CREATEDB` and
  no `CREATEROLE`. The few platform tasks that must read across workspaces, such as erasing an account or finding an
  invitation by its unguessable token, are listed by name, and a check in continuous integration fails if any other
  code opens that path.
- **It refuses to start in an unsafe state.** At boot the server checks that every workspace table has its policy
  enabled and forced, and that no table holding workspace data escaped the policy. In production it also refuses to
  start with a wildcard CORS policy, exposed API docs or any isolation switch turned off.
- **Agents' database tools carry a second fence.** When an agent reads or writes data directly, every statement it
  builds carries the workspace condition, and platform tables are refused.
- **The vector store is partitioned the same way.** Agents' memory, the knowledge bases and the Company Brain live in
  Qdrant, where every collection is partitioned by workspace. The partition is enforced at one point for every kind
  of access: a search or a scan without it is refused, a read by identifier is filtered to the workspace, and a write
  or a delete carries the partition with the identifier.

## Roles and permissions

- **Owner, admin, member.** A person's role belongs to their membership of a workspace, never to a global flag.
- **Platform powers stay with the platform.** Permissions that operate the service itself can never be granted to a
  customer, not even by the customer's own admin. The people who operate the service are invited for that purpose
  and hold no seat in customer workspaces.
- **Every permission is enforced.** A check in continuous integration fails if a permission exists that no server
  gate consults, or if a gate consults a permission that does not exist.
- **An agent never holds more than its person.** A personal agent's permissions are recomputed on every turn as a
  subset of its owner's.
- **Private means private.** A Private document is invisible to admins and owners as well: listings, search and
  restore exclude it, and the storage view shows counts and sizes, never titles.
- **Mailboxes follow visibility and grants, not rank.** A private mailbox belongs to its owner. A shared one belongs
  to the people granted it. In an organisation mailbox members can read, and acting on it takes a grant.

## People approve before agents act

- **Approval cards for real power.** Sending an email, deleting a document, pruning memory and promoting knowledge
  into the Company Brain go through a card that a person approves. Only the platform can raise these cards, and it
  checks the approver's authority again at the moment of the decision.
- **The bytes you approve are the bytes that leave.** An approved email is stored as the exact message the person
  saw and sent from that copy, never from a new draft. A send is claimed once, and an uncertain outcome is
  reconciled rather than sent again.
- **Agents' email has guard rails.** It is off by default. Recipients come from the owner's double opt-in list,
  chosen by identifier: an agent cannot type in a new address.
- **Code changes are proposals.** Agents propose changes to GitHub; a person approves them before anything is
  written.
- **Stopping means stopping.** When a project ends, its running agent turn stops at the next step, and the stop is
  recorded.

## Checks that do not depend on a prompt

- **A deliverable never claims a review it did not get.** A reviewer distinct from the author, a person or an
  agent, judges every deliverable against the project's criteria. A criterion the reviewer found unmet, or could not
  verify, is declared to the client with the delivery, in the project's own words: never hidden, never presented as
  passed.
- **Definition of done, checked by code.** The first tier of the done check is plain code with no model involved,
  and production enforces it together with the handover and grounded-citation checks.
- **Citations must be grounded.** Research cites immutable source records, and a citation that points to no source
  record blocks the result.
- **Models come only from the workspace's policy.** No setting or default can slip in a model. The services that
  produce work require a certified model. When no certified model is available, the request is refused with the
  reason and the way out.
- **Spending has a wall.** Every model call and paid service paid for by the platform meets the workspace's credit
  allowance first. When the allowance is used up the request is refused with the reason in words, not answered
  with a degraded result.
- **Refusals name the way out.** A refused request returns a code and a sentence that says what to do next.
- **Interactive content runs in a sandbox.** Interactive Playbooks surfaces run in an isolated browser frame with
  no network access, and the server never executes their code.

## Every step is on the record

- **A tamper-evident audit log.** Audit entries are chained per workspace with SHA-256: each entry's hash covers
  the previous one and every field of its own. Anyone with audit rights can ask the server to verify the chain.
- **Who did it: a person, an agent, or both.** Entries record the human and the agent behind an action, and a
  recorded entry is never rewritten.
- **Agent turns are traced.** The instructions sent, the tools called, tokens, timing and the done verdict are kept
  for 180 days. A person who purges a trace leaves an audit entry behind.
- **Provenance travels with the work.** Documents record whether a person or an agent created them and where they
  came from.

## Your own assistant, through MCP

- **It acts as you, never more.** An assistant connected through MCP works with your permissions. A connection can
  narrow them, with read-only as the default and a tool allowlist, but never widen them. Revoking a connection
  takes effect immediately.
- **Destructive tools are off until you turn them on.** Tools that delete or overwrite are refused for a connection
  until its owner enables them, and the switch is off by default.
- **Operator tools are not even listed.** Tools reserved for the people who operate the service are hidden from
  customer connections.
- **Posts are labelled.** A message an assistant posts through MCP is marked with the connection it came through.

## Sign-in and sessions

- **Two-factor sign-in.** Authenticator apps (TOTP) come with single-use recovery codes, and admins must enrol.
  Passkeys work as a second factor or for passwordless sign-in with user verification.
- **Sessions you can end.** Every token is tied to a live session. Refresh tokens rotate, and reusing an old one
  ends the session.
- **Sign-ins bound to the tab.** Third-party sign-in carries a per-tab verifier, so a stolen redirect cannot be
  replayed in another browser.

## Data protection and the GDPR

- **Your data, on request.** A person can download their own data (articles 15 and 20) and erase their account
  (article 17). Erasure reaches every workspace that holds their data and leaves a pseudonymised record.
- **Erasing a workspace erases it.** Every isolated table, the vectors and the files are removed, and a receipt of
  the erasure, without content, is kept for ten years. Agent turn traces and backups leave at the end of their own
  windows.
- **Retention is enforced by code, not by a policy page.** Windows in force today:

  | Data | Kept |
  | --- | --- |
  | Audit log | 365 days |
  | Agent turn traces | 180 days |
  | Sign-in events | 365 days |
  | Agora activity feed | 90 days |
  | Operational logs, finished jobs, cards and invitations | 30 days |
  | Deleted documents (bin) | 30 days |

- **Hosted in the EU.** Production runs in France across three availability zones, and the disaster-recovery copy is
  in Sweden.
- **You see where a model processes data.** Every model declares whether it processes data in the EU, on sovereign
  infrastructure or globally, and a model without that declaration cannot be enabled. The choice is visible; the
  platform warns, and the workspace decides.
- **Encryption.** Connections use TLS, with TLS 1.3 and full certificate verification to the database. Connector
  credentials, OAuth tokens and two-factor seeds are encrypted by the application with a rotating key ring before
  they are stored. Files and vectors sit on encrypted storage.

## Built for the EU AI Act

The AI Act's duties that fall on the platform are built in:

- **AI output is marked, in a machine-readable form (article 50(2)).** One disclosure, `ai-generated;
  platform=nexa-agora; human-governed=true`, travels with:
  - emails, as a header and a footer;
  - HTML, Word, Excel and Markdown exports;
  - PDF and image renders;
  - share links;
  - replies in Discord, Telegram and WhatsApp.
- **Agents are visibly agents.** Every message and document records whether a person or an agent wrote it.
- **Logging and human oversight** (articles 12 and 14) are the audit chain, the traces, the approval cards and the
  publication and done checks described above.

The formal documentation, the risk classification and the transparency notice, is being finalised. This page
states the mechanisms, not a certification.

## Nexa Bridge

- **Outbound only.** The bridge dials out: no public address, no open port, no inbound firewall rule.
- **Owner-approved pairing.** A computer or device pairs through a single-use code that a workspace owner or admin
  approves in the browser. The server keeps only a SHA-256 of each device's credential, and the desktop app keeps
  its own in the operating system's credential store.
- **Physical endpoints observe, they do not act.** Lab boards and robots send observations; they take no commands.

## How we test it, and how to report a problem

- **Every change meets the conformity suite.** More than 700 test files, including the isolation, permission,
  approval and erasure guards above, block every change.
- **Security scans run on every change and weekly:** static analysis, dependency audit, secret scanning of the full
  history, container image scanning that blocks high and critical findings, and an SBOM.
- **Adversarial testing.** Our internal security campaign probes isolation, sessions, OAuth, share links and
  server-side request forgery on dedicated test workspaces. It is an internal assessment, not an independent
  third-party penetration test.
- **Report a vulnerability privately** as described in [SECURITY.md](../SECURITY.md), never in a public issue.
