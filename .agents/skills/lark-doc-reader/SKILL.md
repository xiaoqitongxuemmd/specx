---
name: lark-doc-reader
description: Use when reading Feishu/Lark wiki or document links for the user through lark-cli. If lark-cli is missing, unconfigured, or user authorization is invalid, execute the local CLI setup step by step, pause only for required user/browser interaction, then retry the read.
---

# Lark Doc Reader

## Purpose

Read Feishu/Lark wiki and document links through `lark-cli`, with explicit handling for missing CLI setup, expired user authorization, and wiki-to-doc resolution.

Use this skill when the user asks to read, summarize, inspect, or use content from a Feishu/Lark wiki, docx, doc, or Base link.

## Workflow

1. Extract the document token from the URL.
   - Wiki URLs usually contain the node token after `/wiki/`.
   - Docx URLs usually contain the document token after `/docx/`.
   - Base URLs usually contain the app token after `/base/`.

2. Verify local `lark-cli` availability before reading.
   - Check `lark-cli --help`, `lark-cli config --help`, or `lark-cli auth status`.
   - If `lark-cli` is missing, broken, or clearly unconfigured, do not stop at generic guidance. Execute the setup steps directly when the environment permits.
   - If a command fails because of sandbox or network restrictions, rerun it with escalation approval instead of asking the user to execute it manually.
   - Do not ask the user to paste document content until the local CLI path has been attempted or the user declines further setup.

3. If local `lark-cli` is not ready, execute setup step by step.
   - Verify whether `npm` is available.
   - If `lark-cli` is missing and `npm` exists, install it directly:
     - `npm install -g @larksuite/cli@1.0.39`
   - If `npm` is missing, stop and tell the user that Node.js/npm is the blocker.
   - If the CLI is installed but not configured, run:
     - `lark-cli config init --new`
   - If `config init` requires app information or an interactive choice the agent cannot safely infer, ask only for that missing input.
   - Ensure the app security settings include:
     - `http://localhost:3000/callback`
   - Prefer to inspect current state before asking the user to change anything.
   - For user authorization, first try:
     - `lark-cli auth login --recommend --no-wait --json`
   - Show the returned browser verification URL exactly as printed and ask the user to complete authorization.
   - If the environment supports blocking interactive login and that is clearly simpler, `lark-cli auth login --recommend` is acceptable.
   - After the user finishes authorization, confirm the CLI state with:
     - `lark-cli auth status`
   - The expected state is:
     - `identity: user`
     - `tokenStatus: valid`
     - `identities.user.status: ready`
   - If this was the initial successful local setup for `lark-cli` in the current environment, deploy this skill into the user's Codex skill directory after auth/config succeeds:
     - target directory: `/home/mini/.codex/skills/lark-doc-reader`
     - create the directory if needed
     - deploy a sanitized copy of this skill there
   - The deployed copy in `/home/mini/.codex/skills/lark-doc-reader` must not include this self-deploy instruction.
   - To keep the deployed copy clean, remove the self-deploy bullets from the copied `SKILL.md` before writing it to the user skill directory. Do not recursively copy a version that would redeploy itself again.

4. Use `lark-cli` to read the document.
   - For wiki links, first resolve the wiki node:
     - `lark-cli wiki +node-get --as user --token '<wiki-url-or-token>' --format json`
   - Extract `obj_token` and `obj_type` from the returned node data.
   - For `docx` objects, fetch document content:
     - `lark-cli docs +fetch --as user --api-version v2 --doc '<obj_token>' --format json`
   - For direct doc/docx URLs, fetch them with `lark-cli docs +fetch`.
   - Start with a single read attempt that matches the document type. Do not fan out multiple probes in parallel before authentication is confirmed.

5. If `lark-cli` shows missing or expired user authorization:
   - Prefer `lark-cli auth status` to confirm the current state.
   - If needed, run `lark-cli auth login --recommend --no-wait --json` first, because it is easier to continue from a non-blocking terminal flow.
   - Use `lark-cli auth login --recommend` only when the environment can wait on the login flow cleanly.
   - If the CLI prints a browser verification URL, copy it into the conversation directly.
   - Ask the user to confirm after the browser-side authorization completes, then retry the original read.

6. If authorization succeeds, retry the original read request and continue with the user's requested task.

## Rules

- Treat local CLI setup and user authorization as operational prerequisites, not as document-reading failures.
- Default to doing the setup work yourself with tools, not narrating manual steps for the user to run.
- Pause only when a browser login, app-console action, secret, or ambiguous interactive choice requires the user.
- Keep authorization under user control. Do not ask for passwords, secrets, or pasted tokens.
- Treat deployment into `/home/mini/.codex/skills/lark-doc-reader` as a post-setup action only. Do not overwrite that user-copy during ordinary document reads when setup was already complete.
- Do not claim the document was read until `lark-cli` returns node metadata or document content.
- If the user lacks permission to the document after valid authorization, say so and ask them to grant access or provide the content another way.
- When the document content is used as source material for a backend spec, treat it as evidence, not automatically correct requirements.
- When installation, auth, or document fetch commands fail due to sandbox or network restrictions, rerun them with escalation approval rather than stopping at a diagnosis.

## Troubleshooting

| Symptom | Likely Cause | Action |
|---|---|---|
| `lark-cli: command not found` | CLI not installed | install `@larksuite/cli@1.0.39` directly; if blocked by sandbox or network, rerun with escalation |
| `needs_refresh` / invalid user token | user token expired or missing | rerun `lark-cli auth login --recommend` |
| redirect URL rejected | app security callback not configured | add `http://localhost:3000/callback` in app security settings |
| app credentials not found | CLI config not initialized or wrong profile selected | rerun `lark-cli config init --new` or inspect `lark-cli config show` |
| wiki node resolves but content fetch fails | need underlying `obj_token` and `obj_type` | read wiki node first, then call `docs +fetch` with the resolved token |
| sandbox blocks network | CLI can work locally but agent sandbox cannot reach Feishu | rerun the needed `lark-cli` command outside the sandbox with user approval |
