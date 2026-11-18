# Week 6 Geocoding and Spatial History

## Overview

This week examines how historical place statements become spatial data. You will work with the teaching dataset of sixteenth-century printed editions used in Weeks 2 and 4. You will normalize selected place statements, reconcile places with Wikidata in OpenRefine, retrieve identifiers and coordinates, validate the resulting data, and create an interactive proportional-symbol map with Python and Folium.

The principal workflow is:

> historical place statement → normalized search value → authority match → coordinates → aggregation → map → historical interpretation

Every arrow represents a decision or transformation. A coordinate is not simply hidden inside a historical source waiting to be extracted. Historical place names can be ambiguous, misspelled, translated, bracketed, inferred, fictitious, or assigned retrospectively by a bibliographer. A successful software match is therefore not necessarily a historically defensible identification.

The tutorial has three required stages:

1. Reconcile place names and retrieve coordinates with OpenRefine and Wikidata.
2. Validate and aggregate the enriched data in Google Colab or Jupyter.
3. Create and interpret an interactive Folium map.

The optional extension turns the map into a more complete interactive webpage.

## Historical and methodological question

The tutorial investigates:

> What spatial pattern is visible among the printing places represented in this teaching sample, and how do normalization, false imprints, uncertain attributions, and unmatched records affect that pattern?

This question is deliberately limited. The map will describe the records in the supplied sample. It will not by itself show the total volume of sixteenth-century printing, the complete geography of the book trade, readership, circulation, religious influence, or the movement of physical books.

## Learning objectives

By the end of the tutorial, you should be able to:

1. Distinguish a source's place statement from a normalized place name and an interpreted geographic entity.
2. Explain what geocoding and reconciliation do.
3. Reconcile place names with Wikidata while reviewing ambiguous candidates.
4. Preserve confirmed, probable, ambiguous, unmatched, and excluded results.
5. Retrieve Wikidata identifiers and coordinate data through OpenRefine.
6. Explain why a gazetteer match remains a claim requiring validation.
7. Validate latitude, longitude, row counts, missing values, and duplicate place assignments with Python.
8. Aggregate edition records by place without losing the original records.
9. Create a proportional-symbol map with Folium.

## Materials and software

You will need:

- `book-sample.csv` from the Week 2 RAWGraphs tutorial;
- OpenRefine, which you installed for Week 3;
- an internet connection for Wikidata reconciliation;
- Google Colab or a local Jupyter environment;
- access to an LLM that can help draft and explain Python code;
- your personal Applied Digital History portfolio repository.

The supplied CSV contains 500 bibliographical records and 31 distinct non-empty place strings. Each row represents one recorded edition in the teaching sample. It does not represent a physical copy or necessarily a unique text.

Wikidata reconciliation and coordinate retrieval require a working internet connection and access to the external service.

## Files to preserve

Do not overwrite the original CSV. By the end of the tutorial, preserve:

- the original `book-sample.csv`;
- an exported coordinate-enriched CSV;
- the completed notebook (`.ipynb`);
- the interactive map (`.html`);
- your written notes and validation results.

---

# Part 1. Understand the spatial data model

## Place statements are not coordinates

The `place` column includes values such as:

```text
Basel
[Basel]
Basel]
Köln
Tübingen]
Roma (=Basel)
Chamogascko (=Basel)
Buyten Strasbourg (=Emden)
```

These values do not all make the same kind of claim.

- `Basel` appears to state a printing place directly.
- `[Basel]` uses brackets to signal that the place was supplied or inferred rather than printed in that form.
- `Basel]` may contain a transcription or data-entry problem.
- `Roma (=Basel)` distinguishes a stated place from an attributed actual place.
- `Chamogascko (=Basel)` contains a fictitious or disguised place statement and a modern bibliographical attribution.

Before obtaining coordinates, decide which historical phenomenon you intend to map:

- the place stated in an imprint;
- the place attributed by a bibliographer;
- the actual place of production, where it can be established;
- or the difference between stated and attributed places.

For the required map, use the best-supported **actual printing location**, while preserving the raw place statement and documenting inferred or false-imprint cases. For example, `Roma (=Basel)` should be mapped to Basel, not Rome, because Basel is the attributed actual printing location in this field.

## Recommended fields

Your working data should eventually contain fields with distinct purposes:

| Field | Meaning |
|---|---|
| `place` | Original place statement in the supplied dataset |
| `place_norm` | Normalized value used for matching |
| `place_interpreted` | Geographic entity selected for this map |
| `place_relationship` | Relationship between raw and interpreted place |
| `match_status` | Status of the authority match |
| `wikidata_id` | Stable Wikidata QID for a confirmed entity |
| `latitude` | Latitude in decimal degrees |
| `longitude` | Longitude in decimal degrees |
| `coordinate_source` | Authority from which coordinates were retrieved |
| `decision_note` | Brief explanation of a non-obvious decision |

