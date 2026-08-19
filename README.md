---
language:
- en
tags:
- human
- value
pretty_name: NEVU-v1
size_categories:
- 10K<n<100K
---

## Paper

NEVU is introduced in the following paper:

**Event-Centric Human Value Understanding in News-Domain Texts: An Actor-Conditioned, Multi-Granularity Benchmark**

# NEVU Public Temporary Release

This is a temporary public release of the NEVU dataset for human value understanding in news-domain texts.

## Important Note

A portion of the full dataset is currently reserved for an ongoing protected blind evaluation and is therefore excluded from this release to avoid evaluation leakage.

- Protected GUIDs: 350
- Complete protected materials are planned to be released after the blind evaluation period ends in **January 2027**.

All released files in this repository have been filtered at the GUID level to remove protected instances where applicable.

## Files

The current release is organized into the following directories:

```text
event_base/
    event_base.json
    event_base_reformatted

majority-accepted_subset/
    majority-accepted_subset_without_blind_data.json

sub/
    train_human_value_labels.json
    train_human_value_labels_reformatted.json
    train_human_value_labels_2ids.json
    train_human_value_labels_2ids_reformatted.json

    dev_human_value_labels.json
    dev_human_value_labels_reformatted.json
    dev_human_value_labels_2ids.json
    dev_human_value_labels_2ids_reformatted.json

    test_human_value_labels.json
    test_human_value_labels_reformatted.json
    test_human_value_labels_2ids.json
    test_human_value_labels_2ids_reformatted.json

total/
    train_human_value_labels.json
    train_human_value_labels_reformatted.json
    train_human_value_labels_2ids.json
    train_human_value_labels_2ids_reformatted.json

    dev_human_value_labels.json
    dev_human_value_labels_reformatted.json
    dev_human_value_labels_2ids.json
    dev_human_value_labels_2ids_reformatted.json

    test_human_value_labels.json
    test_human_value_labels_reformatted.json
    test_human_value_labels_2ids.json
    test_human_value_labels_2ids_reformatted.json

hv_framework/
    ...
```

The previous top-level main files and example files have been removed. The underlying data formats and field definitions remain unchanged; only the release organization and file names have been updated.

### `event_base/`

The event-base data are kept as one unified collection rather than being split into train/dev/test files.

Train/dev/test membership is defined through the human-value annotation files under `total/` and `sub/`. All files are aligned through the shared `guid` field, so the corresponding event record can be retrieved from `event_base/` using the `guid` in an annotation record.

### `total/`

The `total/` directory contains the currently shareable human-value annotations corresponding to the `Total` subsets reported in the paper.

The annotations are separated into the official:
- train partition
- dev partition
- test partition

All protected GUIDs and their corresponding annotations have been removed from the released files.

### `sub/`

The `sub/` directory contains the currently shareable human-value annotations corresponding to the fixed sampled `Sub` subsets reported in the paper.

Before blind filtering, these sampled subsets contain:
- 20,000 annotated unit--actor pairs from train
- 2,500 annotated unit--actor pairs from dev
- 5,000 annotated unit--actor pairs from test

The current public files exclude protected GUIDs. Therefore, the released `sub/` files are filtered versions of the complete sampled subsets used in the reported experiments.

### `majority-accepted_subset/`

The file:

- `majority-accepted_subset_without_blind_data.json`

contains the currently shareable majority-accepted reference labels derived from the multi-group candidate-acceptance assessment reported in the paper.

The complete majority-accepted reference subset contains 400 evaluated unit--actor pairs covering 187 unique GUIDs. Four GUIDs overlap with the protected blind-evaluation set and are removed from the current public file.

The complete majority-accepted reference subset will be released after the blind evaluation period ends in January 2027.

### `hv_framework/`

The `hv_framework/` directory is unchanged from the previous release.

## File Descriptions

The field definitions below apply across the corresponding train/dev/test files in both `total/` and `sub/`.

For example:

- `total/train_human_value_labels_2ids.json`
- `total/dev_human_value_labels_2ids.json`
- `total/test_human_value_labels_2ids.json`
- `sub/train_human_value_labels_2ids.json`
- `sub/dev_human_value_labels_2ids.json`
- `sub/test_human_value_labels_2ids.json`

all follow the same schema described for `human_value_labels_2ids.json` below.

The same principle applies to the human-readable and reformatted versions.

### 1. `event_base/event_base.json`

This file contains the event-structured base data for each news article. Each record corresponds to one article and includes the article text, title, time, actor information, sentence list, and event-centric semantic structures.

Main fields include:
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

This file is the structural foundation of the dataset and is used to represent each article as a multi-level event-centric semantic unit.

Unlike the human-value annotation files, `event_base.json` is not divided into train/dev/test files. Partition membership is determined from the human-value annotation files, and corresponding event records can be retrieved using `guid`.

### 1b. `event_base/event_base_reformatted`

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

### 2. `*_human_value_labels_2ids.json`

These files contain the released human value labels in a compact ID-based format. Each record corresponds to one annotated `(unit, actor)` instance.

