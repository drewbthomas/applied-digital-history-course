# Week 3 Tutorial: Cleaning and Normalizing Historical Data with OpenRefine

## Overview

Historical datasets are rarely ready for analysis. Names may appear in several forms, dates may be uncertain, places may be recorded in different languages, and one column may contain several kinds of information. Cleaning these inconsistencies is not a neutral or purely mechanical task. Every change can affect what later searches, counts, maps, or visualizations appear to show.

In this tutorial, you will use OpenRefine to examine and clean a synthetic dataset describing historical photographs. The individual records are invented, although some names and places correspond to real people and locations.

You will:

- install and start OpenRefine;
- import a CSV file as a new project;
- use facets to diagnose patterns before changing the data;
- preserve original values while creating normalized columns;
- trim whitespace and normalize selected categories;
- use clustering to find possible variants of names and places;
- represent date ranges and uncertainty explicitly;
- examine the consequences of splitting multi-valued cells;
- identify duplicate candidates;
- export cleaned data and a record of your operations.

## Files and software

You need:

1. the course file `historic_photographs_sample_dataset.csv`;
2. a current web browser, preferably Chrome, Edge, Firefox, or Safari;
3. OpenRefine.

Create a folder on your computer called `week-03-openrefine`. Save the dataset and your exported work in this folder.

> **Important:** Do not edit the original CSV file. OpenRefine creates its own project and does not need to overwrite the source file.

---

# Part 1. Install and start OpenRefine

OpenRefine is a free, open-source application. It runs locally on your computer. Although you use it through a browser window, the data in this tutorial is processed on your computer rather than uploaded to a website.

