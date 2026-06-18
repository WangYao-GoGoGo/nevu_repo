---
language:
- en
tags:
- human-values
- news
- benchmark
pretty_name: NEVU-v1
size_categories:
- 10K<n<100K
---

# NEVU Public Release

NEVU is a dataset for event-centric human value understanding in news-domain texts.

This repository provides a public-release version of the dataset for research use. Some held-out evaluation items are not included in this release.

## Release Note

This repository contains the publicly shareable portion of NEVU for research use and review-time inspection.

A subset of held-out evaluation items is not included in this anonymous public release. These items are reserved for an ongoing blind evaluation setting, and are withheld to prevent benchmark leakage and preserve the integrity of protected evaluation data.

- Number of excluded held-out GUIDs: 350

The manuscript reports statistics over the complete internal NEVU benchmark. This anonymous repository contains the currently shareable public subset. After the blind evaluation period concludes, we will reassess the release status of the withheld items. If they are no longer needed for protected evaluation and no licensing or privacy constraints apply, they will be added to the final de-anonymized release. If continued blind benchmarking is needed, they will remain protected and an access or evaluation protocol will be documented separately.

## Files

### Original release files

Main files:
- `event_base.json`
- `human_value_labels_2ids.json`
- `human_value_labels.json`

Example files:
- `event_base_examples.json`
- `human_value_labels_2ids_examples.json`
- `human_value_labels_examples.json`

### Reformatted release files

To make the dataset easier to parse and use in downstream modeling, this release also provides reformatted versions of the three main files:

- `event_base_reformatted.json`
- `human_value_labels_2ids_reformatted.json`
- `human_value_labels_reformatted.json`

These reformatted files preserve the same annotation content as the original release as much as possible, while reorganizing the structure into more normalized and model-friendly formats.

In all reformatted files, unit types are consistently represented in lowercase as:
- `article`
- `subevent`
- `bce`
- `sce`

where:
- `bce` corresponds to the original `behavior_chains`
- `sce` corresponds to the original `story_narratives`

Users who prefer the original release structure can continue using the original files. Users who want flatter and more standardized formats may use the reformatted files.

## File Overview

### 1. `event_base.json`

This file contains the event-structured base data for each news article. Each record corresponds to one article and includes the article text, title, time information, actor information, sentence list, and event-centric semantic structures.

Main fields:
- `guid`: unique article identifier
- `title`: article title
- `content`: raw article text
- `time`: publication date or time information
- `actors`: normalized major social actors appearing in the article
- `sentences`: sentence-level segmented article content
- `subevents`: fine-grained local event units grounded to sentence IDs
- `behaviors`: behavior-based event chains
- `story_narratives`: higher-level narrative groupings over subevents
- `news_type_l1`: coarse news genre label

This file provides the structural foundation of the dataset and represents each article as a multi-level event-centric semantic structure.

### 1b. `event_base_reformatted.json`

This file is a reformatted version of `event_base.json` with more standardized field organization.

Compared with the original version:
- `behaviors` is renamed to `bces`
- `story_narratives` is renamed to `sces`
- each bce entry is explicitly structured with:
  - `unit_id`
  - `bce_title`
  - `behaviors`

For each bce entry:
- `unit_id` is the list of behavior IDs contained in that bce
- `bce_title` is the original behavior-chain title
- `behaviors` contains the original behavior items grouped under that bce

In this reformatted version:
- `bces` correspond to the original `behavior_chains`
- `sces` correspond to the original `story_narratives`

This file is recommended for users who want a cleaner and more uniform event structure for parsing, modeling, or cross-level processing.

### 2. `human_value_labels_2ids.json`

This file contains the gold human value labels in a compact ID-based format. Each record corresponds to one annotated `(unit, actor)` instance.

Main fields:
- `guid`: article identifier
- `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
- `unit_id`: ID of the target unit within the article
- `actor`: target actor ID
- `aligned`: list of aligned human value IDs
- `contradictory`: list of contradictory human value IDs

Because one article may contain multiple annotated units and multiple actors, this file is expanded: a single `guid` may appear in multiple rows.

This file is suitable for benchmark training and evaluation because it provides the final gold labels in a compact machine-readable format.

### 2b. `human_value_labels_2ids_reformatted.json`

This file is a reformatted version of `human_value_labels_2ids.json`.

Compared with the original version:
- unit types are normalized to `article`, `subevent`, `bce`, and `sce`
- `bce` corresponds to the original `behavior_chains`
- `sce` corresponds to the original `story_narratives`
- the original value labels are reorganized into two explicit levels:
  - `l1_label`
  - `l2_label`

Main fields:
- `guid`: article identifier
- `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
- `unit_id`: ID of the target unit within the article
- `actor`: target actor
- `l1_label`:
  - `aligned`: list of aligned Level-1 value IDs
  - `contradictory`: list of contradictory Level-1 value IDs
