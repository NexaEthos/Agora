# Connect your assistant to Nexa Agora

[Product website](https://www.nexaethos.com/) · [Product tour](https://www.nexaethos.com/product-tour) · [Platform documentation](https://www.nexaethos.com/docs) · [Open your workspace](https://agora.nexaethos.com/)

Your AI assistant can read and act in your Nexa Agora workspace through MCP, the Model Context Protocol. You set it up
in the Nexa Agora web app: there is nothing to clone, download or install from this repository. The web app creates
the connection and shows the exact steps for your assistant, with your server address and client ID filled in.

This guide covers what happens around those steps: what to choose, the instructions to give your assistant, how to
check the connection without changing anything, and how to complete a first piece of work.

## What you need

- A Nexa Agora account with access to the workspace you intend to use.
- An assistant that supports remote MCP servers ([the supported assistants](clients.md)), on a plan its maker
  allows for custom MCP servers.

No local server and no source code are needed. Some assistants take a terminal command or a small settings file of
their own; the web app shows which.

## 1. Create the connection in the web app

Sign in at [agora.nexaethos.com](https://agora.nexaethos.com) to the workspace you intend to use. Open
**Settings › Your external agent**; labels can vary by language. Choose your assistant and where you use it, then how
it signs in:

- **Sign in with Nexa**, when offered: the assistant opens a Nexa sign-in page, and there is no key to handle.
- **A connection key**: the web app shows it once. Keep it in a password manager. Select only the activities the
  assistant needs, and use read-only access for the first check.

Create one connection per assistant, so each one can be paused or revoked on its own.

## 2. Follow the steps the web app shows

The steps are specific to your assistant and already carry your server address and client ID: copy them exactly.
Do not substitute the website address, a local port or an SSE endpoint; the connection uses **Streamable HTTP**.
[Notes on each assistant](clients.md).

## 3. Give your assistant its instructions

The connection grants access; instructions decide how the assistant behaves with it. Copy the
[assistant instructions](assistant-instructions.md) into the place where your assistant keeps standing instructions,
such as a project's custom instructions or its rules. Instructions grant nothing: the authenticated connection does.

## 4. Check without changing anything

Start a new conversation with your assistant and send:

> Use only the connected Nexa Agora tools. List the tools this connection offers and identify my workspace if the
> tools expose it. List a few projects or recent discussions I can access, with their real IDs. Do not create, edit,
> send or delete anything. If the connection is missing or access is denied, say so.

Success means actual tool results, not a description of what the assistant could do. Compare one returned record
with the same workspace in the web app, where your assistant also appears after its first call. Stop if the records
belong to an unexpected workspace. An empty list can be a valid result; it is not evidence of broad access.

## 5. Complete a piece of work

Follow [the discussion workflow](workflows.md): read a request, understand its context, draft a response, save it
when you ask, and find the saved result again.

Only the operations exposed to your connection can be performed. Some activities still need the web app. This guide
does not promise that every screen or function is available through MCP.

## If something fails

See [troubleshooting](troubleshooting.md). The technical rules of the connection are in
[the connection contract](connection.md).

## Verification status

The steps quoted here are the ones the web app's connection screen generates, and the assistants' behaviour was
checked against their makers' documentation on 14 October 2026.

- [x] The web app's connection flow and the remote MCP boundary are documented.
- [x] The supported assistants are listed, with the instructions to give them.
- [x] A read-only connection check and a complete discussion workflow are written.
- [ ] Each listed assistant verified live with a dedicated connection.
- [ ] An explicitly authorized write and its read-back verified live in each assistant.

The unchecked items are live acceptance checks, not completed tests. Each one is recorded with the assistant's
version, the method, the outcome and the date, without credentials or business content.
