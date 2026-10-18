# A complete workflow: request to saved response

Complete the [read-only connection check](README.md#4-check-without-changing-anything) first. Use actual project and
discussion identifiers returned by Agora in place of the descriptive names below.
The prompts are examples of user intent, not permission to run them automatically.

## Read the request

> Find the discussion about the supplier proposal in my project. If there is more
> than one plausible match, show me the choices. Read the selected discussion and
> summarize what is being asked, decisions already made, and outstanding questions.
> Include the discussion ID and links returned by Agora. Do not change anything.

The assistant should search and read the selected record. A title or search snippet
alone is not the full context. If the record cannot be read, it should say so.

## Prepare a response

> Using that discussion and the relevant accessible project documents, draft a
> response addressing the outstanding questions. Separate confirmed information
> from points that need my decision. Keep the draft here in chat.

The draft stays in the chat until you request a write. Ask for revisions there.
The assistant should identify the source records it used and any unavailable context.

## Save the agreed response

If the connection was configured read-only, enable the specific required activity
in the web app before this step and refresh tool discovery in the assistant.

> Post the final response we agreed on to that Agora discussion. Use the discussion
> ID we verified. Read back the saved message and return its ID, the discussion ID,
> and any link Agora provides.

This is an explicit request to publish to the chosen discussion. The assistant
should report a successful write only after the tool confirms it. If the write
succeeds but verification fails, it should distinguish those outcomes. If the
operation times out, it must check for the message before posting again.

## Find the result again

Open the same discussion in the Nexa Agora web app and refresh if necessary. Or ask:

> Read back the message we saved in that discussion and show its content and ID.

The record in Agora is the shared result. Chat history and local drafts are
not a substitute for a saved workspace record.

## Other useful requests

- **Daily review:** “Summarize the unread discussions I can access, with source
  IDs and the ones that require my decision. Do not mark anything read.”
- **Project context:** “Read this project's available tasks and documents. Explain
  its current status and identify missing information without changing records.”
- **Find an answer:** “Search the accessible company documents for this question.
  Cite the returned sources and say when the evidence is insufficient.”
- **Prepare an update:** “Draft a project update using the records we just read.
  Keep it here until I tell you where to save it.”

Each request depends on the tools and permissions actually exposed. If a client
cannot perform an activity, use the web app for that step.
