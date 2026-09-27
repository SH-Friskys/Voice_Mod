# Local issue tracker

Markdown-file issue tracker used by the wayfinder planning process.

- One issue per file: `NNN-slug.md`. The number is the issue id.
- Front matter fields: `id`, `title`, `labels`, `parent`, `status` (`open`/`closed`), `assignee`, `blocked_by` (list of ids).
- **Map**: the issue labelled `wayfinder:map` (`000-map.md`). Tickets are issues whose `parent` is the map's id.
- **Claim**: set `assignee` before starting work. Open + no assignee = unclaimed.
- **Blocked**: a ticket is unblocked when every id in `blocked_by` has `status: closed`.
- **Frontier**: open, unblocked, unclaimed children of the map.
- **Resolve**: append a `## Resolution` section (the resolution comment), set `status: closed`, and add a line to the map's *Decisions so far*.
- **Research findings**: each research ticket's findings live in `research/<slug>.md` on the throwaway branch `research/<slug>`.
