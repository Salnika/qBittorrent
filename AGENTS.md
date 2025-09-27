# Repository Guidelines

## Project Structure & Module Organization
qBittorrent's client lives under `src/`, split into `app/` (entry points), `base/` (core BitTorrent and utility layers), `gui/` (Qt widgets), `searchengine/` (Python-powered discovery), and `webui/` (browser interface). Shared assets sit in `src/icons/` and localized strings in `src/lang/`. CMake logic is stored in `CMakeLists.txt` and the `cmake/` helpers. Tests reside in `test/` with fixtures under `test/testdata/`. Documentation (`doc/`, `INSTALL`, `SECURITY.md`) and release helpers (`dist/`, `build_dist.sh`) round out the top level.

## Build, Test, and Development Commands
Configure with `cmake -B build -S . -DCMAKE_BUILD_TYPE=Release` to produce an optimized GUI build. For headless service builds, add `-DGUI=OFF` and run `cmake --install build` to stage `qbittorrent-nox`. Iterative work uses `cmake --build build --parallel` and `cmake --build build --target qbittorrent`. Enable the optional test suite during configure with `-DTESTING=ON`, then execute `cmake --build build --target check`. Use `cmake --build build --target install` only after verifying artifacts.

## Coding Style & Naming Conventions
Follow the project C++ style in `CODING_GUIDELINES.md`: four-space indentation, Allman braces, camelCase identifiers, and `m_` prefixes on private members. Keep header includes grouped (stdlib → system → Boost → libtorrent → Qt → project). Run `uncrustify -c uncrustify.cfg --no-backup <file>` or import `codingStyleQtCreator.xml` before review. JavaScript/WebUI code mirrors these rules; prefer ESLint defaults from the existing sources.

## Testing Guidelines
Unit tests use Qt Test via CTest targets; add new cases beside related modules in `test/` and name files `test<Feature>.cpp`. Cover new logic paths and update fixtures when protocol behaviour changes. After configuring with `-DTESTING=ON`, `cmake --build build --target check` runs the suite; keep it green before pushing. No explicit coverage gate exists, but patches should include targeted regression checks.

## Commit & Pull Request Guidelines
Craft commit subjects ≤50 chars, capitalized, imperative, and separated from bodies by a blank line; wrap bodies at 72 chars and reference issues with `Closes #NNNN.`. Squash fixups locally to keep history clean. Pull requests need a concise title plus a description explaining motivation, approach, and testing; link issues, list follow-ups, and attach screenshots for UI updates. Rebase onto the target branch before requesting review and confirm CI passes.

## Security & Configuration Tips
Report vulnerabilities privately via `SECURITY.md`. When working on search plugins or scripting features ensure Python ≥3.9 is available, and document any runtime configuration changes in `doc/` so packagers can align defaults.
