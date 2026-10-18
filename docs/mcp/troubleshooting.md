# Troubleshooting

| Symptom | What to check |
| --- | --- |
| No Agora tools | Configure the assistant hosting this chat, not another app or extension. Start or enable the server, refresh discovery, and open a new session. |
| Signed in to the web app but chat has no access | Signing in to the web app does not connect your assistant. Create the connection in Settings › Your external agent and follow its steps. |
| 401 / unauthorized | Confirm the connection key is current and reaches the assistant's app, or repeat Sign in with Nexa. Never print the key to diagnose it. |
| Sign-in or redirect error | Use the exact client ID and callback shown by the web app's steps. Check the assistant's version. |
| 403 / operation denied | Check your access and the connection's activities in the web app. A different transport does not grant permission. |
| URL, transport, or connection failure | Copy the full server address from the web app's steps, select Streamable HTTP, and check network access. Do not use SSE, the website home page, or a guessed localhost address. |
| Connection works but a tool is missing | Compare the live tool catalog and connection permissions. Refresh discovery after changes; do not assume UI parity. |
| No records or wrong records | Confirm the intended workspace and access using read-only tools and the web app. Do not write until the target is clear. |
| Key works in a terminal, not in the assistant's app | Quit the app completely and open it again where the variable is set. An app already running keeps its old environment. |
| A write timed out | Read the destination to establish whether it completed before retrying. Report uncertainty if it cannot be resolved. |
| Draft is absent from Agora | A chat draft is not saved. Explicitly request saving to a verified destination and inspect the tool result. |

To disconnect, remove the server from the assistant and revoke the connection in
the web app. Removing a local setting alone does not revoke a key. If a key was committed or shared, revoke it immediately and replace it;
deleting the latest copy does not remove it from Git history.

When reporting an issue, include the assistant and its version, OS, sign-in method,
sanitized error, and whether the read-only check worked. Exclude tokens and
private record content. A successful setup in one assistant does not prove another
assistant has been verified.
