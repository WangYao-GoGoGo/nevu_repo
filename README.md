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

This repository contains the publicly shareable portion of the dataset.

- Number of excluded held-out GUIDs: 350

Some held-out evaluation data are excluded from this release. A more complete version is tentatively planned for release in late 2026.

## Files

Main files:
- `event_base.json`
- `human_value_labels_2ids.json`
- `human_value_labels.json`

Example files:
- `event_base_examples.json`
- `human_value_labels_2ids_examples.json`
- `human_value_labels_examples.json`

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

### 2. `human_value_labels_2ids.json`

This file contains the gold human value labels in a compact ID-based format. Each record corresponds to one annotated `(unit, actor)` instance.

Main fields:
- `guid`: article identifier
- `unit_level`: semantic level of the annotation (`article`, `subevent`, `BCE`, or `SCE`)
- `unit_id`: ID of the target unit within the article
- `actor`: target actor ID
- `aligned`: list of aligned human value IDs
- `contradictory`: list of contradictory human value IDs

Because one article may contain multiple annotated units and multiple actors, this file is expanded: a single `guid` may appear in multiple rows.

This file is suitable for benchmark training and evaluation because it provides the final gold labels in a compact machine-readable format.

### 3. `human_value_labels.json`

This file contains the human value annotations in a richer and more interpretable format at the article level. Each record corresponds to one article and groups value annotations by semantic level.

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

## Relationship Between the Three Main Files

The three files are aligned through the `guid` field:

- `event_base.json` provides the article structure, event hierarchy, and actor definitions.
- `human_value_labels_2ids.json` provides compact gold labels for `(unit, actor)` instances.
- `human_value_labels.json` provides more detailed and human-readable value annotation content for the same articles.

In practice:
- use `event_base.json` to inspect the article structure,
- use `human_value_labels_2ids.json` for benchmark-style training and evaluation,
- use `human_value_labels.json` for explanation-oriented inspection and qualitative analysis.

## Example Files

To make the dataset easier to preview, this repository also provides three small example files. These files contain aligned examples with matching `guid`s across the three formats.

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

## Statistics

- `event_base.json`: 2515 records
- `human_value_labels_2ids.json`: 65950 records
- `human_value_labels.json`: 2515 records