Main fields include:
- `guid`: article identifier
- `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
- `unit_id`: ID of the target unit within the article
- `actor`: target actor ID
- `aligned`: list of aligned human value IDs
- `contradictory`: list of contradictory human value IDs

Because one article may contain multiple annotated units and multiple actors, these files are expanded: a single `guid` may appear in many rows.

These files are suitable for benchmark training and evaluation, since they provide the released reference labels in a compact machine-readable format.

The same schema is used for:
- `total/train_human_value_labels_2ids.json`
- `total/dev_human_value_labels_2ids.json`
- `total/test_human_value_labels_2ids.json`
- `sub/train_human_value_labels_2ids.json`
- `sub/dev_human_value_labels_2ids.json`
- `sub/test_human_value_labels_2ids.json`

### 2b. `*_human_value_labels_2ids_reformatted.json`

These files are reformatted versions of the corresponding `*_human_value_labels_2ids.json` files.

Compared with the original version:
- unit types are normalized to `article`, `subevent`, `bce`, and `sce`
- `bce` corresponds to the original `behavior_chains`
- `sce` corresponds to the original `story_narratives`
- the original value labels are reorganized into two explicit levels:
  - `l1_label`
  - `l2_label`

Main fields include:
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

These files are recommended for users who want a more explicit hierarchical label structure while preserving the compact machine-readable format.

The same schema is used for the train/dev/test files under both `total/` and `sub/`.

### 3. `*_human_value_labels.json`

These files contain the human value annotations in a richer, more interpretable format. Each record corresponds to one article and groups value annotations by semantic level.

Main sections include:
- `article_human_values`
- `subevents_human_values`
- `behavior_chains_human_values`
- `story_narrative_human_values`

Within each section, annotations are organized by actor and direction:
- `aligned_with_human_values`
- `contradictory_to_human_values`

For each value label, the public release retains:
- `confidence`
- `explanation`

Fields such as `model`, `qa_results`, `human_voting_count`, and `human_voting_num` are not included in the public release.

These files are useful for interpretation, qualitative analysis, and understanding why a particular human value label was assigned.

The same schema is used for:
- `total/train_human_value_labels.json`
- `total/dev_human_value_labels.json`
- `total/test_human_value_labels.json`
- `sub/train_human_value_labels.json`
- `sub/dev_human_value_labels.json`
- `sub/test_human_value_labels.json`

### 3b. `*_human_value_labels_reformatted.json`

These files are reformatted versions of the corresponding `*_human_value_labels.json` files in a flatter and more instance-oriented format.

Each record corresponds to one `(guid, unit_level, unit_id, actor)` instance and groups all associated human values under that instance.

Main fields include:
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

These files are useful for easier programmatic loading, actor-centered inspection, and modeling pipelines that prefer flatter structures.

The same schema is used for the train/dev/test files under both `total/` and `sub/`.

## Relationship Between the Main Files

All files are aligned through the `guid` field.

### Event structure
- `event_base/event_base.json` provides the article structure, event hierarchy, and actor definitions.
- `event_base/event_base_reformatted` provides the corresponding normalized event representation.

### Total human-value annotations
Files under `total/` provide the currently shareable annotations from the complete NEVU train/dev/test partitions after protected-GUID filtering.

### Sampled human-value annotations
Files under `sub/` provide the currently shareable portions of the fixed sampled subsets used for the main HVR experiments.

### Majority-accepted reference labels
- `majority-accepted_subset/majority-accepted_subset_without_blind_data.json` provides the currently shareable portion of the majority-accepted reference subset.

In other words:
- use `event_base/event_base.json` or `event_base/event_base_reformatted` to understand the article and event structure,
- use `*_human_value_labels_2ids.json` or `*_human_value_labels_2ids_reformatted.json` for benchmark-style supervised learning and evaluation,
- use `*_human_value_labels.json` or `*_human_value_labels_reformatted.json` for explanation-oriented inspection and qualitative analysis,
- use the annotation-file `guid` to retrieve the corresponding record from the unified event-base file.

## Original and Reformatted Files

The reformatted files are provided for convenience and do not define a different dataset split or annotation standard. They reorganize the same released content into more normalized structures.

In all reformatted files, unit types are consistently represented in lowercase as:
- `article`
- `subevent`
- `bce`
- `sce`

where:
- `bce` corresponds to the original `behavior_chains`
- `sce` corresponds to the original `story_narratives`

Users who prefer the original structure can use the non-reformatted files. Users who want flatter and more standardized formats may use the reformatted files.

## Contents of the Current Release

The current release is constructed from the NEVU data used in the paper after removing all records associated with the 350 protected GUIDs.

The event-base records are distributed in one unified file, while the human-value annotations are organized according to the train/dev/test partitions under `total/` and `sub/`.

Additional preprocessing for the public release remains consistent with the previous release:
- In `event_base.json`, some internal metadata fields are removed.
- In the human-readable human-value annotation files, only the public annotation fields such as `confidence` and `explanation` are retained.
- Reformatted files are additionally provided for easier parsing and more standardized downstream use.

## Protected Blind-Evaluation Data

The current release excludes 350 protected GUIDs and their corresponding annotations.

Protected instances are removed by `guid` from:
- the released `event_base/` data,
- the train/dev/test files under `total/`,
- the train/dev/test files under `sub/`,
- the released majority-accepted reference subset.

Because some sampled experimental subsets overlap with these protected GUIDs, the current public `sub/` files are not the complete sampled data used for the reported experiments.

The complete protected annotations and associated materials will be released after the blind evaluation period ends in **January 2027**.

## Notes

The `total/` and `sub/` directory names follow the subset terminology used in the paper.

The event-base data are not independently divided into train/dev/test because they serve as the shared structural source. The train/dev/test human-value annotation files determine partition membership, and their `guid` values can be used to retrieve the corresponding event-base records.

The reformatted files preserve the same released annotation content as their corresponding original-format files while reorganizing it for easier use.

If exact backward compatibility with earlier parsing scripts is important, use the original-format files. If easier loading and more uniform field naming are preferred, use the reformatted files.