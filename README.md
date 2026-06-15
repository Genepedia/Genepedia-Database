# Genepedia-Database

Canonical structured people database for Genepedia.

This repository is mounted into the main site as the `data/Genepedia-Database` submodule.
All database files live under the top-level `people/` directory in this repository.

## Layout

`people/manifest.json`
- Database metadata: counts, shard size, import source, and layout summary.

`people/persons/<bucket>/<id>.json`
- Canonical person records.

`people/unions/<bucket>/<id>.json`
- Canonical family / union records.

`people/ownership/<bucket>/<id>.json`
- Profile ownership and maintainer metadata.

`people/index/summary/<bucket>.json`
- Lightweight person summaries used for relationships and listings.

`people/index/search/<key>.json`
- Search shards keyed by normalized name prefixes.

`people/index/all-ids.json`
- Ordered list of all person ids.

`people/index/ownership-logins.json`
- Reverse lookup from GitHub login to claimed profile id.

`people/sources/gedcom-id-map.json`
- External GEDCOM id to local Genepedia person id mapping.

`people/export/full-tree.ged`
- Full regenerated GEDCOM export.

`people/reports/*.json`
- Import, media, and privacy audit reports.

## Sharding

Records are sharded in buckets of 1000 ids to avoid unbounded directory growth.

Examples:

- person `1` -> `people/persons/0/1.json`
- person `1000` -> `people/persons/0/1000.json`
- person `1001` -> `people/persons/1/1001.json`

Bucket formula:

- `floor((max(1, id) - 1) / 1000)`

## Source Of Truth

This repository is the source of truth for structured profile data:

- names
- events
- relationships
- primary and linked media metadata
- ownership / maintainer records

The narrative prose and SEO shells still live in the main Genepedia site repository under `people/<id>/`.

## Editing Rules

- Keep canonical structured changes in the files under `people/` here.
- Do not add a version folder like `v1/` under this repo.
- Keep repository-relative paths rooted at `people/`.
- Derived indexes should be regenerated after bulk edits or imports.

## Tooling

The main Genepedia repo contains the tooling that reads and writes this database:

- `scripts/lib/people-db-paths.mjs`
- `scripts/import-gedcom.mjs`
- `scripts/reindex.mjs`
- `lib/people-db.js`

Those tools address this repository through the submodule path `data/Genepedia-Database/people/...` in the site workspace, which maps to `people/...` inside this repository.
