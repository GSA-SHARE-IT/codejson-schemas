# Changelog

All notable changes to the `code.json` schemas in this repository — the generic, government-wide schema (`schemas/schema-*.json`) and the CMS-specific schema (`schemas/cms/schema-*.json`) — are documented in this file, so changes are easy to track across releases.

This project follows a `major.minor.patch`-style versioning scheme for each schema file. Dates are approximate (based on file history) where not otherwise known.

## Government-Wide Generic Schema

### [schema-0.1.0.json](./schemas/schema-0.1.0.json)

> [!IMPORTANT]  
> This is the latest up-to-date schema that reflects recent government policy updates (the SHARE IT Act). Versioning was intentionally restarted at `0.1.0` as a result; the next release will be published as `1.0.0` once the implementing policy memo is officially released.

#### Added

- `identifier` field — a persistent unique ID used to track an asset across renames/migrations. Added to `properties` (right after `name`) and to `required`.
- `downloads` property added to `reuseFrequency.properties`, alongside the existing `forks` and `clones`.

#### Changed

- `$id` updated to `https://gsa-share-it.github.io/code-json-schema.json` (reflecting the repository's move from the `DSACMS` org to `GSA-SHARE-IT`).
- `permissions.usageType`
  - enum: M-16-21 exemptions are removed from the schema: `exemptByIPRestriction`, `exemptByAgencySystem`, `exemptByAgencyMission`, `exemptByCIO`, and `exemptByPolicyDate`
  - enum: `exemptByDisposableCode`, `exemptByExperimentalCode`, and `exemptByCommercialSoftware` are added, reflecting the SHARE IT Act's exclusions for exploratory/disposable code and commercial/COTS software.

#### Required

- `identifier` added to `required`.
- `AIUseCaseID`, `SBOM`, `date`, `description`, `laborHours`, `languages`, `tags`, and `vcs` removed from `required` (now optional).

### [gov-schema-2.0.0.json](./schemas/cms/gov-schema-2.0.0.json)

#### Added

- `feedbackMechanism` (singular)
- `version`, `homepageURL`, `downloadURL`, `disclaimerURL`, `disclaimerText`, `relatedCode`, `reusedCode`, `partners` added as optional fields, taken from original [M-16-21 code.json schema](https://github.com/GSA/code-gov-data/tree/master/schemas)
- `AIUseCaseID` added for assisting with agency collection of their AI Use Case Inventory as part of M-25-21 compliance
- `SBOM` added to include software's SBOM in metadata as part of EO 14028

#### Removed

- `categories`, `feedbackMechanisms` (plural, replaced by singular `feedbackMechanism`), `localisation`, `longDescription`, `platforms`, `softwareType` — these agency-extension-style fields were dropped from the generic gov-wide schema.

#### Changed

- Migrated `$schema` dialect from `draft-04` to `2020-12` and introduced a formal `$id` (`https://dsacms.github.io/code-json-schema.json`).
- Corrected the same structural bug present in `gov-schema-1.0.0.json`, where all field definitions were incorrectly nested one level too deep under a top-level `items` key instead of directly under `properties`.
- `permissions.usageType` changed from a single `string` value to an `array` of strings, allowing multiple exemption reasons to be declared simultaneously.
- Expanded and refined the exemption enum taxonomy: the old `exemptByLaw` was split into `exemptByNationalSecurity`, `exemptByNationalIntelligence`, `exemptByFOIA`, `exemptByEAR`, `exemptByITAR`, `exemptByTSA`, and `exemptByClassifiedInformation`; the old `exemptByNationalSecurity` (covering national security, confidentiality, _and_ individual privacy risk) was split so privacy risk became its own value, `exemptByPrivacyRisk`; `exemptByIPRestriction` was also added.
- `organization`: removed the CMS-specific enum restriction (`Centers for Medicare & Medicaid Services`) and generalized it to a free-text string with an example (`'18F'` or `'Navy'`) suitable for any agency.
- Top-level `additionalProperties` changed from `false` to `true`, allowing agencies to extend the schema with their own custom fields.

#### Required

- `AIUseCaseID`, `SBOM`, and `feedbackMechanism` added to `required`; `categories`, `feedbackMechanisms`, `localisation`, `longDescription`, `platforms`, and `softwareType` removed from `required` (dropped entirely, see Removed above).

### [gov-schema-1.0.0.json](./schemas/cms/gov-schema-1.0.0.json)

Initial generic (government-wide) `code.json` schema (`draft-04`). Known issues, later fixed in `gov-schema-2.0.0.json`:

- Field definitions are nested one level too deep under `properties.items` instead of directly under `properties` (the same structural bug present in the CMS schema's `schema-0.0.0.json`).
- `permissions.usageType` is a single `string` value (not an array), so only one exemption reason could be declared per release, with a smaller 8-value exemption enum (`openSource`, `governmentWideReuse`, `exemptByLaw`, `exemptByNationalSecurity`, `exemptByAgencySystem`, `exemptByAgencyMission`, `exemptByCIO`, `exemptByPolicyDate`).
- `organization` was restricted to a CMS-only enum value (`Centers for Medicare & Medicaid Services`), despite this being the generic, agency-agnostic schema.

## Agency Schemas

### CMS Schema

### [2.2.0](/schemas/cms/schema-2.2.0.json)
> [!IMPORTANT]  
> This is the latest up-to-date schema that reflects recent government policy updates (the SHARE IT Act).

#### Added

- `identifier` field — a persistent unique ID used to track an asset across renames/migrations. Added to `properties` (right after `name`) and to `required`.
- `downloads` property added to `reuseFrequency.properties`, alongside the existing `forks` and `clones`.

#### Changed

- `permissions.usageType`
  - enum: M-16-21 exemptions are removed from the schema: `exemptByIPRestriction`, `exemptByAgencySystem`, `exemptByAgencyMission`, `exemptByCIO`, and `exemptByPolicyDate`
  - enum: `exemptByDisposableCode`, `exemptByExperimentalCode`, and `exemptByCommercialSoftware` are added, reflecting the SHARE IT Act's exclusions for exploratory/disposable code and commercial/COTS software.
  - Updated the `usageType` description text to define and renumber all 13 enum values to match.
- Reworded the `reuseFrequency` description for clarity: _"Measures frequency of code reuse in various forms such as forks and clones for repositories, and downloads for packaged and distributed software."_

### [2.1.0](/schemas/cms/schema-2.1.0.json)

#### Changed (enum value additions only — no properties added/removed)

- `repositoryHost`: added `github.com/MeasureAuthoringTool`.
- `subsetInHealthcare`: added `SNAP`, `TANF`, `human-benefit-services`.
- `userType`: added `applicants`, `beneficiaries`, `enrollees`.

### [2.0.0](/schemas/cms/schema-2.0.0.json)

#### Added

- `version`, `homepageURL`, `downloadURL`, `disclaimerURL`, `disclaimerText`, `relatedCode`, `reusedCode`, `partners` added as optional fields, taken from original [M-16-21 code.json schema](https://github.com/GSA/code-gov-data/tree/master/schemas)

#### Removed

- `projectURL` (superseded by `homepageURL`/`downloadURL`).

### [0.2.0](/schemas/cms/schema-0.2.0.json)

#### Added

- `feedbackMechanism` (singular).
- `AIUseCaseID` added for assisting with agency collection of their AI Use Case Inventory as part of M-25-21 compliance
- `SBOM` added to include software's SBOM in metadata as part of EO 14028

#### Removed

- `feedbackMechanisms` (plural, replaced by singular `feedbackMechanism`), `upstream`.

#### Changed

- Migrated `$schema` dialect from `draft-04` to `2020-12` and introduced a formal `$id` (`https://dsacms.github.io/code-json-schema.json`).

### [0.1.0](/schemas/cms/schema-0.1.0.json)

#### Added

- `contractNumber`, `feedbackMechanisms` `repositoryVisibility`, `reuseFrequency` added as required metadata fields as per the SHARE IT Act
- `projectURL`, `projects`, `systems`, `upstream` added to better track repository's relationship with its project, CMS systems, and dependencies

#### Fixed

- Corrected a structural bug from `0.0.0` where all field definitions were incorrectly nested one level too deep under a top-level `items` key instead of directly under `properties`.

### [0.0.0](/schemas/cms/schema-0.0.0.json)

Initial CMS `code.json` schema (`draft-04`). Note: this version has a known structural issue — field definitions are nested under `properties.items` instead of directly under `properties`, so the schema does not validate `name`, `status`, etc. as intended.