Use controlled values where possible.

Suggested `place_relationship` values:

```text
stated
inferred
false_imprint_attributed_to
uncertain
unresolved
```

Suggested `match_status` values:

```text
confirmed
probable
ambiguous
unmatched
not_geocoded
```

These categories are analytical decisions, not facts supplied automatically by Wikidata.

> **Checkpoint 1:** In your notes, explain the difference between `place`, `place_norm`, and `place_interpreted`. Classify `Roma (=Basel)` using one of the proposed `place_relationship` values and explain your decision.

---

# Part 2. Create the OpenRefine project

## Step 1. Import the CSV

1. Start OpenRefine.
2. Select **Create Project**.
3. Choose `book-sample.csv`.
4. Confirm that the first row contains column headings.
5. Keep the original text values as text rather than automatically converting them to dates or numbers.
6. Name the project `Week 6 Printing Places Geocoding`.
7. Create the project.

Confirm that OpenRefine reports **500 rows**.

## Step 2. Inspect the place values

On the `place` column:

1. Select **Facet → Text facet**.
2. Sort the facet by **count**.
3. Record the number of distinct non-empty place strings.
4. Identify at least:

   - one straightforward value;
   - one bracketed value;
   - one malformed value;
   - one value containing an attributed alternative place.

Do not begin by replacing every unusual value. First determine what the notation might mean.

> **Checkpoint 2:** OpenRefine contains 500 rows and approximately 31 distinct non-empty place strings. If your totals differ, inspect the import settings and blank values before continuing.

---

# Part 3. Preserve and normalize the place field

## Step 1. Create `place_norm`

From the `place` column menu, select:

**Edit column → Add column based on this column**

Name the new column:

```text
place_norm
```

Begin with this GREL expression:

```text
value.trim()
```

This removes leading and trailing whitespace without changing the original `place` column.

## Step 2. Normalize only defensible variants

Create a text facet on `place_norm`. Use clustering and individual edits to resolve simple cases such as stray brackets only when their intended place is clear.

Do not use a single expression that automatically removes all content in brackets or after an equals sign. In historical bibliographical data, those symbols may distinguish the stated imprint from a scholarly attribution.

Document at least:

- one normalization you accepted;
- one proposed normalization you rejected;
- one value requiring historical interpretation rather than string cleaning.

## Step 3. Create `place_interpreted`

Copy `place_norm` into a new column named `place_interpreted`:

**Edit column → Add column based on this column**

```text
value
```

Create a text facet on the new column:

1. Open the `place_interpreted` column menu.
2. Select **Facet → Text facet**.
3. In the facet, select a value that requires interpretation, such as `Roma (=Basel)`.
4. OpenRefine will filter the table so that only rows containing that value are displayed.
5. Review the filtered rows and confirm that the same interpretation is appropriate for all of them.
6. Edit one of the displayed cells and replace the value with the actual printing location. For example, replace `Roma (=Basel)` with `Basel`.
7. If the same value occurs in more than one row, select **Apply to all identical cells** instead of **Apply**. This changes every cell containing that identical value.
8. Select the next value that requires interpretation and repeat the process.

Using a facet makes repeated values easier to inspect and edit consistently. Do not assume that every superficially similar value should receive the same interpretation: review the filtered records before changing them.

### Alternative: edit a value directly in the facet

A text facet can also mass-edit identical values without first opening an individual cell:

1. Move the pointer over a value in the `place_interpreted` text facet.
2. Select the **edit** link that appears beside the value.
3. Enter the actual printing location.
4. Confirm the edit.

OpenRefine will replace every identical occurrence of that facet value in the `place_interpreted` column. For example, directly editing the facet value `Roma (=Basel)` to `Basel` changes all identical occurrences at once.

This is faster when you already understand the value and know that every identical occurrence should receive the same interpretation. Selecting the facet value first and inspecting the filtered records is safer when the context might affect the decision. Directly editing the `place_interpreted` facet changes only that derived column; do not use this method to overwrite the original `place` evidence.

Edit `place_interpreted` wherever the geographic entity used for mapping differs from the normalized source statement. For this exercise, the value after the equals sign identifies the attributed actual printing location. Therefore, a record encoded as `Roma (=Basel)` must have `Basel` in `place_interpreted`. Similarly, `Chamogascko (=Basel)` should be interpreted as Basel and `Buyten Strasbourg (=Emden)` as Emden.

Do not erase the raw statement from `place` or the normalized search form from `place_norm`.

## Step 4. Add decision fields

Add blank columns for:

```text
place_relationship
match_status
decision_note
```

