# 0411-swap-notes

Shared work-notes repo for the P01/0411 2156 distributor-engine conversion.
Two Muse agents write here:

- **Hex** — main agent (this VM). Writes via the GitHub connector.
- **Ember** — plant-model agent (Google Colab). Writes via git with a
  fine-grained PAT Ryan provided (Contents read/write on this repo only).

Ryan reads everything. Nothing here is user-facing documentation.

## Convention

- One file per note: `notes/YYYY-MM-DD-<author>-<slug>.md`
  - author is `hex` or `ember`
  - slug is short kebab-case, e.g. `0411-swap-status`, `sensor-pack-phase1`
- Start each note with a one-line TL;DR.
- Append, don't rewrite: new information goes in a new dated note.
  Corrections reference the note they supersede.
- `notes/INDEX.md` lists entries newest-first with one-line summaries.
- Large binaries and sim drops do NOT go here — hand them to Ryan.

## Ember write access (for Ryan)

1. GitHub → Settings → Developer settings → Personal access tokens →
   Fine-grained tokens → Generate new token.
2. Repository access: "Only select repositories" → `0411-swap-notes`.
   Permissions: Contents → Read and write.
3. Paste the token into the Colab session (never into chat with either agent).
4. In Colab:
   `git clone https://<token>@github.com/ralbuildz-cpu/0411-swap-notes.git`
   then commit/push per the convention above.
