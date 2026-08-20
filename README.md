---

language:

* en
  tags:
* human
* value
  pretty_name: NEVU-v1
  size_categories:
* 10K<n<100K

---

## Paper

NEVU is introduced in the following paper:

**Event-Centric Human Value Understanding in News-Domain Texts: An Actor-Conditioned Benchmark across Multi-Scope Event Contexts**

# NEVU Public Temporary Release

This is a temporary public release of the NEVU dataset for human value understanding in news-domain texts.

## Important Note

A portion of the full dataset is currently reserved for an ongoing protected blind evaluation and is therefore excluded from this release to avoid evaluation leakage.

* Protected GUIDs: 350
* Complete protected materials are planned to be released after the blind evaluation period ends in **January 2027**.

All released files in this repository have been filtered at the GUID level to remove protected instances where applicable.

## Files

The current release is organized into the following directories:

```text
event_base/
    event_base.json
    event_base_reformatted.json

    train/
        train_event_base.json
        train_event_base_reformatted.json

    dev/
        dev_event_base.json
        dev_event_base_reformatted.json

    test/
        test_event_base.json
        test_event_base_reformatted.json

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

The underlying data formats and field definitions remain unchanged. The release organization additionally provides explicit train/dev/test event-base files aligned with the corresponding human-value annotation partitions.

### `event_base/`

The `event_base/` directory contains both the unified event-base collection and explicit train/dev/test event-base files.

The unified files are:

* `event_base/event_base.json`
* `event_base/event_base_reformatted.json`

These files contain all currently shareable event-base records in a single collection.

For convenience and direct alignment with the released NEVU partitions, the same event-base records are additionally divided into:

* `event_base/train/`
* `event_base/dev/`
* `event_base/test/`

The partitioned files are:

* `event_base/train/train_event_base.json`
* `event_base/train/train_event_base_reformatted.json`
* `event_base/dev/dev_event_base.json`
* `event_base/dev/dev_event_base_reformatted.json`
* `event_base/test/test_event_base.json`
* `event_base/test/test_event_base_reformatted.json`

The train/dev/test assignment of event records follows the GUID-based partition membership defined by the corresponding full human-value annotation partitions under `total/`. No event structure is changed during this partitioning process.

Thus, users may either work with the unified event-base files or directly use the partition-specific event-base files together with the corresponding annotation files.

### `total/`

The `total/` directory contains the currently shareable human-value annotations corresponding to the `Total` subsets reported in the paper.

The annotations are separated into the official:

* train partition
* dev partition
* test partition

All protected GUIDs and their corresponding annotations have been removed from the released files.

The train/dev/test GUID assignments in `total/` are also used to construct the corresponding partition-specific event-base files under `event_base/train/`, `event_base/dev/`, and `event_base/test/`.

### `sub/`

The `sub/` directory contains the currently shareable human-value annotations corresponding to the fixed sampled `Sub` subsets reported in the paper.

Before blind filtering, these sampled subsets contain:

* 20,000 annotated unit--actor pairs from train
* 2,500 annotated unit--actor pairs from dev
* 5,000 annotated unit--actor pairs from test

The current public files exclude protected GUIDs. Therefore, the released `sub/` files are filtered versions of the complete sampled subsets used in the reported experiments.

The event-base train/dev/test files are defined according to the full partition assignments under `total/`, rather than independently from the sampled `sub/` subsets. Users working with `sub/` can retrieve the corresponding event record through the shared `guid`.

### `majority-accepted_subset/`

The file:

* `majority-accepted_subset_without_blind_data.json`

contains the currently shareable majority-accepted reference labels derived from the multi-group candidate-acceptance assessment reported in the paper.

The complete majority-accepted reference subset contains 400 evaluated unit--actor pairs covering 187 unique GUIDs. Four GUIDs overlap with the protected blind-evaluation set and are removed from the current public file.

The complete majority-accepted reference subset will be released after the blind evaluation period ends in January 2027.

### `hv_framework/`

The `hv_framework/` directory is unchanged from the previous release.

## File Descriptions

The field definitions below apply across the corresponding train/dev/test files in both `total/` and `sub/`.

For example:

* `total/train_human_value_labels_2ids.json`
* `total/dev_human_value_labels_2ids.json`
* `total/test_human_value_labels_2ids.json`
* `sub/train_human_value_labels_2ids.json`
* `sub/dev_human_value_labels_2ids.json`
* `sub/test_human_value_labels_2ids.json`

all follow the same schema described for `human_value_labels_2ids.json` below.

The same principle applies to the human-readable and reformatted versions.

### 1. `event_base/event_base.json`

This file contains the unified event-structured base data for the currently shareable news articles. Each record corresponds to one article and includes the article text, title, time, actor information, sentence list, and event-centric semantic structures.

Main fields include:

* `guid`: unique article identifier
* `title`: article title
* `content`: raw article text
* `time`: publication date or time information
* `actors`: normalized major social actors appearing in the article
* `sentences`: sentence-level segmented article content
* `subevents`: fine-grained local event units grounded to sentence IDs
* `behaviors`: behavior-based event chains
* `story_narratives`: higher-level narrative groupings over subevents
* `news_type_l1`: coarse news genre label

This file is the structural foundation of the dataset and is used to represent each article as a multi-level event-centric semantic unit.

The same released event-base records are also provided as explicit train/dev/test files under:

* `event_base/train/`
* `event_base/dev/`
* `event_base/test/`

These partition-specific files preserve the original event-base structure and are constructed solely according to the GUID membership of the corresponding full annotation partitions under `total/`.

### 1b. `event_base/event_base_reformatted.json`

This file is a reformatted version of `event_base.json` with more standardized field organization.

Compared with the original version:

* `behaviors` is renamed to `bces`
* `story_narratives` is renamed to `sces`
* each bce entry is explicitly structured with:

  * `unit_id`
  * `bce_title`
  * `behaviors`

For each bce entry:

* `unit_id` is the list of behavior IDs contained in that bce
* `bce_title` is the original behavior-chain title
* `behaviors` contains the original behavior items grouped under that bce

In this reformatted version:

* `bces` correspond to the original `behavior_chains`
* `sces` correspond to the original `story_narratives`

This file is recommended for users who want a cleaner and more uniform event structure for parsing, modeling, or cross-level processing.

The corresponding train/dev/test versions are provided as:

* `event_base/train/train_event_base_reformatted.json`
* `event_base/dev/dev_event_base_reformatted.json`
* `event_base/test/test_event_base_reformatted.json`

They contain the same reformatted event representation, partitioned solely by `guid`.

### 2. `*_human_value_labels_2ids.json`

These files contain the released human value labels in a compact ID-based format. Each record corresponds to one annotated `(unit, actor)` instance.

Main fields include:

* `guid`: article identifier
* `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
* `unit_id`: ID of the target unit within the article
* `actor`: target actor ID
* `aligned`: list of aligned human value IDs
* `contradictory`: list of contradictory human value IDs