Create the columns one at a time:

1. Open the `place_interpreted` column menu.
2. Select **Edit column → Add column based on this column**.
3. Enter `place_relationship` as the new column name.
4. Delete the expression already shown in the expression box and enter:

```text
null
```

5. Check the preview. Every preview value should be blank or `null`.
6. Select **OK** to create the column.
7. Repeat the same procedure from the `place_interpreted` column to create `match_status`.
8. Repeat it once more to create `decision_note`.

The expression `null` creates an empty value for every row. Using `place_interpreted` as the starting column does not copy its contents; it only determines where OpenRefine initially places each new column.

### Fill `place_relationship`

This field describes the relationship between the original `place` statement and the actual printing location recorded in `place_interpreted`.

Use only the following values:

| Value | Use when |
|---|---|
| `stated` | The source data directly names the place used in `place_interpreted`, for example `Basel` → `Basel` |
| `inferred` | The place is supplied or inferred rather than stated straightforwardly, normally indicated by brackets, for example `[Köln]` → `Köln` |
| `false_imprint_attributed_to` | The recorded statement gives a false or disguised imprint and also supplies an attributed actual printing place, for example `Roma (=Basel)` → `Basel` |
| `uncertain` | The relationship between the recorded value and the actual printing location remains doubtful |
| `unresolved` | You cannot identify an actual printing location sufficiently well to map it |

Assign these values with rule-based filters rather than editing 500 rows individually. Before beginning, create a text facet on `place_relationship` and leave it open. Because the column is initially empty, the facet will begin with a single `(blank)` value. As you classify records, the new controlled values will appear in the facet.

Apply the rules in the following order. After each transformation, inspect the affected rows before continuing.

#### 1. Classify attributed actual printing locations

The equals sign identifies records in which the value distinguishes an imprint statement from an attributed actual printing location.

1. Open the original `place` column menu.
2. Select **Text filter**.
3. Enter an equals sign:

   ```text
   =
   ```

4. Confirm that the filter returns only the relevant values, including `Roma (=Basel)`, `Chamogascko (=Basel)`, and `Buyten Strasbourg (=Emden)`.
5. Open the `place_relationship` column menu and select **Edit cells → Transform**.
6. Enter:

   ```text
   "false_imprint_attributed_to"
   ```

7. Check the preview and select **OK**. The active filter limits the transformation to the displayed rows.
8. Remove the text filter before continuing.

#### 2. Classify bracketed inferred places

1. Add a **Text filter** to the original `place` column.
2. Turn on the **regular expression** option.
3. Enter:

   ```text
   ^\[.*\]$
   ```

   This means: begins with `[`, contains any characters, and ends with `]`.

4. Confirm that the results are fully bracketed values such as `[Basel]`, `[Köln]`, and `[München]`.
5. Transform the filtered `place_relationship` cells to:

   ```text
   "inferred"
   ```

6. Remove the text filter before continuing.

Do not use this rule for a value containing only one stray bracket, such as `Basel]`. A malformed value requires separate review.

#### 3. Classify straightforward stated places

Next, identify rows in which the raw place statement is exactly the same as the interpreted actual printing location.

1. Open the original `place` column menu.
2. Select **Facet → Custom text facet**.
3. Enter this GREL expression:

   ```text
   value == cells["place_interpreted"].value
   ```

4. Select `true` in the resulting facet.
5. In the existing `place_relationship` text facet, select `(blank)`. This prevents the new rule from overwriting a relationship already assigned by an earlier step.
   
   If the text facet is not open, use **Facet → Customized facets → Facet by blank (null or empty string)** on `place_relationship`, then select `true`.
6. Confirm that the displayed rows contain straightforward values such as `Basel` → `Basel` and `Köln` → `Köln`.
7. Transform the filtered `place_relationship` cells to:

   ```text
   "stated"
   ```

8. Clear both facets.

#### 4. Review the remainder manually

Select `(blank)` in the existing `place_relationship` text facet. The remaining rows should contain cases that were not safely classified by the three rules, such as malformed values, incomplete parentheses, or unresolved locations.

If the text facet is no longer open, select **Facet → Customized facets → Facet by blank (null or empty string)** on `place_relationship`, then select `true`.

Inspect each remaining distinct `place` value and assign:

- `uncertain` when you have a tentative interpretation but the relationship remains doubtful;
- `unresolved` when no actual printing location can be identified sufficiently well to map;
- another category only when the evidence justifies it.

For repeated values, use a text facet on `place` to isolate the relevant group, then transform the filtered `place_relationship` cells.

Do not use **Apply to all identical cells** to populate a newly created blank decision column. At this point, all its cells are identical blank values, so that command could fill rows outside the place group you intend to classify.

