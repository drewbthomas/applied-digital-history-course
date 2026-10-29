# Week 4 Using Large Language Models for Python Tasks

## Overview

This tutorial introduces Python as a tool for working with historical data and large language models (LLMs) as assistants for writing, explaining, and debugging code. No previous programming experience is required.

You will not attempt to learn Python syntax systematically. Instead, you will learn enough to supervise a small computational task: define the intended result, describe the data accurately, ask an LLM for code, run the code in a notebook, inspect the output, and revise either the prompt or the code when the result is wrong.

The tutorial uses the two datasets from earlier weeks:

- `book-sample.csv`: 500 bibliographical records for early modern printed editions;
- `historic_photographs_sample_dataset.csv`: 100 synthetic photograph metadata records containing deliberate inconsistencies and missing values.

You will use Python to:

1. inspect a CSV and calculate descriptive statistics;
2. transform a CSV into a new table for RAWGraphs;
3. create a visualization from a CSV;
4. evaluate what the resulting numbers and images do—and do not—demonstrate historically.

Optional extensions use an LLM to generate HTML and JavaScript rather than Python. They show that an LLM can also help make a small interactive visualization or a searchable interface for a CSV.

## Learning Objectives

By the end of this tutorial, you should be able to:

1. Explain what Python is and identify several uses for it in Digital History.
2. Explain the difference between Python, a Jupyter notebook, Google Colab, and an LLM.
3. Add, run, edit, and rerun code cells in a notebook.
4. Upload CSV files to a temporary notebook session and download generated files.
5. Write a prompt that specifies a task, input data, constraints, and expected output.
6. Use an LLM to generate and explain a short Python script.
7. diagnose a simple error by giving the LLM the code and complete error message.
8. verify generated code by checking row counts, column names, missing values, and selected records.
9. Distinguish a technically correct computation from a justified historical inference.
10. Preserve original data and make computational transformations inspectable.

## Materials

You will need:

- `book-sample.csv` from this tutorial folder;
- `historic_photographs_sample_dataset.csv` from this tutorial folder;
- a modern web browser;
- access to [ChatGPT](https://chatgpt.com/) or another LLM that can generate code;
- preferably a Google account for [Google Colab](https://colab.research.google.com/).

Do not upload confidential, copyrighted, personally identifying, or otherwise restricted research data to a public AI service or cloud notebook. The two supplied teaching datasets are appropriate for this exercise.

---

# Part 1. What Python is—and what it is not

Python is a general-purpose programming language. A Python **script** is a sequence of instructions written in that language. The Python **interpreter** executes those instructions.

Python is useful in Digital History because it can perform the same explicit operation across many records. Historians use it to:

- inspect, clean, reshape, and combine tables;
- query APIs and download openly available metadata;
- search or measure large collections of text;
- match names and places across datasets;
- work with dates and geographic coordinates;
- analyze networks;
- compare or classify images;
- create static and interactive visualizations;
- document a reproducible sequence of transformations.

Python does not decide which sources are representative, what a category means historically, whether two people are really the same person, or whether a pattern explains historical change. Those remain historical and methodological questions.

## Four things that are easy to confuse

| Term | What it is | Role in this tutorial |
|---|---|---|
| Python | A programming language | Describes the operations performed on the CSV files |
| Jupyter notebook | A document containing executable code cells, text cells, and output | Organizes the work into small, inspectable steps |
| Google Colab | Google's hosted Jupyter notebook service | Runs the notebook in a browser without a local Python installation |
| LLM | A model that generates text, including plausible code | Helps draft, explain, and revise code; it does not verify the result |

An LLM does not execute Python merely by displaying a code block. The code must be run in a Python environment such as Colab, Jupyter, or JupyterLite.

## A very small amount of Python

You do not need to memorize this syntax, but you should be able to recognize the purpose of each line:

```python
import pandas as pd

books = pd.read_csv("book-sample.csv")
print(books.shape)
display(books.head())

# This is a comment
```

- `import pandas as pd` loads the `pandas` library, a common tool for tables.
- `books = ...` assigns a table to the name `books`.
- `pd.read_csv(...)` reads a CSV file.
- `print(books.shape)` reports the number of rows and columns.
- `display(books.head())` displays the first five rows.
- Text following `#` is a comment for a human reader. That line will not be executed by the interpreter. Comments are useful to write notes in the code about what each block of script does.

The exact spelling, quotation marks, parentheses, and indentation can matter. A computer follows the code that exists, not the intention behind it.

> **Checkpoint 1:** In your own words, explain the difference between Python, a notebook, Colab, and an LLM.

---

# Part 2. Choose a notebook environment

## Recommended: Google Colab

[Google Colab](https://colab.research.google.com/) is a hosted Jupyter notebook service. It requires no local Python setup and stores notebooks in Google Drive. A free account is sufficient for this tutorial. Google's [Welcome to Colab notebook](https://colab.research.google.com/notebooks/welcome.ipynb) demonstrates code and text cells, while the [official Colab FAQ](https://research.google.com/colaboratory/faq.html) explains storage, sharing, runtimes, and limitations. You can also watch Google's short [Introduction to Google Colab video](https://www.youtube.com/watch?v=inN8seMm7UI).

One advantage of Colab is that the Python code runs on a temporary Google virtual machine rather than directly on your laptop. An ordinary mistake in the code is therefore unlikely to damage your computer or its files. You should still read generated code before running it and be cautious if it asks to mount Google Drive, enter credentials, download unfamiliar files, or connect to another service.

Colab code runs in a temporary virtual machine. Files uploaded only to the current runtime can disappear when the runtime ends. Download your outputs and notebook before finishing.

## No Google account: JupyterLite

Use the official [Try Jupyter with JupyterLite](https://jupyter.org/try-jupyter/lab/) page if you cannot or do not want to use a Google account. Choose a Python notebook and upload the CSV files through its file browser.

JupyterLite runs Python inside the browser without an account or installation. It supports the libraries needed for most of this tutorial, but it is not identical to ordinary Jupyter or Colab: some Python packages and system features will not work, and work stored only in the browser is easier to lose. Download the notebook and every output file you want to keep.

## Local option: Jupyter

If Python and Jupyter are already installed on your computer, you may use a local notebook. Keep the notebook and CSV files in the same folder. Local installation is not required for this tutorial.

---

# Part 3. Create and operate a Colab notebook

The instructions below use Colab. The same concepts apply in JupyterLite or local Jupyter, although buttons may be in different places.

## Step 1. Create and name the notebook

1. Open [Google Colab](https://colab.research.google.com/).
2. Sign in if requested.
3. Select **New notebook**.
4. Select the title at the top and rename it:

   ```text
   week-04-llm-python-YOURNAME.ipynb
   ```

The `.ipynb` extension identifies an open Jupyter notebook file.

## Step 2. Recognize the two main cell types

- A **text cell** contains headings, instructions, links, and interpretation written in Markdown.
- A **code cell** contains executable Python.

Use **+ Text** to add a text cell and **+ Code** to add a code cell.

Add a text cell containing:

```markdown
## My first Python notebook

This notebook records the prompts, code, checks, and interpretations from Week 4.
```

The text initially appears in editing mode, where you can see the Markdown symbols. Press `Shift+Enter` to run the text cell. Colab will process the Markdown and display it as a formatted heading and paragraph. To edit the text again, double-click the rendered cell, make the change, and press `Shift+Enter` again. Text cells do not execute Python, but they still need to be run in order to render their Markdown formatting.

## Step 3. Run a code cell

In a code cell, enter:

```python
print("Hello, digital history!")
```

Run it with the play button or `Shift+Enter`. The output should appear below the cell.

The first code cell may take several seconds—and occasionally a minute—to run. Colab must first connect the notebook in your browser to a temporary virtual machine on Google's servers. While this happens, you may see **Connecting**, **Initializing**, or a moving progress indicator near the top right of the notebook. Wait for the connection and the cell to finish rather than repeatedly selecting the play button.

After Colab connects, the top-right resource display may show **RAM** and **Disk**:

- **RAM** is the temporary working memory available to the running Python session. Variables and tables currently being processed use this memory.
- **Disk** is temporary storage attached to the Google virtual machine. CSV files uploaded to the session and files created by the code use this space.

These meters describe Google's temporary runtime, not the RAM or disk inside your laptop. The teaching datasets use only a tiny fraction of the available resources. Clicking the display opens more detailed runtime information. Files held only in this temporary runtime can disappear when it disconnects, so you must download outputs that you want to keep.

Change the message, then run the cell again. Editing a cell does not update its output until the cell is rerun.

## Step 4. Understand state and order

Copy the following into separate code cells. Run these cells in order:

```python
number_of_books = 500
```

```python
print(number_of_books)
```

The second cell depends on the first. If the runtime restarts, stored values and uploaded files disappear even though the visible notebook cells remain. Run the cells again from the beginning using **Runtime → Run all**.

## Step 5. Upload the data

Download both CSV files from the course repository to your computer. In Colab:

1. Select the **Files** icon in the left sidebar.
2. Select **Upload to session storage**.
3. Upload both CSV files.
4. Confirm that these names appear exactly:

   ```text
   book-sample.csv
   historic_photographs_sample_dataset.csv
   ```

Alternatively, run this cell and select both files:

```python
from google.colab import files
uploaded = files.upload()
```

The `google.colab` upload cell is specific to Colab. Do not ask an LLM to use it if you are working in JupyterLite or local Jupyter.

> **Checkpoint 2:** Your notebook contains at least one text cell and two executed code cells, and both CSV files are visible in the notebook environment.

---

# Part 4. Use an LLM as a coding assistant

For this tutorial, use [ChatGPT](https://chatgpt.com/) as the coding assistant. You may use another LLM if you prefer, but menu labels, formatting, and the quality of the generated code may differ. Keep ChatGPT open in one browser tab and your Colab notebook open in another.

ChatGPT writes the proposed Python code based on your prompt, but Colab is where you execute it. The productive workflow is iterative:

```text
historical question
        ↓
describe the data and desired output
        ↓
ask the LLM for a small, inspectable step
        ↓
read the explanation and code
        ↓
run it in the notebook
        ↓
inspect errors and validate results
        ↓
revise the prompt or code
        ↓
interpret the verified output historically
```

## What a useful coding prompt contains

A strong prompt normally specifies:

1. **Context:** your experience level and working environment.
2. **Input:** filename, row meaning, and exact column names.
3. **Task:** the operation to perform.
4. **Constraints:** preserve original data, use particular libraries, avoid unsupported features.
5. **Output:** what should be displayed or saved and its filename.
6. **Validation:** checks that should be printed.
7. **Explanation:** request a plain-language account of the code and assumptions.

Use this reusable template (don't actually use it right now; this is just a demonstration. You will actually use it in later steps):

```text
I am a digital humanities studient with no previous Python experience, working in [Google Colab / JupyterLite].

Input: [filename]. Each row represents [unit of observation]. The relevant columns are [exact column names].

Here is an excerpt from the beginning of the file:
[excerpt]

Task: [specific operation].

Requirements:
- Use Python with pandas [and matplotlib if needed].
- Do not overwrite the input file or silently delete rows.
- Preserve original values and put derived values in new columns.
- Make missing values visible.
- Print validation checks, including row counts and relevant missing-value counts.
- Save the result as [output filename], if applicable.
- Give me one self-contained code cell, followed by a plain-language explanation of every major step and any assumptions.
```

## Ask ChatGPT for an explanation before execution

After ChatGPT has generated the code, remain in the same ChatGPT conversation and send this follow-up prompt:

```text
Explain this code line by line for a person who has never used Python. Identify every place where it changes, excludes, groups, or summarizes data. Also identify any assumptions it makes about the CSV.
```

ChatGPT will use the code already present in the conversation as context. Read its explanation before moving the code to Colab. If it refers to a transformation that you did not request or cannot justify, ask ChatGPT to revise the code before running it.

When you are ready to test the code:

1. Find the Python code block in ChatGPT's response.
2. Use the code block's **Copy code** button, or select and copy only the code. Do not copy the surrounding explanation into a code cell.
3. Return to the Colab browser tab.
4. Select **+ Code** to create a new code cell.
5. Paste the copied Python code into the code cell.
6. Run it with the play button or `Shift+Enter`.
7. Read both the output and any error message before continuing.

You can also add your own Text blocks with headings or instructions documenting what each step you're doing is.

The ChatGPT conversation records the request and proposed solution; the Colab notebook records the code you actually ran and the output it produced. Preserve both parts of the reasoning process by copying important prompts or explanations into text cells in the notebook.

## Protect yourself and your data

Do not run code you cannot broadly explain. It's not particularly applicable to this tutorial, but in general, pay particular attention to code that:

- deletes or overwrites files;
- installs unfamiliar packages;
- includes passwords, API keys, or account credentials;
- uploads data to another service;
- uses network requests;
- hides errors or silently discards records.

For this tutorial, the code should read the supplied CSV files, calculate or transform values in memory, display results, and write clearly named new output files.

---

# Part 5. Exercise 1: Calculate statistics for the book data

## Historical and computational question

Ask:

> What does this sample contain, how complete are its columns, and how are its recorded editions distributed across time and selected categories?

Remember that each row is one **recorded edition in the teaching sample**. A count of rows is not a count of printed copies, readers, texts, or all editions that historically existed.

## Step 1. Give the LLM the schema, not the whole dataset

Use this prompt. Replace the bracketed environment if necessary. Copyt it to ChatGPT and submit the prompt.

```text
I am a digital humanities student with no previous Python experience working in [Google Colab]. Write one self-contained Python code cell using pandas to inspect a CSV named "book-sample.csv".

Each row represents one recorded edition in a teaching sample. The columns are: edition_id, short_title, author, printer, place, year, language, format, country, year_numeric, decade.

Here is a short excerpt from the beginning of the file:
[paste the first 5 rows here]

The code must:
1. load the CSV without changing it;
2. print the number of rows and columns;
3. display the first five rows;
4. print every column name and data type;
5. print the number and percentage of missing values in every column;
6. calculate minimum, median, mean, and maximum of year_numeric;
7. count records by decade, country, language, and format, including missing categories;
8. check whether edition_id is unique;
9. use clear labels so a beginner can interpret each output.

Do not drop, fill, or overwrite any values. After the code, explain it in plain language and identify what these statistics cannot establish historically.
```

## Step 2. Read, paste, and run

1. Read the explanation.
2. Confirm that the filename and column names match the supplied file.
3. Use **Copy code** in ChatGPT, return to Colab, and select **+ Code**.
4. Paste only the Python code into the new Colab code cell.
5. Run the cell with the play button or `Shift+Enter`. (It may take a few seconds to run. The play button has a spinning border around it while its running the code. Just wait. You'll know when it is done, as the output will appear).
6. If the LLM has divided the code into several cells, you may paste and run them in order instead.

## Step 3. Validate the result

At minimum, confirm:

- the table has **500 rows and 11 columns**;
- `edition_id` is unique;
- `year_numeric` runs from **1501 to 1600**;
- missing-value counts include **100** for `author`, **14** for `language`, and **19** for `format`;
- counts across the displayed decades total 500.

If your output differs, do not adjust it merely to match these values. Check the filename, column names, parsing, and code first.

> **Checkpoint 3:** Your notebook displays the expected dimensions and missing-value counts, and your text cell distinguishes file-level evidence from historical inference.

---

# Part 6. When generated code fails

Errors are normal. They often provide more useful information than the original prompt.

## A debugging protocol

1. Read the final line of the error message.
2. Check the filename and capitalization.
3. Check that the CSV is still present in the current runtime.
4. Check the exact column names by running:

   ```python
   print(books.columns.tolist())
   ```

5. Give the LLM:
   - the original prompt;
   - the exact code you ran;
   - the complete error message;
   - the output of `print(books.columns.tolist())` if relevant.
6. Ask for the smallest necessary correction and an explanation of the cause.
7. Run the corrected code from the beginning.

Use this debugging prompt:

```text
The following Python code produced an error in Google Colab. Explain the cause in plain language and provide the smallest correction. Do not redesign the entire analysis or suppress the error.

CODE:
[paste the complete code]

ERROR:
[paste the complete error message]

COLUMN NAMES, if relevant:
[paste the list]
```

Common errors include:

| Error | Likely cause |
|---|---|
| `FileNotFoundError` | The file was not uploaded, the runtime restarted, or the filename differs |
| `KeyError` | A column name is misspelled or does not exist |
| `NameError` | A required earlier cell was not run, or a variable name differs |
| `ModuleNotFoundError` | The requested library is unavailable in that environment |
| `SyntaxError` | Python cannot parse the code, often because punctuation or indentation changed |

Do not ask only, “Why doesn't this work?” The error, code, schema, and environment are evidence needed for diagnosis.

---

# Part 7. Exercise 2: Transform photograph metadata for RAWGraphs

This exercise creates two new tables from the synthetic photograph metadata. The first remains a record-level table with one row per photograph. The second is an aggregated table containing the exact number of records in each `date_certainty` category, ready to visualize in RAWGraphs. The aim is not to let an LLM “clean” or summarize the data autonomously. You will specify limited transformations, preserve the raw fields, inspect the results, and verify that the aggregate counts account for every input record.

## Historical and computational question

Ask:

> How can uncertain and ranged date statements be represented in separate, machine-readable fields without replacing the original evidence?

The raw `date_raw` field contains exact years, ranges, uncertain dates, supplied dates in square brackets, and approximate dates beginning with `c.`. A single numerical year would hide these distinctions.

## Step 1. Prompt the LLM

```text
I am a digital humanities student with no previous Python experience working in [Google Colab]. Write one self-contained Python code cell using pandas and Python's standard re module to transform "historic_photographs_sample_dataset.csv" for later use in RAWGraphs.

Each input row represents one photograph metadata record. The input has 100 rows and these columns:
record_id, title_raw, creator_raw, date_raw, place_raw, country_raw, subject_raw, object_type_raw, caption_language_raw, holding_institution_raw, rights_raw, box_folder_raw, filename_raw, notes_raw, creator_authority_uri, place_wikidata_qid, lcsh_subjects.

Requirements:
- Preserve all original columns and all 100 rows.
- Do not overwrite, fill, or delete raw values.
- Add year_start: the first four-digit year in date_raw, stored as a nullable integer.
- Add year_end: the second four-digit year when present; otherwise use year_start, stored as a nullable integer.
- Add date_certainty using this explicit precedence: "uncertain" if date_raw contains ?, otherwise "approximate" if it begins with c. after trimming and lowercasing, otherwise "supplied/inferred" if it begins with [, otherwise "range" if it contains two four-digit years, otherwise "exact" if it contains one four-digit year, otherwise "unparsed".
- Add place_trimmed by removing leading and trailing whitespace from place_raw. Do not translate or otherwise standardize place names.
- Print input and output row counts, the number of unique record_id values, missing counts for every newly created column, and a table containing date_raw plus the three derived date columns for at least ten representative rows.
- Assert that the output has 100 rows, that record_id remains unique, and that no original column has been removed.
- Save the record-level transformed table as "photographs_for_rawgraphs.csv" with UTF-8 encoding and without a pandas index.
- Create a second DataFrame that groups the transformed records by date_certainty and counts the number of records in each category. It must have exactly two columns named date_certainty and record_count, with one row per date_certainty category. Include every category, including "unparsed".
- Print this aggregated table and assert that record_count sums to 100.
- Save the aggregated table as "date_certainty_counts_for_rawgraphs.csv" with UTF-8 encoding and without a pandas index.
- Do not use Google Drive mounting or any library other than pandas and Python's standard library.

After the code, explain every transformation, explain how aggregation changes the unit of observation, and identify cases that the rules might handle incorrectly.
```

## Step 2. Inspect before downloading

Run the generated code. Check that:

- the input and output both contain 100 rows;
- all `record_id` values remain unique;
- every original column remains present;
- `date_raw` remains unchanged;
- `1891` becomes start/end `1891` and certainty `exact`;
- `1930-1931` becomes start `1930`, end `1931`, and certainty `range`;
- `1896?` becomes start/end `1896` and certainty `uncertain`;
- `[1924]` becomes start/end `1924` and certainty `supplied/inferred`;
- `c. 1935` becomes start/end `1935` and certainty `approximate`;
- `n.d.` has missing start/end years and certainty `unparsed`.

Then inspect the aggregated `date_certainty` table. It should contain these exact counts:

| `date_certainty` | `record_count` |
|---|---:|
| `exact` | 61 |
| `uncertain` | 12 |
| `range` | 11 |
| `supplied/inferred` | 8 |
| `approximate` | 7 |
| `unparsed` | 1 |

The order of the rows may differ, but the six counts must sum to **100**. If they do not, inspect the classification rules and aggregation code rather than manually changing the numbers.

If the generated code does not display all six forms, ask the LLM for a validation cell that selects one example of each certainty category. Do not ask it to rewrite the entire transformation.

## Step 3. Download both outputs

In Colab, both generated files now appear in the Files sidebar where you earlier uploaded the original CSV files. Download both `photographs_for_rawgraphs.csv` and `date_certainty_counts_for_rawgraphs.csv` to your laptop. You can use the Files sidebar or run:

```python
from google.colab import files
files.download("photographs_for_rawgraphs.csv")
files.download("date_certainty_counts_for_rawgraphs.csv")
```

In JupyterLite, right-click each file in the file browser and choose **Download**.

These files have different units of observation:

- In `photographs_for_rawgraphs.csv`, one row represents one photograph metadata record.
- In `date_certainty_counts_for_rawgraphs.csv`, one row represents one `date_certainty` category, and `record_count` records how many photographs were assigned to it.

## Step 4. Create a chart in RAWGraphs

1. Open [RAWGraphs](https://app.rawgraphs.io/).
2. Load `date_certainty_counts_for_rawgraphs.csv`, not the 100-row record-level file.
3. Confirm that `date_certainty` is recognized as text and `record_count` as a number.
4. Choose **Bar Chart**.
5. Map the columns as follows:

   ```text
   Bars:  date_certainty
   Size:  record_count
   ```

6. Confirm that every bar has the same value as the aggregated table printed in Colab.

RAWGraphs receives the already aggregated figures. It draws the chart from the six category rows; it does not need to derive the category counts from the 100 record-level rows.

> **Checkpoint 4:** You have downloaded both CSV files, confirmed that the record-level file still has 100 rows, confirmed that the six aggregated counts sum to 100, and created the RAWGraphs bar chart from `date_certainty_counts_for_rawgraphs.csv`.

---

# Part 8. Exercise 3: Create a visualization with Python

Return to `book-sample.csv`. You will ask the LLM to create a bar chart of records by decade.

## Step 1. Prompt the LLM

```text
I am a digital humanities student with no previous Python experience working in [Google Colab]. Write one self-contained Python code cell using pandas and matplotlib to create a bar chart from "book-sample.csv".

Each row represents one recorded edition in a selected teaching sample. Use the existing decade column to count rows by decade.

Requirements:
- Keep missing categories visible if any exist.
- Sort decade labels chronologically rather than alphabetically.
- Print the aggregated decade-and-count table before plotting it.
- Assert that the sum of all bar counts equals the number of input rows.
- Use an honest title that refers to "recorded editions in the teaching sample", not book production or readership.
- Label both axes and begin the numerical axis at zero.
- Make the chart legible but do not use a decorative style or 3D effects.
- Save it as "book-records-by-decade.png" at 300 dpi with a tight bounding box, then display it.
- Do not modify or overwrite the input CSV.

After the code, explain how the aggregation changes the unit of observation and why this chart cannot demonstrate total historical book production.
```

## Step 2. Validate the aggregation

Before interpreting the chart, confirm that:

- the printed counts sum to 500;
- the decades run from the 1500s through the 1600s;
- the 1600s category contains records from the year 1600 rather than a complete decade of observations;
- the title does not claim to depict all printing, production volume, readership, or influence.

Compare the Python chart with the Week 2 RAWGraphs bar chart. The software differs, but both depend on the same choice to aggregate recorded editions by `decade`.

## Step 3. Revise one design choice through prompting

Ask the LLM to make exactly one justified revision, for example:

- display the count above each bar;
- use horizontal bars to make labels easier to read;
- highlight the incomplete `1600s` category without making it appear more important;
- add a short source-and-limitation note beneath the chart.

State the intended reason in the prompt. Do not request “make it more attractive” without identifying a communicative purpose.

## Step 4. Download the visualization

Download `book-records-by-decade.png` through the Files sidebar or with:

```python
from google.colab import files
files.download("book-records-by-decade.png")
```

> **Checkpoint 5:** Your chart has been validated against the aggregated table, its counts total 500, and its wording identifies the data as a teaching sample.

---

# Part 9. Evaluate an LLM-generated analysis

An LLM can write syntactically valid code that answers the wrong question. Use this checklist for every generated script.

## Input and environment

- Does it use the correct filename and exact column names?
- Does it assume Colab-specific features in another environment?
- Does it require a package that is not available?

## Transformation and provenance

- Does it preserve the original file and raw columns?
- Does it silently drop, fill, merge, or recode values?
- Does it report rejected, missing, or unmatched records?
- Does it make derived fields and assumptions explicit?

## Validation

- Are row counts checked before and after transformation?
- Are unique identifiers still unique?
- Do category counts sum to an expected total?
- Are selected records inspected manually?
- Are assertions based on justified expectations rather than desired historical conclusions?

## Historical interpretation

- What is the unit of observation before and after aggregation?
- What does the dataset actually record?
- Why do these records survive and appear in this dataset?
- Which people, objects, activities, or categories are absent?
- Does the output show a pattern, or does the available evidence also explain its cause?

## Reproducibility

- Could another student rerun the notebook from the first cell?
- Are filenames, libraries, and outputs stated?
- Are prompts and LLM-assisted decisions recorded?
- Has the notebook been downloaded with the final outputs visible?

---

# Part 10. Generate an interactive HTML visualization

Python is not the only language an LLM can generate. HTML structures a web page, CSS controls its appearance, and JavaScript can add interaction. In this extension, the LLM will generate one standalone `.html` file that reads a CSV selected by the user.

You do not need Colab for this. Simply copy and paste the LLM output into a text editor, such as Notepad and save it as `interactive-book-decades.html`.

Prompt:

```text
Create one standalone HTML file for a beginning Digital History student. It should let the user choose a local copy of "book-sample.csv" with an HTML file input, parse it in the browser, count records by the existing decade column, and display an accessible interactive bar chart.

Here is an excerpt of the csv:
[paste first 5 rows]

Requirements:
- Use HTML, CSS, and vanilla JavaScript in one file.
- Do not require Python, a web server, npm, or a build step.
- Do not upload the CSV anywhere.
- Show the aggregated table as well as the chart.
- Sort decades chronologically and make the numerical axis begin at zero.
- State clearly that the bars count recorded editions in a teaching sample.
- Show a useful error for an absent decade column or malformed CSV.
- Include comments and then explain how to save and open the file.
- If you use an external library, name it, explain that internet access is required, and load it only from its official CDN. Prefer a solution that does not need an external library.
```

Save the response's code as `interactive-book-decades.html`. Open the file in your web browser and choose `book-sample.csv`. Compare its counts with the Python aggregation. If they differ, the page is not validated merely because a chart appeared.

---

# Part 11. Generate a searchable CSV browser

Use the photograph metadata to create a small local research interface.

Prompt:

```text
Create one standalone HTML file that lets a user choose a local CSV file and then displays it as a searchable, filterable table. Design it for "historic_photographs_sample_dataset.csv".

Here is an excerpt of the first rows:
[paste first 5 rows]

Requirements:
- Use HTML, CSS, and vanilla JavaScript in one file.
- Do not require Python, a web server, npm, or a build step.
- Do not upload the CSV anywhere.
- Provide a keyword search across all displayed fields.
- Provide dropdown filters for country_raw and object_type_raw.
- Display the number of visible records and the total number of records.
- Allow sorting by clicking column headers.
- Preserve the original cell text; do not normalize values silently.
- Treat cell contents as text, not executable HTML, to reduce script-injection risk.
- Handle quoted commas and UTF-8 characters correctly.
- Show useful messages if the expected columns are absent.
- Make the table keyboard-accessible and usable on a narrow screen.
- Add comments and explain how to save, open, and test the file.
```

Save the response's code as `photograph-csv-browser.html`. Test searches, filters, record counts, accented characters, and a title containing punctuation. Ask what the interface makes easier to discover and what it still cannot establish about the historical photographs.

These HTML exercises are examples of **software prototyping**, not proof that the generated page is reliable, secure, accessible, or ready for publication. Public deployment requires more extensive testing. These are for your personal use.

---
# Part 12. Optional Extension

Do one (or all!) of the following:
1. Create your own prompt to create a python script that does something interesting with one of the CSV files.
2. Build an interactive dashboard of one of the CSV files. 
3. Create an interactive visualization.
4. Use your own supplied dataset and apply what you've learned in this tutorial.
5. Use your creativity and come up with your own idea!

Prepare to share this with the class.

---

# Part 13. Save your work

## Download the notebook

In Colab:

1. Select **File → Download → Download .ipynb**.
2. Keep the filename `week-04-llm-python-YOURNAME.ipynb`.
3. Make sure the saved notebook includes the outputs you intend to submit.

Also download:

```text
photographs_for_rawgraphs.csv
date_certainty_counts_for_rawgraphs.csv
book-records-by-decade.png

interactive-book-decades.html
photograph-csv-browser.html
```

Copy your important LLM prompts into text cells in the notebook. A reproducible record should show not only the final code but also the instructions and assumptions that shaped it.

---

# Part 13. Written reflection

Complete your Week 4 journal entry.

1. Report one result you independently validated and explain how you validated it.
2. Identify one missing-data pattern in the book dataset.
3. Describe one way you improved a prompt after seeing code or output.
4. Describe one error or questionable assumption and how you diagnosed it.
5. What does it mean to supervise generated code rather than merely accept it?
6. What parts of this workflow should remain the historian's responsibility even if the code executes correctly?

---

# Files to Submit

Your personal repository should contain:

```text
tutorials/
└── week-04-llm-python/
    ├── week-04-llm-python-YOURNAME.ipynb
    ├── photographs_for_rawgraphs.csv
    ├── date_certainty_counts_for_rawgraphs.csv
    ├── book-records-by-decade.png
    ├── interactive-book-decades.html
    └── photograph-csv-browser.html

journal/
└── week-04.md
```

Optional files from the extra exercise may be included in the Week 4 tutorial folder.

---

# Troubleshooting

## Colab cannot find the CSV

Open the Files sidebar and check whether the file is still present. Colab session storage is temporary. If the runtime restarted, upload the file again and rerun the notebook from the beginning.

## The LLM used the wrong column name

Run:

```python
import pandas as pd
data = pd.read_csv("book-sample.csv")
print(data.columns.tolist())
```

Give the exact output to the LLM and request the smallest correction.

## The code worked earlier but fails now

The earlier cells may no longer have been executed in the current runtime. Select **Runtime → Run all**. Re-upload session files when requested.

## JupyterLite reports that a package is unavailable

Ask the LLM to revise the code for JupyterLite using only `pandas`, `matplotlib`, and Python's standard library. If the package is still unavailable, use Colab, local Jupyter, or a simpler operation. Package compatibility is a limitation of the environment, not evidence that the historical method is invalid.

## The chart appears but the counts are wrong

Do not troubleshoot only the visual design. Print the grouped table, its sum, the input row count, and missing values in the grouping column. Verify those numbers before plotting again.

## Downloaded CSV text displays incorrectly

Confirm that the script wrote the file with UTF-8 encoding. Import it deliberately in Excel or RAWGraphs rather than relying on an application's automatic character-encoding guess.

## The LLM keeps rewriting everything

Ask for the smallest change to the current code. State what already works, paste the error or undesired output, and specify which behavior must remain unchanged.

---

# Recommended Further Learning

If you would like more structured practice after this tutorial, I strongly recommend the following beginner courses from DeepLearning.AI. They are taught by **Andrew Ng**, founder of DeepLearning.AI, co-founder of Coursera, an adjunct professor of Computer Science at Stanford University, and the founding lead of the Google Brain project. These courses are optional and are not required to complete the Applied Digital History course.

## AI Python for Beginners

[AI Python for Beginners](https://www.deeplearning.ai/courses/ai-python-for-beginners) teaches Python from the beginning while showing how an AI assistant can help you write, explain, test, and debug code. It includes practical work with Jupyter notebooks, Python fundamentals, files, functions, and data analysis. No previous programming experience is required.

This is the most direct continuation of the present tutorial. It provides a more systematic introduction to the Python concepts that we have only used selectively here.

## AI Prompting for Everyone

[AI Prompting for Everyone](https://www.deeplearning.ai/courses/ai-prompting-for-everyone) introduces strategies for using modern AI systems more effectively. It covers finding and evaluating information, supplying useful context, brainstorming and critique, working with images and other media, data analysis, and creating simple applications with AI assistance. It does not require a technical background.

This course is useful beyond programming. Its prompting methods can also support research planning, source analysis, writing, visualization, and critical evaluation—provided that you continue to verify outputs against appropriate evidence.

---

# Further Reference

- [Google Colab: Welcome notebook](https://colab.research.google.com/notebooks/welcome.ipynb)
- [Google: Introduction to Google Colab video](https://www.youtube.com/watch?v=inN8seMm7UI)
- [Google Colab: official FAQ](https://research.google.com/colaboratory/faq.html)
- [Project Jupyter: Try Jupyter in the browser](https://jupyter.org/try-jupyter/lab/)
- [JupyterLite documentation](https://jupyterlite.readthedocs.io/)
- [pandas documentation](https://pandas.pydata.org/docs/)
- [Matplotlib documentation](https://matplotlib.org/stable/)

Remember the full evidentiary chain:

> historical activity → contemporary recording → preservation → archival selection → digitization → database construction → computational transformation → analysis → representation

An LLM enters this chain at the stage of computational assistance. It can accelerate the production of code, but it can also introduce errors, hidden assumptions, and unjustified transformations. Running code is not the same as validating it, and validating a computation is not the same as establishing a historical interpretation.