Download the latest stable release from the [official OpenRefine download page](https://openrefine.org/download). This tutorial relies on OpenRefine 3.10.0. If a later stable version is available, use it unless otherwise instructed.

## Windows

1. Open the [OpenRefine download page](https://openrefine.org/download).
2. Under the latest stable release, download the **Windows installer**.
3. Open the downloaded `.exe` file.
4. If Windows displays a security prompt, check that the publisher and filename refer to OpenRefine before continuing.
5. Follow the installer instructions.
6. Open **OpenRefine** from the Start menu.
7. Keep the OpenRefine program or console window open. A browser should open automatically.

The Windows installer includes the required Java runtime. If the installer does not work, the download page also offers a ZIP package. Extract the entire ZIP file before running `openrefine.exe`; do not run it from inside the compressed archive.

## macOS

1. Open the [OpenRefine download page](https://openrefine.org/download).
2. Download the **Mac OS** `.dmg` file for the latest stable release.
3. Open the downloaded file.
4. Drag the OpenRefine icon into the **Applications** folder.
5. Open **Applications**, then start **OpenRefine**.

If macOS says that the developer cannot be verified:

1. select **Cancel**;
2. Control-click or right-click the OpenRefine application;
3. select **Open**;
4. select **Open** again in the confirmation window.

The macOS package includes Java.

## Linux

The Linux package requires Java. Follow the current instructions on the [official installation page](https://openrefine.org/docs/manual/installing), because package names differ between distributions.

For the downloaded `.tar.gz` package, the general procedure is:

1. install a compatible Java runtime using your distribution's package manager;
2. download the Linux `.tar.gz` file from the OpenRefine website;
3. extract the archive;
4. open a terminal in the extracted folder;
5. run:

   ```bash
   ./refine
   ```

Keep the terminal window open while you work.

## Confirm that OpenRefine is running

OpenRefine should open in your browser at an address similar to:

```text
http://127.0.0.1:3333/
```

This is a local address on your own computer. If the browser does not open automatically, start OpenRefine first and then enter that address in your browser.

You should see a page with options such as **Create Project**, **Open Project**, and **Import Project**.

> **Checkpoint 1:** Do not continue until you can see the OpenRefine start page.

---

# Part 2. Create the project

## Step 1. Select the workbook

1. Select **Create Project**.
2. Select **This Computer**.
3. Select **Choose Files** or **Browse**.
4. Find and select `historic_photographs_sample_dataset.csv`.
5. Select **Next**.

## Step 2. Check the import preview

Before creating the project, inspect the preview.

Confirm that:

- the first row has become the column-header row;
- the first column is `record_id`;
- the last column is `lcsh_subjects`;
- the preview contains 100 data rows;
- values such as `PHO-001` remain text;
- the dates remain as supplied text rather than being automatically converted.

If OpenRefine offers an option such as **Parse cell text into numbers, dates, ...**, turn it **off** for this exercise. We want to inspect and interpret the supplied date strings before converting anything.

## Step 3. Name and create the project

1. Name the project:

   ```text
   Week 3 – Historical Photograph Metadata – YOUR NAME
   ```

2. Select **Create Project**.

You should now see the project grid.

> **Checkpoint 2:** At the top of the grid, OpenRefine should report 100 rows. If it does not, return to the import screen and check the selected worksheet and header settings.

---

# Part 3. Become familiar with the interface

Before changing anything, locate these parts of the interface:

- the **All** column at the far left;
- the dropdown arrow beside each column name;
- the **Facet/Filter** panel on the left;
- the **Undo/Redo** tab on the left;
- the switch between **rows** and **records** near the top of the grid;
- the **Export** menu in the upper-right area.

Use the page controls to inspect several parts of the dataset. Do not correct anything yet.

Consider:

1. What does one row represent?
2. Which columns describe the historical object?
3. Which columns describe its archival or digital representation?
4. Which columns appear suitable for a controlled vocabulary?
5. Which columns contain uncertainty that should not simply be deleted?

Write brief answers in your journal entry.

---

# Part 4. Diagnose the data with facets

A facet summarizes the values in a column and lets you filter the rows. It is useful for diagnosis because it reveals spelling variants, inconsistent categories, blanks, and unexpectedly rare values.

## Step 1. Create a facet for countries

1. Open the dropdown menu on `country_raw`.
2. Select **Facet → Text facet**.
3. Examine the facet in the left panel.
4. Sort the facet by **count** and then by **name**.

You should see approximately 48 distinct non-blank values. Look for variations such as:

- full names and abbreviations;
- trailing spaces;
- names in different languages;
- `unknown` and blank values.

Do not merge or rename anything yet.

## Step 2. Add three more facets

Repeat the procedure for:

- `rights_raw`;
- `object_type_raw`;
- `caption_language_raw`.

Expected starting points are approximately:

| Column | Distinct non-blank values |
|---|---:|
| `country_raw` | 48 |
| `rights_raw` | 10 |
| `object_type_raw` | 10 |
| `caption_language_raw` | 15 |

Small differences can result from OpenRefine version or import settings. Investigate a difference rather than forcing your result to match these numbers.

## Step 3. Use facets together

1. In the `rights_raw` facet, select one value, such as `Rights unclear`.
2. Observe that the grid now displays only matching rows.
3. Examine how the other facets change when this filter is active.
4. Select **reset** or remove the selection before continuing.

Facets can therefore reveal relationships among fields, not only problems within one field.

### Pause and reflect

Record two examples for each category:

- a difference that is probably typographical;
- a difference that probably represents two labels for the same concept;
- a difference that should not be resolved without more evidence.

> **Checkpoint 3:** You have diagnosed the data but have not edited a value.

---

# Part 5. Preserve original and normalized values

The columns ending in `_raw` contain the supplied values. Do not overwrite them. Instead, create new columns for your normalized values.

This makes the transformation inspectable:

- the raw value preserves the evidence supplied to you;
- the normalized value records your interpretation;
- the OpenRefine history records the operation.

## Step 1. Create `title_norm`

1. Open the dropdown menu on `title_raw`.
2. Select **Edit column → Add column based on this column...**.
3. Enter this new column name:

   ```text
   title_norm
   ```

4. In the expression box, enter:

   ```grel
   value.trim()
   ```
   trim() removes any spaces that are at the beginning or end of the field value

5. Check the preview to confirm that leading and trailing spaces disappear while the wording remains unchanged.
6. Select **OK**.

## Step 2. Create `creator_norm`

Repeat the same procedure from `creator_raw`:

- new column name: `creator_norm`
- expression: `value.trim()`

## Step 3. Create `place_norm`

Repeat the procedure from `place_raw`:

- new column name: `place_norm`
- expression: `value.trim()`

## Step 4. Compare raw and normalized columns

Move each normalized column beside its raw source if OpenRefine did not place it there:

1. open the normalized column's dropdown menu;
2. select **Edit column → Move column left** or **Move column right** as needed.

Inspect rows where the raw value contains visible extra spaces.

Do **not** automatically convert every title or name to title case. A case transformation can damage initials, acronyms, name particles, and intentional transcription choices. Cleaning should solve a defined problem, not merely make values look more uniform.

> **Checkpoint 4:** The three `_raw` columns still exist, and each has a corresponding `_norm` column.

---

# Part 6. Normalize controlled categories

We will create a normalized object-type column. This is safer than changing `object_type_raw` directly.

## Step 1. Copy and trim the object type

1. From `object_type_raw`, select **Edit column → Add column based on this column...**.
2. Name the column:

   ```text
   object_type_norm
   ```

3. Use:

   ```grel
   value.trim().toLowercase()
   ```

4. Select **OK**.

## Step 2. Examine the new facet

1. Create a text facet on `object_type_norm`.
2. Compare it with the facet for `object_type_raw`.

Trimming and lowercasing should combine purely typographical variants. They will not decide whether terms such as `photo`, `photograph`, `photo print`, and `gelatin silver print` should be treated as equivalent.

## Step 3. Make an explicit vocabulary decision

Use the facet to normalize only clearly equivalent general labels:

1. hover over a value in the `object_type_norm` facet;
2. select **edit**;
3. enter the chosen value;
4. confirm the edit.

For example, you may decide that `photo` and `photograph` should both become `photograph`. Do not merge a process-specific term such as `albumen print` into `photograph` unless your stated data model deliberately uses only broad categories.

In your notes, record:

- the values you merged;
- the preferred value you selected;
- why that level of specificity is appropriate.

---

# Part 7. Cluster creator names

Clustering suggests groups of similar strings. It does not determine whether two values refer to the same historical person.

## Step 1. Open the clustering window

1. Open the dropdown menu on `creator_norm`.
2. Select **Edit cells → Cluster and edit...**.
3. Begin with the **key collision** method and the **fingerprint** keying function.
4. Select **Cluster**.

Depending on the data and version, you may see suggested groups involving capitalization, punctuation, or reordered words.

## Step 2. Evaluate the suggestions

For every proposed cluster, ask:

1. Are these strings only typographical variants?
2. Could they refer to different people?
3. Is an initial sufficient evidence of identity?
4. Which form should be retained, and why?

Select only clusters you can justify. Leave uncertain clusters unselected.

Choose a preferred value by selecting an existing form or typing a new one. Then select **Merge selected & re-cluster**.

## Step 3. Try another method

Try at least one nearest-neighbor method. Select **Nearest Neighbor** from the **Method** dropdown. Compare its suggestions with fingerprint clustering.

Do not assume that a cluster is correct because OpenRefine produced it. Clustering measures features of strings. It does not know the biographies of the photographers.

## Step 4. Record one rejection

In your notes, record at least one cluster you rejected or left unresolved. Explain what additional evidence would be needed to merge it responsibly.

> **Checkpoint 5:** You have accepted only clusters that you can justify and have documented at least one non-merge.

---

# Part 8. Cluster and normalize place names

## Step 1. Run clustering

1. Open `place_norm`.
2. Select **Edit cells → Cluster and edit...**.
3. Try fingerprint clustering and at least one nearest-neighbour method.
4. Merge only clearly typographical variants.

## Step 2. Recognize the limit of clustering

Values such as the following may denote the same place without being syntactically similar:

- `Wien`, `Vienna`, and `Vienne`;
- `Köln`, `Koeln`, and `Cologne`;
- `Lisboa`, `Lisbon`, and `Lissabon`.

Clustering may identify some spelling variants, but it should not be expected to translate place names or establish historical equivalence. Resolving multilingual place names requires a mapping decision, a gazetteer, or reconciliation against an authority.

## Step 3. Make one documented manual normalization

Choose one group of multilingual place variants that you are confident represents the same city.

1. Create a text facet on `place_norm`.
2. Use the facet's **edit** option to replace each selected variant with one preferred form.
3. Record the mapping in your notes.

Your preferred label is a present-day analytical choice. It does not make the other historical or linguistic forms incorrect. The raw column preserves them.

---

# Part 9. Normalize dates while preserving uncertainty

The `date_raw` column contains years, ranges, complete dates, approximate dates, question marks, and dates in square brackets. Converting all of these to one exact date would create false precision.

We will derive three fields:

- `year_start`;
- `year_end`;
- `date_certainty`.

The original string will remain in `date_raw`.

## Step 1. Create `year_start`

1. Open the dropdown menu on `date_raw`.
2. Select **Edit column → Add column based on this column...**.
3. Name the new column:

   ```text
   year_start
   ```

4. Enter this expression:

   ```grel
   with(value.find(/\d{4}/), years,
     if(years.length() > 0, years[0].toNumber(), null)
   )
   ```

5. Inspect the preview carefully.
6. Select **OK** only if the expression extracts the first four-digit year and leaves values without one blank.

## Step 2. Create `year_end`

Repeat the process from `date_raw` with:

- new column name: `year_end`
- expression:

   ```grel
   with(value.find(/\d{4}/), years,
     if(years.length() > 1,
       years[1].toNumber(),
       if(years.length() > 0, years[0].toNumber(), null)
     )
   )
   ```

A single year will therefore have the same start and end. A range will have different values.

## Step 3. Create `date_certainty`

Create another column from `date_raw`:

- new column name: `date_certainty`
- expression:

   ```grel
   if(value.contains("?"), "uncertain",
     if(value.toLowercase().startsWith("c."), "approximate",
       if(value.startsWith("["), "supplied/inferred",
         if(value.find(/\d{4}/).length() > 1, "range", "exact")
       )
     )
   )
   ```

Use the preview before applying it. The order of these tests matters. A more complex historical dating system might require several independent fields rather than one category.

## Step 4. Check representative examples

Confirm that the following kinds of values are represented as intended:

| Raw form | Expected start/end | Expected certainty |
|---|---|---|
| `1891` | 1891 / 1891 | exact |
| `1892-1893` | 1892 / 1893 | range |
| `1896?` | 1896 / 1896 | uncertain |
| `[1924]` | 1924 / 1924 | supplied/inferred |
| `c. 1935` | 1935 / 1935 | approximate |

If you find an exception, do not silently force it into the system. Record it and explain how the model would need to change.

---

# Part 10. Examine multi-valued subjects

The `subject_raw` column combines multiple concepts using inconsistent separators. These include slashes, vertical bars, semicolons, commas, words such as `and`, and occasionally hyphens.

Not every separator has the same meaning. For example, a hyphen may be part of one term rather than a boundary between two terms.

## Step 1. Create `subject_norm`

From `subject_raw`, add a new column called `subject_norm` with:

```grel
value.trim()
```

## Step 2. Normalize only unambiguous separators

On `subject_norm`:

1. select **Edit cells → Transform...**;
2. enter:

   ```grel
   value.replace("|", ";").replace("/", ";")
   ```

3. inspect the preview;
4. select **OK**.

This transformation deliberately does not replace commas, hyphens, or the word `and`. Their meaning requires inspection.

## Step 3. Test splitting the cells

1. Open the `subject_norm` dropdown menu.
2. Select **Edit cells → Split multi-valued cells...**.
3. Enter a semicolon (`;`) as the separator.
4. Confirm the operation.
5. Switch between **rows** and **records**.

Observe what happened:

- a photograph with several subject terms now occupies several rows;
- the rows remain grouped as one record;
- counting rows no longer gives the number of photographs.

This is a change to the structure of the data, not simply a change to its appearance.

## Step 4. Undo the split

For the remainder of this tutorial, return to one row per photograph:

1. open the **Undo/Redo** tab;
2. locate the split operation;
3. select the step immediately before it.

Confirm that the project again contains 100 rows.

In your notes, answer:

> When would a separate subject table be preferable to storing several subjects in one cell?

---

# Part 11. Identify duplicate candidates

A duplicate-looking value does not necessarily describe a duplicate historical object. Two records might represent:

- an accidentally repeated database entry;
- two files of the same scan;
- two scans of one physical print;
- two prints made from one negative;
- related versions or crops;
- distinct photographs with similar supplied titles.

We will therefore identify **duplicate candidates**, not automatically delete duplicates.

## Step 1. Find repeated filenames

1. Open the `filename_raw` dropdown menu.
2. Select **Facet → Customized facets → Duplicates facet**.
3. In the new facet, select `true`.
4. Inspect the matching records and their notes.
5. Reset or remove the facet selection.

## Step 2. Find repeated normalized titles

Create a duplicates facet on `title_norm`. Inspect the results.

Ask whether identical titles are sufficient evidence that the records describe the same object.

## Step 3. Create a comparison key

From `title_norm`, select **Edit column → Add column based on this column...**.

1. Name the column:

```text
duplicate_key
```

2. Expression:

```grel
cells["title_norm"].value + " || " +
cells["creator_norm"].value + " || " +
cells["date_raw"].value
```
3. Select **OK** to create the column

4. Create a duplicates facet on `duplicate_key` and inspect any matches.

This key is a diagnostic device. It is not a universally valid definition of identity. It will miss duplicates with different transcriptions, and it may group distinct objects whose descriptive metadata happens to match.

## Step 4. Classify two candidates

Choose two duplicate candidates and record:

- the evidence suggesting duplication;
- the evidence against treating them as identical;
- your provisional decision;
- what additional source or metadata would help resolve the case.

Do not delete any rows.

---

# Part 12. Reconciliation

To complete this section, you will need an internet connection.

Reconciliation attempts to match text values to entities in an external authority such as Wikidata. It is useful for linking variant names to identifiers, but it remains a semi-automated process requiring human judgment.

## Reconcile a small place sample

1. Create a text facet on `place_norm`.
2. Select one or two place values so that only a small set of rows is visible. (To select multiple options at once, select **include** for each value in the facet insted of selecting the name itself)
3. Open the `place_norm` dropdown menu.
4. Select **Reconcile → Start reconciling...**.
5. Choose Wikidata if it is available.
6. Choose **Next**.
7. Reconcile against an appropriate place type, such as a **city**, rather than against every possible Wikidata entity. Choose **city** from the bulleted list under "Reconcile each cell to an entity of one of these types:"
8. Start reconciliation.
9. In the `place_norm` column, you should now see the ronciled field values. If there are multiple candidates, it will show them. Review each proposed candidate before accepting it. You can hover your mouse over the value and a pop-up should appear with more information. Or you can click on it to open the Wikidata record.

Do not accept a match solely because it has the highest score. Check the entity's name, type, country, and other available contextual information.

If reconciliation is unavailable or produces an error, skip it. The core tutorial does not depend on an external service.

---

# Part 13. Review the transformation history

Open the **Undo/Redo** tab and examine the list of operations.

The history should show operations such as:

- adding normalized columns;
- trimming whitespace;
- editing values through facets;
- clustering names or places;
- deriving date fields;
- transforming subject separators;
- splitting and undoing the split.

Select earlier points in the history and observe how the project preview changes. Then return to the most recent state.

This history is part of the provenance of the cleaned dataset. It records what was done, although it does not by itself explain why each decision was historically justified.

---

# Part 14. Export your work

You will export both the cleaned table and the transformation history.

## Step 1. Clear filters

Before exporting:

1. remove or reset all facet selections;
2. confirm that the grid shows all 100 rows.

If a facet remains active, some export options may export only the matching rows.

## Step 2. Export the cleaned data

1. Select **Export**.
2. Select **Comma-separated value** or **Excel 2007+**.
3. Save the file in your `week-03-openrefine` folder using a name such as:

   ```text
   historical_photographs_cleaned_YOURNAME.csv
   ```

Do not overwrite the original workbook.

## Step 3. Export the operation history

1. Open the **Undo/Redo** tab.
2. Select **Extract...**.
3. Leave all relevant operations selected.
4. Copy the JSON shown in the window.
5. Paste it into a plain-text editor.
6. Save it as:

   ```text
   openrefine_operations_YOURNAME.json
   ```

The JSON file allows the same sequence of operations to be applied to a compatible dataset. It does not replace a written explanation of interpretive choices.

## Step 4. Optional project backup

To preserve the complete interactive project:

1. Select **Export**.
2. Select **OpenRefine project archive to file**

This will save the project as an `.openrefine.tar.gz` file.

---

# Part 15. Challenge 1: RawGraphs (Optional)

Use your knowledge from last week by importing this dataset into RawGraphs. Create and save an interesting chart to share.

---

# Part 16. Challenge 2: Books dataset (Optional)

Create a new OpenRefine project using the `books-sample.csv` dataset from last week's tutorial. Explore the dataset, identify gaps or areas that would need cleaning. Record what steps you would need to take.

---

# Part 17. Written reflection

Make sure you log everything in your journal

## Evidence

1. Identify two forms of inconsistency that were directly visible in facets.
2. Identify one field in which a blank value, `unknown`, and an uncertain statement should not automatically be treated as equivalent.

## Inference

3. Give one example of two values you inferred referred to the same person, place, or category. What evidence supported the merge?
4. Give one example you chose not to merge. What evidence was missing?

## Interpretation

5. Explain one way that normalizing place, creator, date, or subject values could change a later historical analysis.
6. Why might a count of normalized place names differ from a count based on the raw metadata?

## Limits and uncertainty

7. What information in the dataset would be lost if `date_raw` were replaced by a single year?
8. Why are repeated filenames, titles, or composite keys evidence of possible duplication rather than proof?

---

# Troubleshooting

## OpenRefine does not open in the browser

1. Confirm that the OpenRefine application, console, or terminal window is still running.
2. Open a browser and enter `http://127.0.0.1:3333/`.
3. If the page still does not load, close OpenRefine completely and start it again.
4. Consult the [official installation documentation](https://openrefine.org/docs/manual/installing).

## The project does not contain 100 rows

Return to the import preview and check that:

- the first row was treated as column headers;
- no filter is currently active;
- the subject-splitting experiment was undone.

## A GREL expression produces an error

Check:

- spelling and capitalization of column names;
- straight quotation marks rather than typographic quotation marks;
- matching parentheses;
- that the expression was pasted into the correct column operation;
- the preview before selecting **OK**.

If you cannot resolve the problem, take a screenshot of the expression and error message. Do not replace the operation with undocumented manual edits.

## Clustering produces no useful groups

This is not necessarily an error. Try another clustering method, but do not merge values merely to demonstrate that you used the tool. A justified decision that no suggested cluster is reliable is a valid result.

## Reconciliation fails

Reconciliation requires internet access and a functioning external service. Skip the optional section and continue with export. Your local OpenRefine project remains usable.

## Your exported file contains only a few rows

A facet or filter was probably active during export. Return to the project, reset all facets, confirm that all 100 rows are visible, and export again.

---

# Further reference

- [OpenRefine user manual](https://openrefine.org/docs)
- [Installing OpenRefine](https://openrefine.org/docs/manual/installing)
- [Exploring facets](https://openrefine.org/docs/manual/facets)
- [Cell editing and clustering](https://openrefine.org/docs/manual/cellediting)
- [Transforming data](https://openrefine.org/docs/manual/transforming)
- [GREL functions](https://openrefine.org/docs/manual/grelfunctions)
- [Reconciliation](https://openrefine.org/docs/manual/reconciling)

Remember the larger research pipeline:

> historical activity → contemporary recording → preservation → archival selection → digitization → database construction → computational transformation → analysis

OpenRefine operates mainly at the stages of database construction and computational transformation. It can make inconsistencies more visible and transformations more reproducible. It cannot determine whether the surviving records are representative or whether a standardized category adequately captures a historical phenomenon.