Because one article may contain multiple annotated units and multiple actors, these files are expanded: a single `guid` may appear in many rows.

These files are suitable for benchmark training and evaluation, since they provide the released reference labels in a compact machine-readable format.

The same schema is used for:

* `total/train_human_value_labels_2ids.json`
* `total/dev_human_value_labels_2ids.json`
* `total/test_human_value_labels_2ids.json`
* `sub/train_human_value_labels_2ids.json`
* `sub/dev_human_value_labels_2ids.json`
* `sub/test_human_value_labels_2ids.json`

### 2b. `*_human_value_labels_2ids_reformatted.json`

These files are reformatted versions of the corresponding `*_human_value_labels_2ids.json` files.

Compared with the original version:

* unit types are normalized to `article`, `subevent`, `bce`, and `sce`
* `bce` corresponds to the original `behavior_chains`
* `sce` corresponds to the original `story_narratives`
* the original value labels are reorganized into two explicit levels:

  * `l1_label`
  * `l2_label`

Main fields include:

* `guid`: article identifier
* `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
* `unit_id`: ID of the target unit within the article
* `actor`: target actor
* `l1_label`:

  * `aligned`: list of aligned Level-1 value IDs
  * `contradictory`: list of contradictory Level-1 value IDs
* `l2_label`:

  * `aligned`: list of aligned Level-2 value IDs
  * `contradictory`: list of contradictory Level-2 value IDs

These files are recommended for users who want a more explicit hierarchical label structure while preserving the compact machine-readable format.

The same schema is used for the train/dev/test files under both `total/` and `sub/`.

### 3. `*_human_value_labels.json`

These files contain the human value annotations in a richer, more interpretable format. Each record corresponds to one article and groups value annotations by semantic level.

Main sections include:

* `article_human_values`
* `subevents_human_values`
* `behavior_chains_human_values`
* `story_narrative_human_values`

Within each section, annotations are organized by actor and direction:

* `aligned_with_human_values`
* `contradictory_to_human_values`

For each value label, the public release retains:

* `confidence`
* `explanation`

Fields such as `model`, `qa_results`, `human_voting_count`, and `human_voting_num` are not included in the public release.

These files are useful for interpretation, qualitative analysis, and understanding why a particular human value label was assigned.

The same schema is used for:

* `total/train_human_value_labels.json`
* `total/dev_human_value_labels.json`
* `total/test_human_value_labels.json`
* `sub/train_human_value_labels.json`
* `sub/dev_human_value_labels.json`
* `sub/test_human_value_labels.json`

### 3b. `*_human_value_labels_reformatted.json`

These files are reformatted versions of the corresponding `*_human_value_labels.json` files in a flatter and more instance-oriented format.

Each record corresponds to one `(guid, unit_level, unit_id, actor)` instance and groups all associated human values under that instance.

Main fields include:

* `guid`: article identifier
* `unit_level`: semantic level of the annotation (`article`, `subevent`, `bce`, or `sce`)
* `unit_id`: ID of the target unit
* `actor`: target actor
* `human_values`: list of value entries

Each value entry includes:

* `l1_label`: Level-1 value label
* `l2_label`: corresponding Level-2 value label
* `confidence`: retained confidence score
* `explanation`: retained explanation text
* `direction`: `aligned` or `contra`

For reformatted unit identifiers:

* for `article`, `unit_id` is empty
* for `subevent`, `unit_id` is the `subevent_id`
* for `sce`, `unit_id` is the `story_narrative_id`
* for `bce`, `unit_id` is the list of IDs in `behavior_ids_chains`

These files are useful for easier programmatic loading, actor-centered inspection, and modeling pipelines that prefer flatter structures.

The same schema is used for the train/dev/test files under both `total/` and `sub/`.

## Relationship Between the Main Files

All files are aligned through the `guid` field.

### Event structure

The complete currently shareable event-base collection is available in:

* `event_base/event_base.json`
* `event_base/event_base_reformatted.json`

For direct split-wise use, the same event records are additionally organized into:

* `event_base/train/`
* `event_base/dev/`
* `event_base/test/`

The partition-specific event-base files follow the same train/dev/test GUID assignments as the corresponding full human-value annotation files under `total/`.

### Total human-value annotations

Files under `total/` provide the currently shareable annotations from the complete NEVU train/dev/test partitions after protected-GUID filtering.

For example:

* `total/train_human_value_labels_2ids.json` aligns with `event_base/train/train_event_base.json`
* `total/dev_human_value_labels_2ids.json` aligns with `event_base/dev/dev_event_base.json`
* `total/test_human_value_labels_2ids.json` aligns with `event_base/test/test_event_base.json`

The same relationship applies to the reformatted versions.

### Sampled human-value annotations

Files under `sub/` provide the currently shareable portions of the fixed sampled subsets used for the main HVR experiments.

The sampled annotation subsets do not define a separate event-base partition. Their corresponding event records can be retrieved either from the unified event-base files or from the appropriate train/dev/test event-base file using `guid`.

### Majority-accepted reference labels

* `majority-accepted_subset/majority-accepted_subset_without_blind_data.json` provides the currently shareable portion of the majority-accepted reference subset.

In other words:

* use the unified `event_base/event_base.json` or `event_base/event_base_reformatted.json` when a single event collection is preferred,
* use the files under `event_base/train/`, `event_base/dev/`, and `event_base/test/` when explicit partition alignment is preferred,
* use `*_human_value_labels_2ids.json` or `*_human_value_labels_2ids_reformatted.json` for benchmark-style supervised learning and evaluation,
* use `*_human_value_labels.json` or `*_human_value_labels_reformatted.json` for explanation-oriented inspection and qualitative analysis,
* use the shared `guid` field to align event records with human-value annotation records.

## Original and Reformatted Files

The reformatted files are provided for convenience and do not define a different dataset split or annotation standard. They reorganize the same released content into more normalized structures.

In all reformatted files, unit types are consistently represented in lowercase as:

* `article`
* `subevent`
* `bce`
* `sce`

where:

* `bce` corresponds to the original `behavior_chains`
* `sce` corresponds to the original `story_narratives`

Users who prefer the original structure can use the non-reformatted files. Users who want flatter and more standardized formats may use the reformatted files.

The unified and partition-specific event-base files follow the same principle: the train/dev/test versions do not define different event content, but provide the corresponding released event records organized according to the NEVU partition assignments.

## Contents of the Current Release

The current release is constructed from the NEVU data used in the paper after removing all records associated with the 350 protected GUIDs.

The event-base records are provided in two organizational forms:

* a unified collection under `event_base/event_base.json` and `event_base/event_base_reformatted.json`;
* explicit train/dev/test collections under `event_base/train/`, `event_base/dev/`, and `event_base/test/`.

The partition-specific event-base files are constructed from the train/dev/test GUID assignments of the full human-value annotation data under `total/`.

The human-value annotations are organized according to the train/dev/test partitions under `total/` and `sub/`.

Additional preprocessing for the public release remains consistent with the previous release:

* In `event_base.json`, some internal metadata fields are removed.
* In the human-readable human-value annotation files, only the public annotation fields such as `confidence` and `explanation` are retained.
* Reformatted files are additionally provided for easier parsing and more standardized downstream use.

## Protected Blind-Evaluation Data

The current release excludes 350 protected GUIDs and their corresponding annotations.

Protected instances are removed by `guid` from:

* the unified released `event_base/` data,
* the train/dev/test event-base files,
* the train/dev/test files under `total/`,
* the train/dev/test files under `sub/`,
* the released majority-accepted reference subset.

Because some sampled experimental subsets overlap with these protected GUIDs, the current public `sub/` files are not the complete sampled data used for the reported experiments.

The complete protected annotations and associated materials will be released after the blind evaluation period ends in **January 2027**.

## Notes

The `total/` and `sub/` directory names follow the subset terminology used in the paper.

The unified event-base files provide all currently shareable event records in a single collection. For convenience, the same records are also distributed into train/dev/test event-base files according to the GUID assignments of the full annotation partitions under `total/`.

The event-base train/dev/test files therefore align directly with the corresponding `total/` partitions. The `sub/` files remain fixed sampled annotation subsets and can be linked to the appropriate event records through the shared `guid` field.

The reformatted files preserve the same released annotation or event content as their corresponding original-format files while reorganizing it for easier use.

If exact backward compatibility with earlier parsing scripts is important, use the original-format files. If easier loading and more uniform field naming are preferred, use the reformatted files.