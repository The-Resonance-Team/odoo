# Packages are deep modules

Every immediate child directory of `packages/` is one package and one **deep
module**: a lot of behaviour behind a small interface. The **interface** is the
package's entry points — the files at its root (`index.ts`, `client.ts`, …).
Everything in a subfolder is **implementation** and private.

Copy this shape when adding a package:

```
packages/pkg-core/
├── index.ts        # entry point (public); export one small function
├── client.ts       # another entry point, if needed — no barrel
├── lib/impl.ts     # implementation (private)
└── tests/core.test.ts
```

Import a package only through its entry points: `import { thing } from
"@odoo/pkg-core"` (or a relative path to its root file from inside the repo).
Never import `lib/` or any other subfolder from outside the package. Prefer
several small entry points over one barrel that re-exports a whole subtree —
a barrel makes the interface as large as the implementation.

1. **Entry-point boundary**: code outside a package — app code or another
   package — may import only that package's root files, never a subfolder.
   The root files are the seam; everything below them is implementation.
2. **Intra-package freedom**: a package's own files may import each other
   freely. Internal seams are private to the implementation and its own tests.
3. **Tests through the entry points**: tests may import any package's entry
   points and their own `tests/` fixtures, but never any package's internals,
   not even their own. Cross-package integration tests are fine; deep imports
   are not.
4. **No cycles**: the dependency graph is acyclic. A cycle means two modules
   share one interface and neither has a clean seam.

Layering is a separate concern: `pkg-*` (shared, db, contracts) is the lowest
tier; `addon-*` may depend on `pkg-*` and lower-tier addons; `app-*` may depend
on any package. Rust workers under `workers/` depend on shared contracts only,
never on apps or addons. Addon tier order is declared when real addons land.
If a case needs a bypass, ask — do not add an inline suppression.

Run `bun run lint:boundaries` (`depcruise packages`) to enforce all four rules.
The config lives at `.dependency-cruiser.cjs`; no per-package config is needed.
