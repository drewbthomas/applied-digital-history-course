# Week 2 From Tables to Visual Arguments

## Overview

This week introduces common data-file formats, tabular data, and data visualization with [RAWGraphs](https://app.rawgraphs.io/). You will first inspect a CSV file in Excel in two different ways. You will then examine how a historical dataset has been structured, identify what each row and column represents, and create several visualizations from the same data.

The purpose is not simply to produce an attractive chart. You will investigate how aggregation, categories, visual models, and design choices influence the historical argument a visualization appears to make.

Before beginning the RAWGraphs exercises, read through the [RAWGraphs learning materials](https://www.rawgraphs.io/learning). You do not need to memorize every chart type or option. Use the site as a reference while you work and return to it when you encounter an unfamiliar visual model.

RAWGraphs processes uploaded data in the browser and accepts tabular formats such as CSV, TSV, and JSON. For this exercise, you will work with a supplied CSV file containing 500 records for printed-book editions dated from 1501 to 1600 and assigned to either the Holy Roman Empire or the Swiss Confederation.

The exercise has four stages:

1. Open and inspect the CSV in Excel.
2. Create four guided visualizations from the book dataset.
3. Experiment independently with another chart type or dataset.
4. Optionally complete two advanced visualization challenges.

## Learning Objectives

By the end of this tutorial, you should be able to:

1. Explain the differences between Excel workbooks, CSV, TSV, and JSON files.
2. Explain what a row, column, variable, value, and unit of observation represent in a historical table.
3. Distinguish string, numerical, and temporal data.
4. Identify missing values and potentially inconsistent categories.
5. Open a CSV directly in Excel and import it using Excel's data-import tools.
6. Load a CSV file into RAWGraphs and check how its columns have been interpreted.
7. Map data columns to the visual dimensions of several chart types.
8. Explain why different visual models reveal different aspects of the same dataset.
9. Distinguish between what a visualization displays and the historical interpretation attached to it.

## Materials

You will need:

- `book-sample.csv`, supplied in this tutorial folder.
- `DATA-DICTIONARY.md`, containing the provenance and definitions of the fields.
- A modern web browser.
- Access to [RAWGraphs](https://app.rawgraphs.io/).
- Your personal Applied Digital History portfolio repository.

The teaching dataset is a selected sample of bibliographical records containing a diversity of languages and countries. Each row represents one recorded edition, not one surviving physical copy and not necessarily one unique text.

| Field | Meaning | Expected type |
|---|---|---|
| `edition_id` | Identifier assigned to the bibliographical record | String or identifier |
| `short_title` | Abbreviated title | String |
| `author` | Standardized author name, where known | String |
| `printer` | Standardized printer name | String |
| `place` | Standardized place of printing | String |
| `year` | Original date statement assigned to the edition | String |
| `language` | Principal language category | String |
| `format` | Bibliographical format, where available | String |
| `country` | Historical country or polity category assigned in the source data | String |
| `year_numeric` | First four-digit year extracted from the original `year` field | Number, derived for teaching |
| `decade` | Decade derived from `year_numeric` | String, derived for teaching |

Use the data dictionary rather than assuming that a familiar-looking field has an obvious historical meaning. In particular, a count of rows is a count of **recorded editions in this sample**, not a direct measurement of readership, influence, production volume, or surviving copies.

### Important Limitation

The records were selected to provide varied examples for learning. The sample should not be assumed to represent all early modern printing or the relative historical importance of particular languages, countries, places, authors, or printers. Your visualizations describe the records contained in the teaching dataset.

## Part 1 Common Data-File Formats

Most spreadsheet users are familiar with Microsoft Excel workbooks. Digital research tools, however, frequently exchange data using simpler text-based formats.

### Excel Workbooks

An Excel workbook normally uses the `.xlsx` extension. It can contain:

- Multiple worksheets.
- Formulas.
- Formatting, colors, and fonts.
- Charts and images.
- Filters and frozen rows.
- Comments and other spreadsheet features.

These features are useful for people working in Excel, but many digital tools do not know which worksheet, formatting choice, or formula result should be treated as the data. For transferring a single table between programs, a simpler format is often preferable.

### CSV

CSV means **comma-separated values**. A CSV file is a plain-text representation of one table.

```csv
edition_id,year,place,language
E001,1520,Wittenberg,German
E002,1520,Augsburg,Latin
E003,1521,Wittenberg,German
```

- Each line represents a row.
- Commas separate the columns.
- The first line normally contains the column names.
- Quotation marks can enclose values that themselves contain commas.

A CSV file does **not** preserve Excel formatting, charts, comments, multiple worksheets, or formulas. It contains the values of one table.

CSV files can be opened in Excel. You can often double-click the file, but it is safer to import it deliberately:

1. Open Excel.
2. Select **Data**.
3. Select **From Text/CSV**.
4. Choose the CSV file.
5. Confirm the file encoding, normally UTF-8.
6. Confirm that the delimiter is a comma.
7. Inspect the preview before loading the data.

Importing deliberately matters because Excel may automatically convert identifiers, dates, fractions, or long numbers. For example, it might remove leading zeroes from an identifier or interpret a historical date as a modern calendar value.

If you edit a CSV file in Excel, use **Save As** and check the selected format. Saving as `.xlsx` creates an Excel workbook. Saving as CSV retains a plain-text table, but only the active worksheet is saved and formulas are replaced by their displayed values.

### TSV

TSV means **tab-separated values**. It follows the same basic model as CSV, except that a tab character separates the columns.

```text
edition_id    year    place        language
E001          1520    Wittenberg   German
E002          1520    Augsburg     Latin
```

TSV can be useful when textual values contain many commas. TSV files can also be opened or imported in Excel. During import, select **Tab** as the delimiter.

The apparent spaces in a TSV file are meaningful tab characters. Do not replace them casually with ordinary spaces.

### JSON

JSON means **JavaScript Object Notation**. It stores information as named fields and values and can represent nested structures that do not fit neatly into one table.

```json
[
  {
    "edition_id": "E001",
    "year": 1520,
    "place": {
      "name": "Wittenberg",
      "region": "Saxony"
    },
    "languages": ["German"]
  }
]
```

JSON is common in APIs and web applications. Unlike CSV, a field can contain another object or a list of several values. Before JSON can be analyzed as a table, someone must decide how its nested structure should be flattened into rows and columns.

Excel can import some JSON files through its data-import tools, but JSON should not be thought of simply as another kind of spreadsheet. It is a more flexible way of structuring data.

### Summary

| Format | Structure | Preserves formatting and formulas? | Common use |
|---|---|---:|---|
| `.xlsx` | Workbook with one or more worksheets | Yes | Working interactively in Excel |
| `.csv` | One plain-text table separated by commas | No | Exchanging tabular data between tools |
| `.tsv` | One plain-text table separated by tabs | No | Exchanging tables containing many commas |
| `.json` | Fields, objects, and lists; may be nested | No | APIs, web data, and structured data exchange |

For this tutorial, CSV provides the simplest bridge between a familiar Excel-style table and RAWGraphs.

## Part 2 Open the CSV Directly in Excel

Download `book-sample.csv` from the course repository. Keep the original file unchanged so that you can return to it if something goes wrong.

First, open the file directly:

1. Locate `book-sample.csv` in your Downloads folder.
2. Double-click it. If it does not open in Excel, right-click it and choose **Open with → Excel**.
3. Check whether the data appears in separate columns.
4. Look at names and places containing accented characters, such as `Köln`. Are they displayed correctly?
5. Scroll across the columns and down through the rows.
6. Find a blank cell. Does it mean zero, unknown, not applicable, or simply missing?
7. Find one value in `year` that is more complicated than a four-digit number, if one is present. Compare it with `year_numeric`.

Excel often recognizes a comma-separated file automatically. That is convenient, but it also means Excel makes decisions about delimiters and data types for you. It may convert dates, identifiers, or long numbers into forms you did not intend.

Do not edit or save the file yet. Close it when you have finished inspecting it. If Excel asks whether you want to save changes, choose **Don't Save**.

## Part 3 Import the CSV into Excel

Now import the same file deliberately:

1. Open a new blank Excel workbook.
2. Open the **Data** tab.
3. Choose **From Text/CSV**. The exact wording may vary slightly between versions of Excel.
4. Select `book-sample.csv`.
5. In the preview, choose **UTF-8** as the file origin or encoding if Excel has not detected it correctly.
6. Confirm that the delimiter is a **comma**.
7. Confirm that the preview separates the data into eleven columns.
8. Choose **Load**. If your version offers **Transform Data**, you may open it to inspect the detected data types before loading.

Compare this result with opening the CSV directly. Importing gives you more control over how Excel interprets the file. This becomes especially important when a dataset uses another delimiter, contains non-English characters, or has identifiers that Excel might mistake for numbers or dates.

If you want to keep notes, filters, formatting, or additional worksheets, save the workbook as `book-sample-inspection.xlsx`. An `.xlsx` workbook can store those Excel features; a CSV cannot. Do not overwrite `book-sample.csv`.

Be aware that saving an Excel workbook as CSV normally saves only the active worksheet and discards formatting, formulas, charts, and additional worksheets. CSV is a plain-text data-exchange format, not an Excel workbook format.

## Part 4 Understand the Dataset

Read `DATA-DICTIONARY.md`, then examine the imported table. Record brief answers to these questions in your `visualization-notes.md` file:

1. What does one row represent?
2. Does a row describe a physical copy, an edition, or an aggregate?
3. Which columns contain categories?
4. Which columns contain quantities?
5. Which columns are identifiers rather than quantities?
6. How are dates represented in `year`, `year_numeric`, and `decade`?
7. How are missing values represented?
8. Are any categories uncertain, inconsistent, or historically constructed?

### Unit of Observation

The **unit of observation** is the entity represented by one row. It might be a book, person, event, place, letter, transaction, or an aggregated count.

This distinction matters. Here, one row represents one recorded edition. Counting `edition_id` therefore counts sampled bibliographical records. It does not provide the number of copies printed or surviving.

## Part 5 Check the Table Structure

A table suitable for RAWGraphs should normally have:

- One header row.
- A unique name for every column.
- One consistent type of information in each column.
- One consistent unit of observation across all rows.
- Numerical values stored as numbers rather than mixed with words or symbols.
- Categories written consistently.

Inspect the supplied file for at least one example of each of the following:

- A string value.
- A numerical value.
- A temporal value.
- A missing or uncertain value, if present.

Do not correct the file for this exercise unless the instructions accompanying the dataset explicitly ask you to do so. Record problems that will require cleaning instead. Data cleaning will be the focus of Week 3.

## Part 6 Load the Data into RAWGraphs

1. Open [RAWGraphs](https://app.rawgraphs.io/).
2. Find the option to load or upload data.
3. Upload `book-sample.csv` or copy and paste the data.
4. Confirm that RAWGraphs has recognized the header row.
5. Inspect the detected type of each column.
6. Confirm that `year_numeric` and `edition_id` have been recognized as numerical.
7. Confirm that `place`, `language`, `format`, and `country` have not been interpreted as numbers or dates.
8. Compare the original `year` field with `year_numeric`. Find at least one record for which they differ in form.

If a column has been interpreted incorrectly, do not proceed immediately. Consider why this happened. Possible causes include:

- Mixed text and numbers in the same column.
- Non-standard date formats.
- Thousands separators or units included in numerical cells.
- Blank cells.
- Inconsistent delimiters.

Record any problem you encounter and how you resolved or worked around it.

## Part 7 Create Four Visualizations

You will create four different visualizations from the same records. A chart type is not a decorative wrapper: each visual model requires particular kinds of data, aggregates records in a particular way, and encourages different questions.

When counting rows, use `edition_id` as the measure and change its aggregation from **Sum** to **Count**. The identifier's numerical value has no historical meaning; it is being used only to count rows.

For every chart, add one variable at a time and look at the visualization after each step. Do not drag all the variables into place at once. Notice what changes, what remains the same, and what new claim the chart appears to make. This is one of the best ways to understand what each chart variable controls.

### Visualization 1 Bar Chart: Editions by Decade

Create a **Bar Chart** that asks:

> How are the sampled edition records distributed across decades?

Map the columns as follows:

```text
Bars:    decade
Size:    edition_id (Count)
Color:   leave empty
Series:  leave empty
```

First add `decade` to **Bars** and inspect the result. Then add `edition_id` to **Size**, open its aggregation setting, and select **Count**. Inspect the chart again. It should now size the bars according to the number of sampled edition records rather than the numerical values of their identifiers.

- **Bars** defines the categories represented by separate bars.
- **Size** determines each bar's length or height. Setting `edition_id` to **Count** counts the records in each decade.
- **Color** can encode an additional field, but is unnecessary here.
- **Series** creates separate versions of the chart for different values and should remain empty.

Make sure the decades appear in chronological order. Export the chart as:

```text
01-bar-editions-by-decade.svg
```

### Visualization 2 Line Chart: Country Records over Time

Create a **Line Chart** that asks:

> How do the annual numbers of sampled edition records assigned to the two countries change over time?

Map the columns as follows:

```text
X Axis:  year_numeric
Y Axis:  edition_id (Count)
Lines:   country
Color:   country
Series:  leave empty
```

Build the chart in stages:

1. Add `year_numeric` to **X Axis** and `edition_id` to **Y Axis**. Change the aggregation for `edition_id` to **Count**. View the chart before adding anything else. It shows the annual number of records in the sample.
2. Add `country` to **Lines**. View the chart again. The single total line is divided into one line for the Holy Roman Empire and one for the Swiss Confederation. At this stage, both lines have the same color.
3. Add `country` to **Color**. View the chart again. The two lines are now assigned different colors and are easier to distinguish.

- **X Axis** requires a numerical or date field and positions observations horizontally. Use `year_numeric`, not the textual `year` field.
- **Y Axis** requires a numerical or date field. Setting `edition_id` to **Count** counts records sharing the same year and line.
- **Lines** divides the values into separate lines. Using `country` produces one line for each country.
- **Color** controls the lines' colors. Mapping `country` here gives the two country lines different colors.
- **Series** creates separate line charts rather than additional lines, so leave it empty.

A line implies continuity between the plotted years. It does not prove that historical change was smooth, and fluctuations in this teaching sample should not be treated as complete production totals.

Export the chart as:

```text
02-line-editions-by-year-and-country.svg
```

### Visualization 3 Beeswarm Plot: Individual Records over Time

Create a beeswarm plot in which every point represents an edition record. This chart asks:

> Where do the individual records in the sample fall across the sixteenth century?

Map the columns as follows:

```text
X Axis:  year_numeric
Size:    leave empty
Color:   language
Label:   leave empty
Groups:  country
```

Add the variables one at a time and inspect the plot after every step. In particular, compare the single distribution produced by **X Axis** with the separated distributions produced after adding **Groups**.

- **X Axis** requires a number or date and positions each point horizontally.
- **Size** controls point size. Leave it empty so that every edition record is displayed with the same point size.
- **Color** accepts numerical, textual, or date fields. Here it distinguishes languages.
- **Label** displays text on the plot. Leave it empty because hundreds of edition labels would overlap and obscure the visualization.
- **Groups** separates points into distinct horizontal groups. Here it creates one group for each country.

Observe the difference between displaying individual records and aggregating them into totals. Consider whether numerous language colors clarify the result or make it harder to read.

Export the chart as:

```text
03-beeswarm-editions-over-time.svg
```

### Visualization 4 Treemap: Country, Language, and Format

Create a treemap that asks:

> How are the sampled records divided among countries, languages, and bibliographical formats?

Map the columns as follows. Add the three hierarchy fields in this order:

```text
Hierarchy:  country
Hierarchy:  language
Hierarchy:  format
Size:       edition_id (Count)
Color:      language
Label:      format
```

Add the hierarchy fields one at a time. View the treemap after adding `country`, again after adding `language`, and again after adding `format`. Watch how each additional field subdivides the existing rectangles.

- **Hierarchy** accepts multiple fields. Their order determines the nesting: country contains language, and language contains format.
- **Size** requires a numerical field and determines rectangle area. Set `edition_id` to **Count** so that area represents the number of records in each country-language-format combination.
- **Color** can encode a numerical, textual, or date field. Here it reinforces the language categories.
- **Label** controls the labels on the smallest rectangles. Use `format` so that the leaf rectangles display their bibliographical-format category.

In the customization panel, expand the **Labels** options and turn on **Show hierarchy labels**. This displays the country and language labels belonging to the higher levels of the hierarchy.

Do not try to encode every available field. A treemap is useful for part-to-whole relationships, but areas and small rectangles are harder to compare precisely than aligned bars.

Export the chart as:

```text
04-treemap-country-language-format.svg
```

### Check Every Visualization

Before exporting, ask:

1. Is `edition_id` set to **Count** wherever the chart should count records?
2. Are years or decades ordered chronologically?
3. Are labels and legends readable?
4. Does color communicate a meaningful category?
5. Are small categories hidden or difficult to compare?
6. What is aggregated, and what remains visible as an individual record?
7. Could a viewer mistake the teaching sample for a complete account of historical print production?

Export SVG files rather than relying on screenshots. You may also export PNG copies for convenient previewing.

Explore the customization options for every visualization. Try different dimensions, orientations, sorting options, color schemes, label settings, margins, and canvas sizes. Observe the chart after each change and retain only choices that make the evidence easier to understand.

## Part 8 Optional Advanced Challenges

These challenges are intended for students who finish early or want to explore RAWGraphs more deeply.

### Challenge 1 Horizontal Bar Chart by Place

Create a horizontal bar chart showing the number of sampled edition records associated with each place of publication.

Export the chart as:

```text
challenge-horizontal-bar-editions-by-place.svg
```

### Challenge 2 Pie Chart by Format

Create a pie chart showing the proportion of sampled edition records belonging to each bibliographical format.

**Hint:** RAWGraphs displays the data you provide; it does not perform the data analysis or restructuring needed to turn the values in the `format` column into separate numerical **Arcs**. Examine what the Pie Chart accepts, then decide how the original data must be counted and reorganized before it can produce the chart.

Do not overwrite `book-sample.csv`. Save any transformed or summarized data as a new CSV and preserve missing-format records as an explicit category rather than silently removing them.

Submit the transformed CSV, the pie chart, and a short note explaining:

- How you counted the format categories.
- How you checked that the totals match the original 500 records.
- How you reorganized the data for RAWGraphs.
- What the pie chart reveals and what it makes difficult to compare.

```text
challenge-formats-for-pie.csv
challenge-pie-formats.svg
challenge-pie-notes.md
```

## Part 9 Explore another dataset

Choose another chart type and create a visualization you can present in class. You may use a dataset supplied by RAWGraphs or a dataset of your own.

1. Return to Step 1, **Load your data**, in RAWGraphs.
2. To use a supplied dataset, select **Try our data samples** and choose one that interests you. Alternatively, load a dataset of your own.
3. Inspect its columns before choosing a chart.
4. Identify its unit of observation.
5. Choose a chart type that you have not already used in the guided exercises.
6. Add the chart variables one at a time, observing how the visualization changes.
7. Try the available customization options for style and display. Experiment with color, sorting, labels, dimensions, spacing, and other relevant settings.
8. Export the visualization as SVG.
9. Prepare a two- to three-minute explanation for class.

Your explanation should cover:

- What the dataset records and what one row represents.
- Why you selected this chart type.
- Which columns you mapped to which visual dimensions.
- One pattern that becomes visible.
- One limitation, ambiguity, or possible source of misinterpretation.

Name the file:

```text
05-independent-[short-description].svg
```
## Part 10 Organize Your Portfolio

If `weekly-work/week-02/` does not yet exist, create it through GitHub:

1. Open `weekly-work/` in your personal repository.
2. Select **Add file** and then **Create new file**.
3. In the filename field, enter `week-02/README.md`.
4. Add the heading `# Week 2 RAWGraphs`.
5. Save the file with the description `Create Week 2 work folder`.

Upload the visualizations to this folder.

Create `journal/week-02.md` by following the instructions in `journal/README.md` in your portfolio repository.

## Files to Submit

Your personal portfolio should contain:

```text
weekly-work/week-02/
├── README.md
├── 01-bar-editions-by-decade.svg
├── 02-line-editions-by-year-and-country.svg
├── 03-beeswarm-editions-over-time.svg
├── 04-treemap-country-language-format.svg
└── 05-independent-[short-description].svg

journal/
└── week-02.md
```

You may also submit PNG versions to make the visualizations easier to preview. Place files from either optional challenge in the same folder.

There is no need to upload a duplicate of the supplied dataset. The central course repository preserves the original teaching file.

## Week 2 Journal

Use the standard weekly journal questions. Pay particular attention to:

- The relationship between the unit of observation and the visual result.
- A choice you made when importing the CSV or mapping data columns to visual properties.
- Something that the visualization appears to show but cannot establish historically.
- Whether visualization might be useful for your final project.

## Further Guidance

RAWGraphs describes its general workflow as loading data, selecting a visual model, mapping data columns to visual dimensions, customizing the visualization, and exporting the result. Its documentation also explains how to format and load tabular data:

- [RAWGraphs learning materials](https://www.rawgraphs.io/learning)
- [How to load and format data for RAWGraphs](https://www.rawgraphs.io/learning/how-to-load-and-format-your-data-for-rawgraphs)
