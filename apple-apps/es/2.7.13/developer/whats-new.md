
# What's New

Recent architectural and procedural changes from roughly the last 12 months. Newest at the top.

<!-- DEV_WHATS_NEW_START -->
<!-- Add new entries at the top. Format:
**Month YYYY** — [Page or area](relative/path.md) — One sentence on what changed architecturally or procedurally.
Show roughly the last 12 months of changes; archive entries older than a year by removing them.
-->

**May 2026** — [Deep Links](deep-links.md) — Added `audio` and `neighborInfo` deep links for new module config screens.

**May 2026** — [Architecture](architecture.md) — Audio, Neighbor Info module config screens; Pax Counter threshold fields; Compass Orientation picker; `IntervalConfiguration.neighborInfo` enum case for update interval picker.

**May 2026** — [Architecture](architecture.md) — Docs Translation Pipeline (`009`): markdown-level translation with community CDN feed, manifest-based caching, and automatic contribution back to `meshtastic/translations` repo.

**May 2026** — [Architecture](architecture.md) — Automatic Docs Translation (`008`): on-device Apple Translation framework integration for in-app docs, with file-based cache in Application Support.

**May 2026** — [Architecture](architecture.md) — Message Formatting Toolbar (`004`): pure SwiftUI markdown toolbar using `TextSelection` (iOS 18+), raw markdown storage in existing `messagePayload` field — no schema changes.

**May 2026** — [SwiftData](swiftdata.md) — Documented save strategy (autosave disabled, debounced saves), `@Attribute(.unique)` indexes, and data caps for positions/telemetry/messages. Fixed stale `QueryCoreData`/`UpdateCoreData` references.

**May 2026** — [CarPlay](carplay.md) — Documented fetch limits and predicates on CarPlay data queries.

**May 2026** — [Deep Links](deep-links.md) — Added `coreDataBrowser` deep link for the SwiftData database browser.

**May 2026** — [Testing](testing.md) — Snapshot test conventions established: consolidated multi-state views into single combined images (light + dark pairs), use `assertViewSnapshot` helper with explicit `width`/`height` and `transparent: true` for icon snapshots.

**May 2026** — [Architecture](architecture.md) — In-app documentation system added (`003-app-docs-markdown`): markdown source under `docs/user/` and `docs/developer/` is converted to HTML by `scripts/build-docs.sh` and bundled at `Meshtastic/Resources/docs/`.

**Apr 2026** — [Transport](transport.md) — Documented AccessoryManager transport extensions and connection lifecycle.

**Mar 2026** — [SwiftData](swiftdata.md) — Initial SwiftData developer guide: ModelContainer setup, `@Query` usage, `MeshPackets` actor, schema migrations.
<!-- DEV_WHATS_NEW_END -->

