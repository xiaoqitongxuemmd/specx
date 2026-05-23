---
name: lark-doc-reader
description: Use when reading Feishu/Lark wiki or document links for the user through lark-cli. If lark-cli is missing, unconfigured, or user authorization is invalid, guide the user to fix the local CLI setup and then retry the read.
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
   - If `lark-cli` is missing, broken, or clearly unconfigured, switch into local CLI setup guidance instead of trying to read the document.
   - Do not ask the user to paste document content until the local CLI path has been attempted or the user chooses not to configure it.

3. If local `lark-cli` is not ready, guide the user to configure it.
   - Verify the CLI is installed.
   - If needed, initialize a local app profile:
     - `lark-cli config init --new`
   - Ensure the app security settings include:
     - `http://localhost:3000/callback`
   - Log in with:
     - `lark-cli auth login --recommend`
   - If the harness cannot block for browser login, use:
     - `lark-cli auth login --recommend --no-wait --json`
   - Show the returned browser verification URL exactly as printed.
   - After authorization, confirm the CLI state with:
     - `lark-cli auth status`
   - The expected state is:
     - `identity: user`
     - `tokenStatus: valid`
     - `identities.user.status: ready`

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
   - If needed, run `lark-cli auth login --recommend`.
   - If the CLI prints a browser verification URL, copy it into the conversation directly.
   - Tell the user the command may block while waiting for authorization, and ask them to confirm after completion before retrying the original read.

6. If authorization succeeds, retry the original read request and continue with the user's requested task.

## Rules

- Treat local CLI setup and user authorization as operational prerequisites, not as document-reading failures.
- Keep authorization under user control. Do not ask for passwords, secrets, or pasted tokens.
- Do not claim the document was read until `lark-cli` returns node metadata or document content.
- If the user lacks permission to the document after valid authorization, say so and ask them to grant access or provide the content another way.
- When the document content is used as source material for a backend spec, treat it as evidence, not automatically correct requirements.

## Troubleshooting

| Symptom | Likely Cause | Action |
|---|---|---|
| `lark-cli: command not found` | CLI not installed | ask the user to install `lark-cli` first |
| `needs_refresh` / invalid user token | user token expired or missing | rerun `lark-cli auth login --recommend` |
| redirect URL rejected | app security callback not configured | add `http://localhost:3000/callback` in app security settings |
| app credentials not found | CLI config not initialized or wrong profile selected | rerun `lark-cli config init --new` or inspect `lark-cli config show` |
| wiki node resolves but content fetch fails | need underlying `obj_token` and `obj_type` | read wiki node first, then call `docs +fetch` with the resolved token |
| sandbox blocks network | CLI can work locally but agent sandbox cannot reach Feishu | rerun the needed `lark-cli` command outside the sandbox with user approval |