- `l2_label`:
  - `aligned`: list of aligned Level-2 value IDs
  - `contradictory`: list of contradictory Level-2 value IDs

This file is recommended for users who want a more explicit hierarchical label structure while preserving the compact machine-readable format.

### 3. `human_value_labels.json`

This file contains the human value annotations in a richer and more interpretable format. Each record corresponds to one article and groups value annotations by semantic level.

Main sections:
- `article_human_values`
- `subevents_human_values`
- `behavior_chains_human_values`
- `story_narrative_human_values`

Within each section, annotations are organized by actor and direction:
- `aligned_with_human_values`
- `contradictory_to_human_values`

For each value label, this public release retains:
- `confidence`
- `explanation`

Some internal annotation fields are omitted from this release.

This file is useful for interpretation, qualitative analysis, and understanding why a particular human value label was assigned.

### 3b. `human_value_labels_reformatted.json`

This file is a reformatted version of `human_value_labels.json` in a flatter and more instance-oriented format.

Each record corresponds to one `(guid, unit_level, unit_id, actor)` instance and groups all associated human values under that instance.

Main fields:
- `guid`: article identifier
- `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
- `unit_id`: ID of the target unit
- `actor`: target actor
- `human_values`: list of value entries

Each value entry includes:
- `l1_label`: Level-1 value label
- `l2_label`: corresponding Level-2 value label
- `confidence`: retained confidence score
- `explanation`: retained explanation text
- `direction`: `aligned` or `contra`

For reformatted unit identifiers:
- for `article`, `unit_id` is empty
- for `subevent`, `unit_id` is the `subevent_id`
- for `sce`, `unit_id` is the `story_narrative_id`
- for `bce`, `unit_id` is the list of IDs in `behavior_ids_chains`

This file is useful for easier programmatic loading, actor-centered inspection, and modeling pipelines that prefer flatter structures.

## Relationship Between the Main Files

The files are aligned through the `guid` field.

### Original files
- `event_base.json` provides the article structure, event hierarchy, and actor definitions.
- `human_value_labels_2ids.json` provides compact gold labels for `(unit, actor)` instances.
- `human_value_labels.json` provides more detailed and human-readable value annotation content for the same articles.

### Reformatted files
- `event_base_reformatted.json` provides a normalized event structure using `subevents`, `bces`, and `sces`.
- `human_value_labels_2ids_reformatted.json` provides compact gold labels with normalized unit types and both Level-1 and Level-2 label groupings.
- `human_value_labels_reformatted.json` provides flatter and more human-readable instance-level value annotations.

In practice:
- use the **original files** if you want the original public release structure
- use the **reformatted files** if you want easier parsing, flatter loading, and more standardized field naming

## Example Files

To make the dataset easier to preview, this repository also provides three small example files. These files contain aligned examples with matching `guid`s across the three original formats.

### 1. `event_base_examples.json`

This file contains sample article records from `event_base.json`. It is intended to help users quickly inspect the event-centric structure of the dataset, including actors, subevents, behavior chains, and story narratives.

### 2. `human_value_labels_2ids_examples.json`

This file contains the corresponding gold label examples from `human_value_labels_2ids.json` for the same sampled `guid`s. Since this file is expanded by `(unit, actor)`, it may contain more rows than the number of sampled articles.

### 3. `human_value_labels_examples.json`

This file contains the corresponding sample records from `human_value_labels.json`, showing the same articles together with their human-readable value annotations and explanations.

These example files are intended for quick inspection and visualization. For research use, please use the main release files.

## Contents of This Release

This release is derived from the internal dataset after excluding a held-out portion.

Additional preprocessing for this public release:
- In `event_base.json`, some internal metadata fields were removed.
- In `human_value_labels.json`, selected explanatory fields are retained for each value entry.
- Reformatted files are additionally provided to support easier parsing and more standardized downstream use.

## Statistics

### Original files
- `event_base.json`: 2515 records
- `human_value_labels_2ids.json`: 65950 records
- `human_value_labels.json`: 2515 records

### Reformatted files
- `event_base_reformatted.json`: 2515 records
- `human_value_labels_2ids_reformatted.json`: 65950 records
- `human_value_labels_reformatted.json`: flattened instance-level records organized by `(guid, unit_level, unit_id, actor)`

## Notes

The reformatted files are provided for convenience and do not define a different dataset split or annotation standard. They reorganize the same released content into more normalized structures.

If exact backward compatibility with earlier parsing scripts is important, please use the original files. If easier loading and more uniform field naming are preferred, please use the reformatted files.