The resulting classifications should include:

| `place` | `place_interpreted` | `place_relationship` |
|---|---|---|
| `Basel` | `Basel` | `stated` |
| `[Basel]` | `Basel` | `inferred` |
| `Roma (=Basel)` | `Basel` | `false_imprint_attributed_to` |
| `Chamogascko (=Basel)` | `Basel` | `false_imprint_attributed_to` |
| `Buyten Strasbourg (=Emden)` | `Emden` | `false_imprint_attributed_to` |

Classify malformed or less obvious values only after inspecting them. A stray bracket may be a transcription problem, but its presence alone does not prove how the original edition presented its printing place.

### Fill `decision_note`

Use `decision_note` to explain non-obvious interpretations. Straightforward `stated` cases can remain blank. Notes should be brief but specific enough that another researcher can reconstruct the decision.

Examples include:

```text
Bracketed place treated as a bibliographical attribution.
Actual printing location taken from the value after the equals sign.
Malformed place statement; interpreted as Basel after comparison with the supplied attribution.
No sufficiently reliable identification; excluded from the map.
```

Use the same facet-and-transform procedure to insert a shared note for identical raw place values. Write different notes when superficially similar records require different reasoning.

Notes are not required. But they are useful for explaining your decision if you return to this work at a later date.

### Leave `match_status` blank for now

Leave `match_status` blank. You will complete it after reconciling `place_interpreted` with Wikidata in Part 4.

> **Checkpoint 3:** The original `place` column is unchanged. You have separate `place_norm` and `place_interpreted` columns, and at least three documented decisions.

---

# Part 4. Reconcile places with Wikidata

## What reconciliation does

Reconciliation attempts to connect a text value in your table to a particular entity in an external authority. In this exercise, a successful match connects a place name to a Wikidata entity with a stable identifier such as `Q1794`.

Reconciliation is not verification. It produces candidates using strings, types, and other structured information. You must decide whether the proposed entity represents the place intended by the historical record and your mapping decision.

## Step 1. Reconcile `place_interpreted`

1. Open the `place_interpreted` column menu.
2. Select **Reconcile → Start reconciling**.
3. Choose the Wikidata reconciliation service.
4. OpenRefine will suggest Wikidata types such as **big city**, **urban municipality in Germany**, and **city**. Select **city**.
5. Do not select **big city** or **urban municipality in Germany**. Those types are too narrow for this dataset, which also contains places in Switzerland, France, Austria, and Italy. **City** is the broader relevant type and can include more specific kinds of cities.
6. Begin reconciliation.

Interface labels may differ slightly between OpenRefine versions. Use the named operation rather than relying only on the exact position of a button.

## Step 2. Review candidates

Do not automatically accept every highest-scoring result.

After reconciliation, cells can appear in several different states.

OpenRefine also automatically creates two facets in the left sidebar:

- **`place_interpreted: judgment`** shows the current reconciliation decision for each cell. `matched` means that one Wikidata entity has been accepted, while `none` means that no candidate has yet been accepted. Depending on the OpenRefine version or interface language, the label may appear as `judgement`.
- **`place_interpreted: best candidate's score`** is a numeric facet showing the reconciliation score assigned to the strongest candidate for each cell. Moving the range sliders lets you inspect higher- or lower-scoring candidates separately.

Use the judgment facet to move between matched and unresolved cells. Use the score facet to prioritize records for inspection, not to declare them correct. A high score can still represent the wrong place, while a correct candidate may receive a lower score because of spelling, language, or incomplete Wikidata information.

### Automatically matched cells

When the reconciliation service is confident, a cell normally displays one linked Wikidata value in dark blue. OpenRefine has already recorded this as a match.

One displayed value does not make the identification historically correct. Hover over the link to inspect the preview, or open the Wikidata record, and confirm that it is the intended city rather than another place with the same or a similar name.

### Cells with several candidates

When OpenRefine cannot select one result confidently, the cell displays multiple Wikidata candidates. Each candidate may include a label, a reconciliation score, and one or two checkmark buttons.

- Select the **candidate label** to open its Wikidata record and inspect it.
- A **single checkmark** accepts that candidate for this cell only.
- A **double checkmark** accepts that candidate for every cell in the column containing the same original `place_interpreted` value.

Because this project contains repeated printing places, normally use the double checkmark after verifying the candidate. For example, once you have established that `Basel` refers to the intended city, the double checkmark applies that Wikidata entity to every identical `Basel` cell. Use the single checkmark only if identical text values refer to different entities in different records.

The reconciliation score ranks candidates according to the service's matching procedure. It is not a percentage probability and does not establish that the highest-scoring candidate is historically correct.

### Cells without an acceptable candidate

