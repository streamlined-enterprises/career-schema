# Extended JSON Resume Schema (Career Factory)

A versatile, backwards-compatible superset of the JSON Resume data model for maintaining a single “career database” and generating many targeted resume variants and layouts. This repo publishes `career-schema-v2.json` as the canonical JSON Schema for validating those data files. [code_file:79]

## Goals

- Keep high compatibility with the original JSON Resume shape so existing tooling can still consume the core fields.
- Add “factory” features for targeted resumes: scoring, tagging, visibility rules, and rich metrics.
- Avoid opinionated sections (e.g., “speaking”) by providing a generic `accomplishments` collection that can represent talks, patents, press, community work, art, and more.

## Compatibility

This schema includes the full JSON Resume feature surface (core sections and fields) and keeps their names intact where possible:

- `basics`
- `work`
- `volunteer`
- `education`
- `awards`
- `certificates`
- `publications`
- `skills`
- `languages`
- `interests`
- `references`
- `projects`

Tools that expect JSON Resume will typically ignore extra fields and continue to work (exact behavior depends on the tool).

## Extensions (Factory Layer)

These additions are designed for generating many resumes from one dataset:

- `emphasis`: Per-item relevance scores (0–10) for any “angle” (e.g., `executive`, `security`, `backend`, `platform`).
- `visibility`: Controls what gets included (e.g., `public`, `confidential`, `includeIn`, `excludeFrom`).
- `metrics`: Key-value impact metrics (numbers or strings) attached to experiences/projects/accomplishments.
- `tags`: Keywords for filtering and grouping.
- `accomplishments`: A generic, non-opinionated list of notable outputs; each item has a `type` (e.g., `Keynote`, `Article`, `Patent`, `Media`, `Award`, `Workshop`, `OpenSourceRelease`).

### Example (minimal but factory-ready)

```json
{
  "$schema": "https://raw.githubusercontent.com/YOUR_GITHUB_ORG/YOUR_REPO/main/career-schema-v2.json",
  "basics": {
    "name": "Jane Doe",
    "email": "jane@example.com",
    "label": "Technology Leader",
    "summary": "Builds reliable platforms and teams."
  },
  "work": [
    {
      "company": "ExampleCo",
      "position": "Director of Engineering",
      "startDate": "2021-01-01",
      "endDate": null,
      "summary": "Owned platform strategy and delivery.",
      "highlights": [
        "Reduced infrastructure cost by 35%",
        "Improved uptime to 99.99%"
      ],
      "tags": ["leadership", "platform", "cloud"],
      "emphasis": { "executive": 9, "technical": 7 },
      "metrics": { "teamSize": 12, "budgetUsd": 2000000 },
      "visibility": { "public": true, "includeIn": ["executive", "platform"] }
    }
  ],
  "accomplishments": [
    {
      "title": "Scaling Secure SaaS Platforms",
      "type": "Keynote",
      "entity": "PlatformConf",
      "date": "2024-10-15",
      "stats": { "attendees": 450 },
      "emphasis": { "speaker": 10, "executive": 7 }
    }
  ]
}
