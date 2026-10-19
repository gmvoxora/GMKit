# GMKits · GemeZ

**Your kits. Reliable claims. Complete control.**

GMKits `0.1.0-rc.3` is a runnable release candidate with an English-first interface and twelve included language catalogs. It is built separately from GMAlive.

## Install

Use **one** JAR from `dist/0.1.0-rc.3/`:

| Artifact | Intended server range | Evidence |
|---|---|---|
| Legacy | 1.8.8–1.12.2 | Spigot 1.8.8 compilation; core tests on Java 8 and 21 |
| Modern | 1.13–26.3 | Actual JAR exercised with MockBukkit/Paper API 1.21.1 on Java 21 |

These are target ranges, not a completed live compatibility matrix. CraftBukkit, Spigot, Paper, Purpur and Folia need real-server checks before public compatibility claims. Folia scheduling is implemented; its certification is pending. Read [COMPATIBILITY.md](COMPATIBILITY.md).

Place the selected JAR in `plugins/`, start the server, then use `/kit`. New installations contain a public `adventure` example kit. Ordinary kits keep complete overflow packages in `/kit claimbox`.

**First-join equipment is an independent, private module in `starter.yml`.** A genuinely new server player automatically receives vanilla supplies once, without a menu entry, preview, autocomplete, claim command, chat message, title, sound or inbox entry. Full inventory means a quiet automatic retry; existing items are preserved. The receipt persists across reconnects, restarts and season changes. Default eligibility is `NEW_PLAYERS`; optional `FIRST_PLUGIN_JOIN` includes accounts previously known to Minecraft but not yet recorded by GMKits. Previously recorded GMKits accounts are never retroactively classified as new. See [UPGRADE.md](UPGRADE.md).

English (`en_US`) is default and fallback. Client detection starts disabled. Included: English, Polish, German, Spanish, French, Brazilian Portuguese, Italian, Dutch, Czech, Ukrainian, Turkish and Simplified Chinese. Select `/kit language pl_PL` or use the language menu. Catalog keys and placeholders are verified; native-speaker editorial review remains advisable before publication.

## Included

- Virtual command rewards with configurable preview icons, translated type labels and no physical icon delivery. Mixed and command-only kits use the existing command journal. See [COMMAND_REWARDS_PL.md](COMMAND_REWARDS_PL.md) and the commented `content.command-rewards` example in `kits/adventure.yml`.
- Chest menus with categories, favorites, private search, sorting, pagination, status icons, countdowns and preserved filters. Gold, emerald, ice and custom interface colors.
- Protected previews, armor/offhand display, reward information, configurable slots and confirmations. Clicks, shifts, number keys, dragging and stale views cannot extract menu items.
- In-game editor for names, descriptions, cooldowns, limits, prices, permissions and delivery. Contents can be copied with the cursor, drag or shift-click, undone and explicitly saved; original player items stay intact. Private input, stale-view protection, atomic saves and history.
- Native inventory capture on compatible Paper or an installed compatible NBT-API. Capture copies items without consuming them. Data must survive a same-runtime round trip before saving.
- Permission/world/level/prerequisite checks; seasonal/lifetime limits; rolling and daily/weekly calendar cooldowns with explicit DST behavior.
- Free kits, XP points/levels and optional Vault/PlayerPoints. Per-claim payment or access purchase. Purchased access never requires the purchase price again within its scope.
- SQLite by default; optional shared MariaDB/MySQL with verified TLS. SQL owns reservations, limits, unlocks and inbox entries. One serialized JDBC connection per instance, bounded workers, reconnect backoff and versioned schema migrations with pre-upgrade backups. Interrupted effects are never replayed on reconnect.
- Whole-package packing, empty-slot equipment, durable inbox, explicit full-inventory refusal and opt-in drops. Optional ordinary-kit FIRST_JOIN automation uses the public inbox; the independent private starter delivers silently.
- Explicitly enabled console reward actions with permitted roots and a per-action journal. Uncertain effects are never replayed automatically.
- Validation, local diagnostics, operation audit, counter reset and console-only manual recovery.