Some cells may have no candidates, or all displayed candidates may be wrong. Do not select an incorrect entity merely to complete the map. Leave the cell unresolved. You can isolate these cells later and attempt reconciliation with a broader type, search Wikidata manually, or classify them as `unmatched` or `not_geocoded`.

For each distinct interpreted place:

1. inspect the candidate label;
2. inspect the entity type;
3. inspect the country or geographic context;
4. open the Wikidata record when necessary;
5. check whether the coordinates refer to the intended settlement;
6. confirm, reject, or leave the value unresolved.

Pay particular attention to names shared by several settlements. A high match score measures the reconciliation service's confidence based on available structured data. It does not measure historical truth.

## Step 3. Preserve unresolved cases

Use one of these controlled values:

| Value | Use when |
|---|---|
| `confirmed` | You inspected and accepted a Wikidata entity as the intended actual printing location |
| `ambiguous` | Two or more plausible candidates remain and you cannot justify selecting one |
| `unmatched` | Wikidata produced no acceptable candidate |
| `not_geocoded` | You deliberately did not attempt to assign coordinates, normally because the actual place is unresolved |

Do not treat OpenRefine's numerical reconciliation score as `match_status`. The score ranks computational candidates; the status records your reviewed judgment.

### Assign `confirmed` to the automatically matched group

You do not need to update 500 records individually.

1. In **`place_interpreted: judgment`**, select `matched`.
2. Create or examine a text facet on `place_interpreted`. It shows the distinct place values within the matched group and the number of records assigned to each one.
3. Review those distinct places. Check the Wikidata link for each place rather than inspecting every repeated edition row.
4. Once the displayed matches have been verified, open the `match_status` column menu.
5. Select **Edit cells → Transform**.
6. Enter:

```text
"confirmed"
```

7. Check the preview and select **OK**. Because `judgment = matched` is active, all currently matched rows receive `confirmed` together.

### Inspect the records that were not automatically matched

1. In **`place_interpreted: judgment`**, select `none`.
2. Use the `place_interpreted` text facet to work through the remaining distinct place values, not individual edition rows.
3. Inspect the candidates for each distinct value.
4. If one candidate is correct, use the **double checkmark** to apply it to every identical value. Those rows will change from `none` to `matched` and disappear from the current view.
5. After accepting the defensible candidates, select `matched` in the judgment facet.
6. In a text facet on `match_status`, select `(blank)`. This isolates newly matched rows that have not yet received a status.
7. Transform their `match_status` cells to:

   ```text
   "confirmed"
   ```

### Classify the unresolved group

Return to `none` in the judgment facet. The remaining cells have no accepted Wikidata entity.

- Assign `ambiguous` when multiple plausible candidates remain.
- Assign `unmatched` when Wikidata provides no acceptable candidate.
- Assign `not_geocoded` when you deliberately did not attempt a coordinate match because the actual printing location was already classified as unresolved.

Use the `place_interpreted` facet to select one distinct unresolved value at a time, then transform `match_status` for all filtered rows. Leave an uncertain identification unresolved rather than choosing an entity merely to complete the map.

Finally, clear the reconciliation facets and create a text facet on `match_status`. Confirm that every row belongs to one of the four controlled categories and that no unexpected `(blank)` values remain.

> **Checkpoint 4:** Every distinct place has been reviewed. At least one candidate decision is documented, and ambiguous or unmatched cases have not been silently discarded.

---

# Part 5. Retrieve Wikidata identifiers and coordinates

## Step 1. Add the Wikidata identifier

From the reconciled `place_interpreted` column menu, select:

**Reconcile → Add entity identifiers column**

Name the column:

```text
wikidata_id
```

Confirmed entities should receive QIDs. Unmatched cells should remain blank.

## Step 2. Add coordinates from reconciled values

Open the dropdown menu on the reconciled `place_interpreted` column, then select:

**Edit column → Add columns from reconciled values**

Search for and add:

```text
coordinate location
country
```

These are Wikidata properties `P625` and `P17`.

## Step 3. Export the enriched data

1. Clear all temporary facets and filters.
2. Confirm that the project still represents 500 edition records.
3. At top right, select **Export → Comma-separated value**.
4. Save the file as:

```text
book_places_wikidata.csv
```

Depending on the OpenRefine version and export format, the coordinate may appear as a single geographic value rather than separate latitude and longitude columns. Preserve it as exported. Python can inspect and separate it in the next stage.

> **Checkpoint 5:** Your exported CSV contains all original columns, `place_norm`, `place_interpreted`, `match_status`, `wikidata_id`, and coordinate information. The original CSV remains unchanged.

---

# Part 6. Understand the mapping tools

The tools used in this tutorial have different roles:

| Tool | Role |
|---|---|
| **Folium** | Python library that constructs interactive web maps |
| **Leaflet** | JavaScript mapping library used by the HTML generated by Folium |
| **OpenFreeMap Natural Earth** | Keyless hosted raster basemap used as the background in this tutorial |
| **OpenStreetMap** | Collaborative geographic data project; its data is used in many other basemaps, although the low-zoom Natural Earth layer used here is a separate dataset |

These are not competing products in this workflow. Folium generates a Leaflet map. Leaflet loads the OpenFreeMap Natural Earth background tiles.

The background and your historical data are separate layers. The present-day basemap supplies modern roads, borders, names, and settlement extents. It should not be mistaken for a representation of sixteenth-century political geography.

---

# Part 7. Prepare the Colab or Jupyter notebook

## Step 1. Create the notebook

In Google Colab:

1. create a new notebook;
2. rename it `week_06_geocoding.ipynb`;
3. add a text cell containing the historical question and a description of the input file.

Do not also upload `book_places_wikidata.csv` through the Files sidebar. The first Python code will ask you to select and upload the file. Uploading it through both methods can create a duplicate file in the Colab runtime.

You may instead use local Jupyter. Tell the LLM which environment you are using because file selection and download procedures differ.

## Step 2. Ask the LLM to inspect, not assume

Use a prompt based on the following template. Change the coordinate-column name only after inspecting the exported CSV.

```text
I am a digital history student with no previous programming experience,
working in [Google Colab / Jupyter]. I have a CSV named
"book_places_wikidata.csv" exported from OpenRefine.

Write beginner-friendly Python using pandas. If I am using Google Colab,
begin by asking me to select and upload the CSV once, then load the uploaded
file. If I am using local Jupyter, assume that the CSV is in the same folder
as the notebook and load it directly. Do not create or upload a second copy
of the input file.

Load the CSV without modifying it and print:

1. the number of rows and columns;
2. the complete list of column names;
3. the first five rows;
4. the number of unique edition_id values;
5. missing-value counts for place, place_interpreted, wikidata_id, and the
   coordinate column;
6. the distinct sample values from the coordinate column.

Do not drop or alter rows. Do not guess the coordinate format. Explain each
validation output in plain language.

Excerpt of first 5 rows from the csv:
[insert first 5 rows here]
```

Run the inspection before requesting coordinate parsing. The actual values reveal whether the coordinates are already separated or stored in a combined format.

> **Checkpoint 6:** The notebook reports 500 input rows and 500 unique `edition_id` values. You have inspected the actual coordinate representation.

---

# Part 8. Parse and validate coordinates

## Step 1. Request format-specific parsing

Continue in the same LLM conversation used in Part 7. The LLM already has the CSV excerpt and the inspection results, including the coordinate column name and representative values. Send this follow-up prompt:

```text
I am working in [Google Colab / Jupyter] with a pandas dataframe named books.
It contains 500 bibliographical records, with one row per recorded edition.
The CSV has already been uploaded and loaded, so do not ask me to upload it
again.

Using the coordinate column name, sample values, and inspection output from
the preceding task in this conversation, write one beginner-friendly Python
code cell that parses the observed coordinate format.

Requirements:

1. Begin with a short comment stating the coordinate format you identified
   and whether its source order is longitude–latitude or latitude–longitude.
2. If the examples do not establish the order unambiguously, do not guess.
   Explain what additional information is needed instead of producing code.
3. Preserve the original coordinate column unchanged.
4. Create new numerical columns named latitude and longitude.
5. Create a column named coordinate_parse_status that distinguishes
   successful parses, missing source values, format errors, and coordinates
   outside the valid numerical ranges.
6. Leave latitude and longitude blank when parsing fails. Do not replace
   failures with zero and do not delete any row.
7. Check that latitude is between -90 and 90 and longitude is between -180
   and 180.
8. Print the input and output row counts and assert that they are equal.
9. Print the number of unique edition_id values and confirm that edition_id
   remains unique.
10. Print the number and percentage of rows with usable coordinates.
11. Print every distinct original coordinate value that could not be parsed,
    together with its coordinate_parse_status.
12. List distinct place_interpreted values without usable coordinates.
13. Report any place_interpreted value assigned more than one coordinate
    pair.
14. Report any coordinate pair assigned to more than one place_interpreted
    value.
15. Print counts by match_status and coordinate_parse_status.
16. Display representative rows containing place, place_interpreted,
    match_status, the original coordinate value, latitude, longitude, and
    coordinate_parse_status.
17. Do not overwrite books or the original CSV. Store the validated result
    in a new dataframe named books_coordinates.

After the code, explain each validation output in plain language and identify
which results require manual review.
```

