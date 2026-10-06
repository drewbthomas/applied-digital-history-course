# Data Dictionary for `book-sample.csv`

## Dataset Overview

`book-sample.csv` is an instructor-prepared teaching dataset containing 500 bibliographical records for printed-book editions dated between 1501 and 1600. The records are assigned to either the Holy Roman Empire or the Swiss Confederation.

Each row represents one **edition record**. A row does not represent:

- One surviving physical copy.
- One unique text or work.
- One author.
- One print run.
- The number of copies originally produced.
- The number of historical readers.

Different editions of the same text may therefore appear in separate rows. The survival and cataloguing of an edition may also differ from its historical production, circulation, or significance.

The records were selected to provide examples from different languages within these two geographical categories. The sample is designed for learning how to inspect and visualize data; it should not be treated as a proportionally representative account of printing in either territory or of early modern European printing more generally.

## File Format

The dataset is supplied as UTF-8 encoded comma-separated values. The first row contains the column names. Text containing a comma is enclosed in quotation marks.

The CSV can be viewed in a text editor, imported into spreadsheet software, or uploaded to RAWGraphs. When importing it into Excel, use **Data → From Text/CSV**, select UTF-8 encoding, and confirm that the delimiter is a comma.

## Column Definitions

### `edition_id`

**Type:** Identifier  
**Example:** `604419`

Identifier assigned to the bibliographical edition record. Although it consists of digits, it is an identifier rather than a historical quantity and should not be summed or averaged.

The identifier distinguishes records in the source data. It does not imply that editions with similar titles are duplicates.

### `short_title`

**Type:** String  
**Example:** `Newe Zeittung und erschreckliche Propheceyung`

Short title recorded for the edition. Historical spelling, capitalization, punctuation, and transcription practices may vary. The value should not automatically be treated as a complete diplomatic transcription of a title page.

Similar or identical short titles can belong to different editions. Small textual differences can also reflect cataloguing decisions rather than historically meaningful differences.

### `author`

**Type:** String  
**Example:** `Bucer, Martin`

Author associated with the bibliographical record. Names generally appear in surname-first form where an attribution is present.

A blank value means that no author is recorded in this field. It does not by itself establish that the work was historically anonymous. Authorship may be unknown, disputed, omitted from the source, or recorded elsewhere.

### `printer`

**Type:** String  
**Example:** `Samuel Apiarius`

Printer or printing establishment associated with the edition record. Names may contain spelling variants, alternative forms, uncertain attributions, institutional descriptions, or errors.

Do not assume that differently written names necessarily represent different historical people or businesses. Resolving printer identities would require additional research and data cleaning.

### `place`

**Type:** String  
**Examples:** `Basel`, `[Strasbourg]`, `Basel (=La Rochelle)`

Place of printing as recorded or interpreted in the bibliographical data. The field may contain qualifications and should not always be read as a simple, certain modern place name.

Possible conventions include:

- Square brackets for supplied or inferred information.
- An equals sign for a stated place associated with a different interpreted place.
- `s.l.` for *sine loco*, meaning that no place is given or securely identified.

These values would require interpretation and normalization before reliable geocoding.

### `year`

**Type:** String  
**Examples:** `1574`, `1522 (=1523 n.s.)`

Date statement associated with the edition record. It is stored as text because some dates contain qualifications, alternatives, or explanations.

Do not replace this field with a simplified year. It preserves information that may be lost during computational processing.

### `language`

**Type:** String  
**Examples:** `German`, `Latin`, `French`

Language category assigned to the edition record. A blank value means that no language is recorded in this field.

The field may simplify multilingual editions or complicated linguistic features. It should be understood as a database category rather than a complete description of every language appearing in an edition.

### `format`

**Type:** String  
**Examples:** `2o`, `4o`, `8o`, `16o`, `Broadsheet`

Bibliographical format recorded for the edition. Common values include:

- `2o`: folio
- `4o`: quarto
- `8o`: octavo
- `12o`: duodecimo
- `16o`: sextodecimo

Format refers primarily to the arrangement and folding of the printed sheet. It is not identical to an exact physical page size. Blank values indicate that no format is recorded in this field.

### `country`

**Type:** String  
**Examples:** `Holy Roman Empire`, `Swiss Confederation`, `France`, `Spain`

Country or polity category associated with the printing place. The labels should not automatically be interpreted as consistent present-day nation-states or as categories used by historical actors themselves.

The assignment depends on decisions about historical geography, uncertain printing places, political boundaries, and the date of publication. `s.l.` indicates that a place could not be securely assigned. A blank value means that no country category is recorded.

### `year_numeric`

**Type:** Number, with blanks  
**Example:** `1522`

Derived field containing the first four-digit year extracted from `year`. It allows RAWGraphs to order and group dates numerically.

This transformation simplifies the original date. For example, `1522 (=1523 n.s.)` becomes `1522`. Always consult `year` when the precise dating or qualification matters. A blank value means that the preparation script could not extract a four-digit year.

### `decade`

**Type:** String, with blanks  
**Example:** `1520s`

Decade calculated from `year_numeric`. It is provided for convenient grouping and visualization.

A decade value does not add new historical evidence. It is a derived category and inherits any simplification or uncertainty in `year_numeric`.

## Missing Values

Blank cells mean that the corresponding field contains no recorded value in the teaching dataset. A blank should not automatically be interpreted as evidence that the historical attribute did not exist.

For example:

- A blank author does not prove that an edition was historically anonymous.
- A blank language does not mean that an edition had no language.
- A blank format does not mean that the physical format did not exist.
- A blank country may result from missing or uncertain place information.

Missingness is part of the dataset and should be reported rather than silently removed.

## Known Limitations

When interpreting visualizations created from this dataset, remember that:

1. The file is a selected teaching sample rather than a census of early modern printing.
2. The unit of observation is a bibliographical edition record, not a surviving copy or unique text.
3. Bibliographical records depend on historical survival, cataloguing, attribution, and database construction.
4. Author, printer, and place names may contain variants, uncertainties, or errors.
5. The country categories model historical geography and may simplify contested or changing political boundaries.
6. `year_numeric` and `decade` simplify complex date statements.
7. Blank values may reflect missing documentation rather than historical absence.
8. Frequency within the sample should not be equated with readership, influence, production volume, or historical importance.

## Appropriate Claims

A visualization can accurately state:

> Within the teaching sample, the number of recorded German-language editions is higher in these years than in those years.

It cannot establish without additional evidence:

> German-language printing was more important in those years.

The first statement describes the supplied data. The second requires a broader historical argument about coverage, production, survival, readership, and significance.