## Commands

| Command | Purpose |
|---|---|
| `/kit`, `/kits` | Main menu |
| `/kit <id>`, `/kit claim <id>` | Request a kit |
| `/kit preview <id>` | Protected preview |
| `/kit claimbox` | Collect saved rewards |
| `/kit language [locale]` | Choose language |
| `/kit create <id>` | Create an adventure-based ordinary kit and open its editor |
| `/kit edit <id>` | Edit settings |
| `/kit capture <id>` | Copy your inventory, armor and offhand into the kit |
| `/kit give <player> <id>` | Give an online player a free admin gift from game or console; no recipient permission, payment, confirmation or claim counter consumption; journal records the sender. Safety/profile/action policy still applies. |
| `/kit validate`, `/kit reload` | Check settings/definitions or apply a valid candidate |
| `/kit doctor` | Write a local diagnostic report |
| `/kit audit <operation-uuid>` | Inspect an operation |
| `/kit reset <player-uuid> <id> confirm` | Reset counters; keeps inbox rewards and purchased access; no refund |
| `/kit resolve <operation-uuid> <delivered\|retry> confirm` | Console-only recovery after checking external effects; see SECURITY.md |

If another plugin owns `/kit`, use `/gmkits:kit` and resolve the alias conflict. Existing commands are preserved.

Players need `gmkits.use` and the kit's configured permission. `gmkits.kit.adventure` defaults to true. The independent first-join module requires no kit permission. Administrator permissions are `gmkits.edit`, `.capture`, `.give`, `.reload`, `.doctor`, `.reset`; `gmkits.admin` groups them and defaults to operators. Cost/cooldown/limit bypasses are separate and default to false, including for operators.

## Configuration and scope

Runtime defaults in `src/main/resources/` match [examples/](examples/). See [KONFIGURACJA_PL.md](KONFIGURACJA_PL.md) for personalization and [START_PL.md](START_PL.md) for practical setup.

`/kit validate` and `/kit reload` additionally build the enabled kit items and private first-join equipment in small scheduler batches. Invalid materials/features or impossible packages report the file and item entry and prevent reload. Capture and content-editor saving require a working full codec. Blueprints can run without it, but frozen fallback rewards require the exact originating server version. Shared SQL does not convert modern items into legacy items.

This candidate rejects enabled animations, partial withdrawal, inbox expiry, telemetry and named preview layouts. Redis, importers, external plugin API and real component migration certification remain future work. English marketplace copy is drafted in [MARKETPLACE_EN.md](MARKETPLACE_EN.md); actual game screenshots and verified server versions still need to be supplied. The [product specification](SPECYFIKACJA_PL.md) is a roadmap, not a shipped-feature list.

SQL cannot atomically commit a Minecraft inventory or another plugin's economy. Uncertain effects require review; this is not a crash-proof exactly-once guarantee. See [SECURITY.md](SECURITY.md) and [TESTING.md](TESTING.md).

## Build

Use JDK 21 and Maven 3.9.9. Runtime classes target Java 8; the server determines its Java requirement.

```powershell
./build.ps1 -Verify
```

Alternatively: `mvn -Pmodern package`, `mvn -f verification/pom.xml test`, then `mvn package`. Verification uses the actual built Modern JAR. The script packages both artifacts, documentation, sources, notices and checksums. Optional `-Java8` runs core tests with a supplied Java 8 executable. Fresh builds need Maven dependency access; runtime downloads no libraries.

Maven uses the included catalogs directly. Their optional maintenance generator uses Node: `npm install --prefix tools`, then `node tools/locales.cjs`. It is not needed to build or run GMKits.

Code belongs to GemeZ. Dependency rights remain intact: [THIRD_PARTY.md](THIRD_PARTY.md). This is not an official Minecraft product and is not endorsed by Mojang, Microsoft or the server projects.