Do not accept code that extracts the first two numbers from every string without explaining whether they represent longitude–latitude or latitude–longitude. Geographic formats do not all use the same order.

## Step 2. Validate the coordinate ranges

Run the generated code and confirm that it checks:

```text
-90 ≤ latitude ≤ 90
-180 ≤ longitude ≤ 180
```

Read every reported failure and inconsistency before continuing. If the code reports a place with multiple coordinate pairs, reversed-looking coordinates, changed row counts, or lost edition identifiers, inspect the relevant source values and correct the parsing logic rather than suppressing the warning.

Coordinates inside the possible numerical range are not necessarily correct. A transposed coordinate can still produce two valid numbers, and the wrong settlement can have perfectly valid coordinates.

## Step 3. Manually verify representative places

Inspect a few places to ensure the coordinates are correct. Paste the coordinates into Google and see if the correct city is returned.

> **Checkpoint 7:** The row count and edition identifiers are unchanged. Invalid or missing coordinates are reported rather than silently deleted, and at least a few representative cases have been checked.

---

# Part 9. Aggregate editions by place

Plotting all 500 edition rows would place many markers directly on top of one another. Create one row per interpreted place and coordinate pair, with a count of the recorded editions assigned there.

Use or adapt this prompt:

```text
Continue from the preceding task in this conversation. Using the validated
dataframe books_coordinates that we just created, write beginner-friendly
Python that creates a new aggregated dataframe named city_counts for mapping.

Create one row for each unique combination of place_interpreted, latitude,
and longitude. Add a column named editions containing the number of edition
records assigned to that place and coordinate pair.

Requirements:

1. Do not modify, overwrite, or remove rows from books_coordinates.
2. Use only records with usable numerical latitude and longitude in the
   aggregated map table.
3. Report how many records are excluded because either coordinate is missing
   or invalid, and list their distinct place_interpreted and match_status
   values.
4. Count edition_id values and confirm that edition_id is unique in the
   original dataframe.
5. Check whether one place_interpreted value has been assigned more than one
   coordinate pair. Print any such cases for review rather than silently
   combining them.
6. Print city_counts sorted from the largest to the smallest edition count.
7. Print the number of rows with usable coordinates and the sum of the
   editions column. Assert that those two totals are equal.
8. Save city_counts as "printing_places_aggregated.csv" without including a
   pandas index column.
9. Explain in plain language what one row represents in books_coordinates
   and what one row represents in city_counts.
```

The aggregation may exclude records without usable coordinates from the map table, but it must not delete them from `books_coordinates` or the original `books` dataframe.

Confirm from the printed validation results that the sum of `editions` equals the number of edition rows with usable coordinates, not necessarily all 500 records.

If not automatically saved, save the aggregated table as:

```text
printing_places_aggregated.csv
```
You made need to click the refresh icon on the sidebar on the left to see the newly created file.

> **Checkpoint 8:** You can explain what one row represents in the original table and what one row represents in the aggregated table.

---

# Part 10. Create a proportional-symbol map with Folium

In this task, you will use the aggregated `city_counts` dataframe to create an interactive map of printing locations. Each city will be represented by a circle. Cities assigned more recorded editions will have larger circles. Viewers will be able to hover over or select a circle to identify the place and see its edition count.

Folium will generate the interactive map from the dataframe and save it as an HTML file that can be opened in a web browser.

## Step 1. Prompt the LLM

Use or adapt this prompt:

```text
Continue from the preceding task in this conversation. Using the city_counts
dataframe that we just created, write beginner-friendly Python using folium
to create an interactive map.

Requirements:

1. Center the initial view on Central Europe.
2. Do not use Folium's default basemap. Create the map with no initial tiles,
   then add a custom Folium tile layer using
   `https://tiles.openfreemap.org/natural_earth/ne2sr/{z}/{x}/{y}.png`.
   Set its name to `OpenFreeMap Natural Earth`, its maximum zoom to 6, and
   include visible attribution to OpenFreeMap and Natural Earth. This public
   layer does not require an account or API key. Keep the initial zoom at 5
   or lower because this is a low-resolution contextual basemap.
3. Create one folium.CircleMarker per row.
4. Set each radius to 3 + 2.2 * sqrt(editions), so area is approximately
   proportional to the edition count.
5. Use a partly transparent fill and a visible border.
6. Add a tooltip containing the place and edition count.
7. Add a popup containing the place, count, latitude, and longitude.
8. Fit the final view to the plotted points if possible.
9. Save the result as "printing_places_map.html".
10. Display the map in the notebook.
11. Do not modify city_counts.
```

## Step 2. Inspect the map

Do not treat successful rendering as validation.

Check:

- whether Central European places appear in plausible locations;
- whether latitude and longitude appear to have been reversed;
- whether every mapped place has a tooltip;
- whether the largest circle corresponds to the largest count;
- whether the smallest circles remain visible;
- whether the basemap attribution is visible;
- whether the HTML file was saved successfully.

## Step 3. Validate marker values

Choose at least five markers and compare their displayed counts with `printing_places_aggregated.csv`. Include the largest place and at least one place with a small count.

> **Checkpoint 9:** The interactive map opens, marker counts match the aggregated CSV, and marker area—not radius—is intended to represent the count.

---

# Part 11. Interpret the map historically

Pretend this dataset accurately reflects the historical record. Write a short interpretation based on the map.


---

# Part 12. Optional extension: turn the map into an interactive webpage

The Folium output is already an HTML webpage, but it contains little beyond the map. Use an LLM to create a new HTML file that preserves and displays the interactive map while adding other ways to explore or understand the data. Give the LLM the existing map HTML and any data files it needs.

You might add a searchable table of the original books dataset, a dashboard containing summary information and charts, controls that connect the table to the map, explanatory text, or another creative feature. Decide what would help a reader investigate the historical data rather than adding features only for decoration.

Save the result as:

```text
printing_places_dashboard.html
```

---

# Files to submit

Upload the following to the `weekly-work/week-06` folder in your personal portfolio:

## Required

- `book_places_wikidata.csv`
- `week_06_geocoding.ipynb`
- `printing_places_aggregated.csv`
- `printing_places_map.html`

## Journal
Upload your journal entry identifying your progress and workflow and any difficulties or thoughts you have.

## Optional, if completed

- `printing_places_dashboard.html`

---

# Troubleshooting

## OpenRefine reports the wrong row count

Return to the import settings. Confirm that the first row was interpreted as headings and that the comma delimiter was detected. The project should begin with 500 edition rows.

## Wikidata reconciliation produces irrelevant entities

Check the selected entity type. Reconcile against cities or settlements rather than all Wikidata entities. Inspect whether an ambiguous name needs geographic context. Do not accept a candidate simply because it has the highest score.

## Wikidata reconciliation or data extension is unavailable

Save your work, record the error, and try again later. External availability is outside your control; documenting the failure is part of the workflow.

## A place has several coordinate values

Do not silently take the first. Open the Wikidata item and inspect the statements, ranks, and represented entities. Record your decision or leave it unresolved.

## The Python code cannot find the CSV

Check the exact filename and current notebook file list. Colab runtime storage is temporary; upload the file again after a runtime restart.

## Latitude and longitude appear reversed

Central European latitudes are generally around the high 40s or low 50s north, while longitudes are generally single digits or teens east. This is only a diagnostic clue, not a substitute for validation. Inspect the source coordinate format and correct the parser rather than swapping columns blindly.

## The map is blank

Print `city_counts` and check whether it contains rows with numerical latitude and longitude. Inspect notebook errors. A map object may render even when no markers were successfully added.

## The basemap displays a 403 message or asks for an API key

Check which tile provider the generated code uses. Folium's default `OpenStreetMap` tiles may be blocked when a map is embedded in Colab or opened directly from a local `file://` HTML file. `CartoDB Positron` now requires an API key and should not be used for this exercise.

Ask the LLM to make the smallest correction: create the Folium map with `tiles=None`, then add a custom `folium.TileLayer` using:

```python
tiles="https://tiles.openfreemap.org/natural_earth/ne2sr/{z}/{x}/{y}.png"
```

The layer should have `max_zoom=6`, a descriptive name, and visible attribution to OpenFreeMap and Natural Earth. It does not require an API key. Do not remove the attribution or attempt to bypass another provider's access policy. Changing the tile provider changes the modern background map, not the historical coordinates or edition counts plotted above it.

## The circles are too large or too small

Adjust the constants in:

```python
3 + 2.2 * math.sqrt(editions)
```

Keep the square-root relationship. Record any changed constants because they affect the visual representation.

## The HTML map does not display on GitHub

GitHub normally displays HTML source rather than executing an interactive page. Download the file and open it locally in a browser. The notebook should also contain a rendered version when saved with its outputs.

# Further reference

- [OpenRefine: Reconciling](https://openrefine.org/docs/manual/reconciling)
- [Wikidata: Data access](https://www.wikidata.org/wiki/Help:Data_access)
- [Wikimedia API rate limits](https://www.mediawiki.org/wiki/Wikimedia_APIs/Rate_limits)
- [Folium user guide](https://python-visualization.github.io/folium/latest/user_guide.html)
- [Leaflet documentation](https://leafletjs.com/reference.html)
- [OpenFreeMap](https://openfreemap.org/)
- [OpenStreetMap tile usage policy](https://operations.osmfoundation.org/policies/tiles/)

