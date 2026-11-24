# Week 7 Historical Network Analysis

## Overview

This tutorial introduces historical network analysis through a synthetic catalogue of early-sixteenth-century correspondence. You will begin with a single CSV in which each row represents one catalogued letter. You will then use a large language model (LLM) to help write Python code that transforms those letter records into the node and edge tables required for network analysis.

The principal workflow is:

> catalogued letters → correspondents and relationships → node and edge tables → network file → layout and metrics → historical interpretation

In this tutorial:

- a **node** represents a correspondent identified in the catalogue;
- a **directed edge** represents one or more catalogued letters sent from one correspondent to another;
- an edge's **weight** represents the number of catalogued letters assigned to that sender–recipient pair.

You will use Python to create:

- a node table containing one row per correspondent;
- an edge table containing one row per directed sender–recipient pair;
- a GEXF network file that can be opened in Gephi Lite.

You will then use Gephi Lite to explore layouts, visual encodings, centrality measures, and filters. The goal is not merely to produce an attractive network image. It is to understand how the source data, network model, algorithmic operations, and preservation history affect the result.

## What is network analysis?

Network analysis studies entities by examining the relationships among them. Instead of treating every person, place, text, or object as an independent observation, it asks how those entities are connected and what larger structures emerge from the recorded connections.

A network is also called a **graph**. It has two essential components:

- A **node** (also called a vertex) represents an entity. Depending on the question, a node might represent a person, institution, city, book, letter, ship, or other historical object.
- An **edge** (also called a relationship, tie, or link) connects two nodes. It might represent correspondence, kinship, trade, travel, co-membership, citation, collaboration, or another specifically defined relationship.

Nodes and edges can have **attributes**. A person node might have a name, occupation, confession, and city. A correspondence edge might have a date, subject, direction, or number of surviving letters.

An edge can be:

- **directed**, when the direction matters, such as a letter sent from Anna to Matthias;
- **undirected**, when the relationship is treated as mutual, such as two people belonging to the same committee;
- **weighted**, when a numerical value describes its strength or frequency, such as eight catalogued letters rather than one.

A network model is constructed for a question. The same historical material can support several different networks. A catalogue of letters might be modeled as:

- people connected to people by correspondence;
- people connected to letters they sent or received;
- places connected by the movement of letters;
- people connected when they discuss the same subject.

These models are not interchangeable. Each makes some historical relationships visible and leaves others out.

## What can historians do with networks?

Network analysis can help historians:

- identify highly connected or structurally intermediary actors;
- locate clusters or communities of densely connected nodes;
- compare incoming and outgoing relationships;
- trace possible paths through which information, objects, or practices could circulate;
- compare networks across periods, places, or source collections;
- investigate whether relationships cross geographic, confessional, occupational, or institutional boundaries;
- identify isolated or weakly recorded actors;
- formulate questions for close reading of particular sources.

For example, a correspondence network might reveal that one scholar connects two otherwise weakly linked groups. This is a useful observation, but it does not yet demonstrate that the scholar intentionally acted as a broker. The apparent position may result from actual behavior, surviving evidence, the boundaries of the dataset, or the way the network was constructed.

Network analysis is therefore not a replacement for source criticism or close reading. It is a way to describe and investigate relational patterns that must still be interpreted historically.

## What are Gephi and Gephi Lite?

[Gephi](https://gephi.org/) is open-source software for exploring, analyzing, and visualizing networks. It can calculate network measures, apply layout algorithms, filter a graph, encode attributes through size and color, and export network data and images.

There are two related applications:

| Application | Description | Role in this course |
|---|---|---|
| **Gephi Desktop** | A program installed on a computer. It provides the more extensive interface, supports spreadsheet import, larger workflows, plugins, and additional export options. | Useful for advanced or larger projects, but installation is not required for this tutorial. |
| **Gephi Lite** | A lighter browser-based application. It opens graph files such as GEXF and GraphML and provides layouts, appearance controls, filters, and metrics without requiring a desktop installation. | The application used in this tutorial. |

Gephi Lite runs in your browser, but it is not simply a static website displaying a finished image. It loads a graph into an interactive workspace on your computer. You must still save or export the workspace, graph, and images you want to preserve.

Gephi and Gephi Lite do not discover historical relationships automatically. They analyze and display the nodes, edges, weights, directions, and attributes supplied by the researcher.

## Historical and methodological question

The tutorial investigates:

> Which correspondents occupy structurally prominent or intermediary positions within this catalogue of recorded letters, and how might the construction and survival of the catalogue affect those positions?

This is deliberately narrower than asking who was most powerful, influential, or important in the historical society. A network metric describes a position within the modeled network. It does not independently establish social power, intellectual influence, or historical causation.

## Learning objectives

By the end of the tutorial, you should be able to:

1. Explain what nodes, edges, direction, and weight represent in a particular historical network.
2. Distinguish a table of historical records from a node table and an edge table.
3. Use an LLM to help generate and explain Python code for a defined transformation.
4. Validate node and edge tables using row counts, identifiers, missing values, and selected source records.
5. Explain why aggregating individual letters into weighted edges removes information.
6. Create a directed weighted network file from node and edge tables.
7. Explain what an undirected projection simplifies and what information it conceals.
8. Use layouts, visual encodings, metrics, and filters in Gephi Lite.
9. Interpret degree, betweenness, closeness, and eigenvector centrality without treating them as direct measures of historical importance.
10. Distinguish degree, in-degree, out-degree, reciprocity, weak connectivity, and strong connectivity.
11. Explain how archival survival, cataloguing, uncertain identification, and modeling choices shape a network.
12. Preserve the original data and make each computational transformation inspectable.

## Materials and software

You will need:

- `synthetic_correspondence_records.csv` from this tutorial folder;
- access to [ChatGPT](https://chatgpt.com/) or another LLM that can generate Python;
- [Google Colab](https://colab.research.google.com/), JupyterLite, or a local Jupyter environment;
- [Gephi Lite](https://lite.gephi.org/);
- your personal Applied Digital History portfolio repository.

The supplied CSV is entirely **synthetic**. Its people, letters, and catalogue records, which resemble a plausible historical correspondence catalogue, were created for this tutorial.

## Files to preserve

Do not overwrite the supplied CSV. By the end of the tutorial, preserve:

- `synthetic_correspondence_records.csv`;
- `week_07_network_preparation.ipynb`;
- `correspondence_nodes.csv`;
- `correspondence_edges.csv`;
- `correspondence_network_summary.csv`;
- `correspondence_node_metrics.csv`;
- `correspondence_network.gexf`;
- your saved Gephi Lite workspace (`.json`);
- one full-network image (`.png`);
- one filtered-network image (`.png`);
- one ego-network image (`.png`);
- your journal entry.

---

# Part 1. Understand the source table

## One row represents one catalogued letter

Open `synthetic_correspondence_records.csv` in a spreadsheet application (Excel or OpenRefine) or text editor. Do not sort and save over the original file.

The table contains **304 rows of data** and 24 columns. Each row represents one letter known to the fictional catalogue. The record may describe an original letter, a later copy, a printed extract, or a letter known only through a catalogue reference.

Important columns include:

| Column | Meaning |
|---|---|
| `letter_id` | Stable identifier for one catalogued letter |
| `letter_date` | Recorded date or partial date |
| `date_certainty` | Whether the date is exact, approximate, or known only to the month |
| `sender_id` | Stable identifier for the sender |
| `sender_name` | Display name assigned to the sender |
| `sender_gender` | Gender category recorded in the teaching data |
| `sender_role` | Principal occupational or social role assigned in the catalogue |
| `sender_city` | City associated with the sender |
| `sender_confession` | Confessional category assigned in the catalogue |
| `recipient_id` | Stable identifier for the recipient |
| `recipient_name` | Display name assigned to the recipient |
| `subject_category` | Broad modern category assigned to the letter |
| `document_form` | Form in which the letter is known |
| `survival_status` | Whether the text survives or is known only through a reference |
| `repository` | Repository or catalogue in which the record is preserved |
| `catalogue_note` | Note about uncertainty or identification |

The sender and recipient fields repeat person attributes because the same person may appear in either role. Repetition is useful in a record table, but a network node table needs one row per person.

## The table records evidence, not total communication

Even if this correspondance catalogue were real, the table would not represent every letter these people wrote. A letter can appear only if it was written, recorded or retained, preserved or described, catalogued, transformed into structured data.

Keep the following chain in mind:

```text
historical communication
        ↓
letters produced
        ↓
letters retained or mentioned
        ↓
materials preserved
        ↓
materials catalogued
        ↓
records included in this CSV
        ↓
network constructed from the records
```

A person with many recorded letters may have corresponded extensively. They may also have had a better-preserved archive, received more cataloguing attention, or been deliberately selected when the dataset was created.

## Inspect representative records

*This may be easier if you open the csv in Excel or OpenRefine.*

Find and record:

1. one letter for which the original text survives;
2. one record with an uncertain date;
3. one letter involving the unidentified correspondent;
4. a pair of people for whom letters are recorded in both directions.

> **Checkpoint 1:** Explain what one row represents. Explain why 304 rows do not demonstrate that only 304 letters were historically exchanged among these people.

---

# Part 2. Define the network model

## Nodes

For the required network, one node represents one distinct `person_id`. A node table should contain one row for each correspondent and include:

```text
Id
Label
gender
role
city
confession
latitude
longitude
```

`Id` must contain the stable identifier, such as `P001`. `Label` contains the readable name, such as `Anna Keller`.

Do not use names as identifiers. Names can be duplicated, misspelled, translated, abbreviated, or changed. A stable identifier allows a display label to change without changing the identity of the node.

## Edges

For the required network, one directed edge represents all catalogued letters sent by one person to another. The edge table should contain:

```text
Source
Target
Type
Weight
first_date
last_date
surviving_text_count
catalogue_reference_only_count
```

For example:

```csv
Source,Target,Type,Weight
P001,P002,Directed,8
```

This means the catalogue contains eight letter records sent from `P001` to `P002`.

## Preserve direction now; analyze it later

Letters have senders and recipients, so the historically appropriate data model is directed. `P001 → P002` and `P002 → P001` must remain different relationships in the edge table. Do not alphabetize the pair or combine both directions.

Direction will not be the first feature you analyze. The opening visualization and structural measures will temporarily ask only whether two people are connected. This creates an **undirected projection**: a simplified view in which the arrows are ignored but the original directed graph remains unchanged.

After you understand the overall pattern of connections, a later section will restore the arrows and ask what changes when sending and receiving are distinguished.

## Weight requires aggregation

The source table has one row per letter. If eight records have the same sender and recipient, the required edge table combines them into one edge with `Weight = 8`.

Aggregation makes the network easier to analyze, but removes information. A single weighted edge does not display:

- the sequence of the eight letters;
- changes in subject over time;
- intervals between letters;
- differences in document form;
- uncertainty attached to individual records.

Preserve the original CSV so those details remain available for validation.

## Inclusion rule

For the required network, include **all 304 catalogued records**, including the thirteen letters whose text is not preserved. The network therefore represents evidence of catalogued correspondence, not only surviving letter texts.

This is an analytical choice. A later extension will ask what changes if the catalogue-only records are excluded.

> **Checkpoint 2:** In your notebook, state what a node, edge, and weight represent. Explain why the data preserves direction even though the first analysis will temporarily ignore it.

---

# Part 3. Create the notebook and inspect the data

## Step 1. Create the notebook

Open Google Colab or another Jupyter environment. Create a notebook named:

```text
week_07_network_preparation.ipynb
```

Add a text cell containing:

```markdown
# Week 7: Historical Network Analysis

This notebook transforms a synthetic catalogue of letters into node and edge
tables.
```

## Step 2. Upload the CSV once

Upload:

```text
synthetic_correspondence_records.csv
```

If you are using Colab, you may use the Files sidebar or a single `files.upload()` cell. Do not upload a second copy under another name. Remember that the first time you run code in Colab, it
takes longer, since you need to connect to the runtime.

## Step 3. Ask the LLM to inspect before transforming

Use a prompt based on the following template. Change the notebook environment if necessary.

```text
I am a Digital History student with limited Python experience. I am working
in Google Colab with a CSV named "synthetic_correspondence_records.csv".

Write one beginner-friendly Python code cell using pandas. Begin by asking me
to select and upload the CSV once, then load the uploaded file into a dataframe
named letters. Do not modify the input file.

Print and explain:

1. the number of rows and columns;
2. the complete list of column names;
3. the first five rows;
4. whether letter_id is unique;
5. the number of distinct sender_id values;
6. the number of distinct recipient_id values;
7. the number of distinct people appearing as either sender or recipient;
8. counts for date_certainty, document_form, survival_status, and repository;
9. missing-value counts for every column;
10. all records involving an identifier that begins with U rather than P.

Do not create the network tables yet. Do not delete, replace, or silently
ignore missing values. Explain each validation result in plain language.
```

If you uploaded the file through the Colab sidebar, tell the LLM that the file is already uploaded and it should load it directly rather than generating another upload cell.

## Step 4. Run and read the output

Do not evaluate the code only by whether it runs. Confirm that it reports:

```text
304 rows
24 columns
304 unique letter_id values
41 distinct people across sender and recipient fields
```

It should also find:

```text
291 records with survival_status = text_surviving
13 records with survival_status = text_not_surviving
```

If the results differ, check that you loaded the correct file exactly once and that the code combines sender and recipient identifiers before counting distinct people.

> **Checkpoint 3:** The notebook has inspected the unmodified source table and found 304 unique letter records involving 41 distinct person identifiers.

---

# Part 4. Generate and validate the node table

## Step 1. Describe the transformation to the LLM

Continue in the same LLM conversation so that it retains the column information and inspection results. Use or adapt this prompt:

```text
Continue from the preceding task. The dataframe letters is already loaded, so
do not ask me to upload the file again.

Write one beginner-friendly Python code cell that constructs a node table named
nodes from the sender and recipient columns in letters.

Requirements:

1. Create one temporary table from the sender fields and one from the recipient
   fields.
2. Rename both sets to the same columns: Id, Label, gender, role, city,
   confession, latitude, longitude.
3. Stack the two temporary tables and create one row per unique Id.
4. Before removing duplicate appearances, check whether any Id is associated
   with conflicting nonblank values for Label, gender, role, city, confession,
   latitude, or longitude. Print every conflict. Do not silently choose one.
5. Preserve the unidentified correspondent as a node. Do not drop it because
   some attributes or coordinates are missing.
6. Sort the finished table by Id.
7. Confirm that Id is unique and contains no missing values.
8. Print the number of nodes and missing-value counts for every column.
9. Display the full row for the unidentified correspondent and five other rows.
10. Save the result as "correspondence_nodes.csv" without a pandas index.
11. Do not modify the original letters dataframe.

After the code, explain in plain language why stacking is necessary, why Id is
different from Label, and what each row in nodes represents.
```

After running this code in Colab, you should now see `correspondence_nodes.csv` in the files sidebar on the left.

## Step 2. Validate the node table

The completed table should contain:

```text
41 rows
8 columns
41 unique nonblank Id values
```

Check that:

- `P001` occurs exactly once;
- `U001` occurs exactly once;
- the unidentified correspondent remains present despite missing coordinates;
- the names and attributes are consistent between sender and recipient appearances;
- no node was created from a blank identifier.

Do not treat a lack of reported conflicts as proof that the historical identification is correct. It establishes only that this CSV uses the attributes consistently.

> **Checkpoint 4:** `correspondence_nodes.csv` contains 41 unique people, including the unidentified correspondent.

---

# Part 5. Generate and validate the edge table

## Step 1. Describe the aggregation

Continue in the same LLM conversation and use or adapt this prompt:

```text
Continue from the preceding task. The dataframes letters and nodes already
exist. Do not upload or reload the CSV.

Write one beginner-friendly Python code cell that constructs a directed edge
table named edges from letters.

Requirements:

1. Group records by sender_id and recipient_id without reversing or
   alphabetizing the pair.
2. Rename sender_id to Source and recipient_id to Target.
3. Add Type with the value "Directed" for every row.
4. Set Weight to the number of catalogued letter records in that directed pair.
5. Add first_date and last_date based on the recorded letter_date values.
   Preserve the source letter_date column unchanged. Explain any limitation
   caused by month-only or approximate dates.
6. Add surviving_text_count for records whose survival_status is
   text_surviving.
7. Add catalogue_reference_only_count for records whose survival_status is
   text_not_surviving.
8. Sort the result by Weight from largest to smallest, then by Source and
   Target.
9. Confirm that Source and Target contain no missing values.
10. Confirm that every Source and Target value exists in nodes.Id. Print any
    unmatched identifiers rather than ignoring them.
11. Check for self-loops and duplicate Source–Target pairs and print any found.
12. Assert that the sum of Weight equals the number of rows in letters.
13. Assert that surviving_text_count plus catalogue_reference_only_count equals
    Weight for every edge.
14. Print the input letter count, output edge count, total edge weight, minimum
    and maximum Weight, and the first ten edges.
15. Save the result as "correspondence_edges.csv" without a pandas index.
16. Do not modify letters or nodes.

After the code, explain what one row in letters represents, what one row in
edges represents, and what information was lost through aggregation.
```
After running this code in Colab, you should now see `correspondence_edges.csv` in the files sidebar on the left.


## Step 2. Validate the edge table

The completed table should contain:

```text
82 directed edges
total Weight = 304
```

Check that:

- every endpoint exists in the node table;
- there is no duplicate `Source`–`Target` pair;
- all weights are positive whole numbers;
- reciprocal relationships remain two separate edges;
- the thirteen catalogue-only records remain represented in the appropriate edge counts.

## Step 3. Manually verify selected edges

Choose three edges:

1. the edge with the largest weight;
2. an edge with `Weight = 1`;
3. one direction of a reciprocal pair.

For each edge, filter `letters` by its `Source` and `Target`. Confirm that the number of matching letter records equals the reported weight. Inspect their letter IDs, dates, document forms, and survival statuses.

This validation returns from the aggregated relationship to the records on which it is based.

> **Checkpoint 5:** `correspondence_edges.csv` contains 82 unique directed pairs whose weights sum to all 304 source records. Three selected weights have been checked against the original letters.

---

# Part 6. Create a GEXF network file

Gephi Lite reads graph formats such as GEXF and GraphML rather than separate node and edge CSV files. The CSV tables remain important because they make the constructed network inspectable. Python will now combine them into a GEXF file for Gephi Lite.

## Step 1. Ask the LLM to construct the graph

Continue in the same conversation:

```text
Continue from the preceding task. The validated dataframes nodes and edges
already exist. Write one beginner-friendly Python code cell using networkx to
create a directed weighted graph named G and export it as
"correspondence_network.gexf".

Requirements:

1. Create a networkx.DiGraph because the edges are directed.
2. Add every row in nodes as one graph node, using Id as the graph identifier.
3. Add Label, gender, role, city, confession, latitude, and longitude as node
   attributes. Do not convert missing values to the strings "nan" or "None";
   omit an attribute when its value is missing.
4. Add every row in edges as one directed graph edge from Source to Target.
5. Store Weight as the numerical network weight attribute named "weight".
6. Also preserve Type, first_date, last_date, surviving_text_count, and
   catalogue_reference_only_count as edge attributes.
7. Print whether the graph is directed, the number of nodes, the number of
   edges, and the sum of all edge weights.
8. Confirm that the graph contains exactly the same node identifiers as nodes.Id.
9. Confirm that every edge and weight in edges appears in G.
10. Write the graph as "correspondence_network.gexf".
11. Read the saved GEXF back into a new graph named G_check and repeat the node,
    edge, direction, and total-weight checks. Do not assume a successful save
    means a correct file.

After the code, explain the relationship among the original letter table, the
node table, the edge table, and the GEXF file.
```

If `networkx` is unavailable, ask the LLM for the smallest environment-appropriate installation step. Google Colab normally includes it.

## Step 2. Confirm the exported graph

The validation should report:

```text
directed graph: True
nodes: 41
edges: 82
total edge weight: 304
```

Download the node CSV, edge CSV, GEXF, and notebook before the notebook runtime ends.

> **Checkpoint 6:** All three files have been saved to your repository.

---

# Part 7. Open and inspect the network in Gephi Lite

## Step 1. Open the graph

1. Open [Gephi Lite](https://lite.gephi.org/) in a modern browser.
2. Choose the option to open a local graph file.
3. Select `correspondence_network.gexf`.
4. Read any import report or warnings before continuing.

Gephi Lite operates in the browser. The imported graph is not the same as a permanently saved project. Save or export your work before closing the tab.

## Step 2. Inspect the data tables

Open **Data** and inspect both **Nodes** and **Edges**.

Confirm:

- 41 nodes appear;
- 82 edges appear;
- node identifiers and labels are distinct fields;
- the nodes retain attributes such as city, role, and confession;
- the edges retain weight and survival-count attributes;
- the graph is directed.

The imported file preserves arrow direction, but do not analyze sending and receiving yet. For Parts 7–9, treat a visible line as evidence that two correspondents are connected. If Gephi Lite allows arrowheads to be hidden through **Appearance**, you may hide them temporarily. This changes only the display, not the directed graph data.

Search for `U001`. Confirm that the node exists even though some of its attributes are missing.

Search for one manually validated edge and confirm that its weight matches the CSV and original records.

If the counts differ, stop and return to the Python validation. Do not continue styling an incorrectly imported graph.

> **Checkpoint 7:** Gephi Lite contains the same 41 nodes and 82 edges as the validated Python tables.

---

# Part 8. Apply and compare layouts

## What a layout does

A network layout algorithm assigns visual positions to nodes. The source catalogue does not contain these positions. A force-directed layout usually pulls connected nodes together and pushes disconnected or weakly connected nodes apart.

The resulting distances are calculated visual arrangements. They are not automatically:

- geographic distance;
- social intimacy;
- ideological similarity;
- chronological sequence;
- institutional hierarchy.

## Step 1. Preserve the initial view

Before changing the layout, export or capture an image (screenshot) of the initial network. Record whether the initial arrangement makes any structure easy to interpret.

## Step 2. Apply ForceAtlas2

Open **Layout** and choose **ForceAtlas2**. ForceAtlas2 is a force-directed layout. It treats connected nodes somewhat like objects joined by springs:

- edges attract their connected nodes toward one another;
- repulsion pushes nodes away from other nodes;
- repeated calculations move the nodes until the arrangement changes more slowly.

The layout does not normally produce a single correct finished image. It creates positions through an iterative process.

### Generate suitable starting settings

Next to the **Start** button is a small settings button. Hover over it and confirm that its tooltip says:

```text
Generate settings that fit the current graph best
```

Select this button before starting the layout. Gephi Lite will inspect the current graph and propose ForceAtlas2 settings. For this teaching network, one test produced approximately:

```text
Gravity: 0.05
Scaling ratio: 10
Slow down: 4.71
Strong gravity mode: selected
```

The exact slowdown value may contain many decimal places, and values may differ between software versions or graph states. Use the generated settings rather than manually copying these example numbers. Record the values Gephi Lite generates for you.

### Start and stop the calculation

1. Select **Start**. The nodes should begin moving as ForceAtlas2 repeatedly recalculates their positions.
2. Watch the graph while the largest movements occur. Do not repeatedly select **Start**; the layout is already running.
3. When the general arrangement changes only slightly, select **Stop**.
4. Wait until the layout is stopped before trying to read labels, change appearance settings, or export an image.

“Run until stable” therefore means watching the animated movement and stopping the calculation once additional movement no longer reveals a substantially different structure. It does not mean waiting for a formal message saying that the historically correct arrangement has been found.

## Step 3. Spread out a crowded network

The generated settings should produce a much clearer arrangement than the default settings. The nodes may nevertheless remain too close together for the edges to be read easily.

Make the following adjustments in order. Stop the layout before changing a setting.

1. Increase **Scaling ratio** from `10` to `20`.
2. Select **Start**, watch the nodes spread out, and select **Stop** when the arrangement settles.
3. If the network is still crowded, try `30`. Increase the value gradually rather than jumping immediately to an extreme setting.
4. Keep the generated gravity settings unchanged during these first tests.
5. Record the final scaling ratio you used.

The scaling ratio controls the strength of repulsion. A higher value pushes nodes farther apart. It changes their visual positions, not the underlying nodes or edges.

If a scaling ratio of approximately `30` still leaves the graph tightly compressed, try one additional change:

- either clear **Strong gravity mode**;
- or reduce **Gravity** slightly.

Change only one of these at a time, then start and stop the layout again. Gravity pulls the network toward the center. Strong gravity prevents peripheral nodes from drifting far away, but it can also keep the drawing compact.

Do not increase **Slow down** in order to create more space. Slowdown controls how cautiously or quickly nodes move between iterations; it does not directly determine their final separation.

Keep labels hidden or small while adjusting the layout, then restore selected labels after the positions are stable. Note one cluster or intermediary node that becomes easier to see.

## Step 4. Size nodes by degree

After ForceAtlas2 has stopped and the nodes are reasonably separated:

1. Open **Appearance**.
2. Select **Nodes**.
3. Under **Size**, choose the option to **set size from** a data field or metric.
4. Select **Degree**.
5. Adjust the minimum and maximum sizes if the largest circles conceal too many edges.

Nodes with more recorded connections will now appear larger. Size represents degree within the modeled network, not a person's general historical importance. Because the imported graph preserves direction, Gephi Lite's degree may count incoming and outgoing edges separately. Part 12 will distinguish those directions; for now, use size only as an orientation aid showing comparatively connected nodes.

Changing node size does not rerun ForceAtlas2 or alter the network relationships. If the larger nodes now overlap, increase the scaling ratio slightly and run the layout again briefly, then stop it.

## Step 5. Compare the stages

Preserve or capture three views if possible:

1. the initial crowded graph;
2. ForceAtlas2 using Gephi Lite's generated settings;
3. the final version after increasing the scaling ratio and sizing nodes by degree.

In your notes, identify one feature that became clearer and one impression that may result from the layout settings rather than the historical evidence.

> **Checkpoint 8:** You used the generated ForceAtlas2 settings, adjusted the scaling ratio if necessary, sized nodes by degree, recorded the final values, and can explain why node position and node size represent different things.

---

# Part 9. Encode attributes through appearance

Visual variables should represent explicitly chosen data. Do not allow size, color, and position to acquire undefined meanings.

Node size is already based on **Degree** from Part 8. Keep that setting while adding color, edge size, and labels.

## Step 1. Color nodes by city or gender

1. Open **Appearance**.
2. Select **Nodes**.
3. Select **Color**.
4. Choose the option to set color from an attribute.
5. Select either `city` or `gender`.

Both are useful choices:

- Coloring by `city` can reveal whether correspondents associated with the same place cluster together or connect across places.
- Coloring by `gender` can help identify how the recorded connections are distributed across the catalogue's gender categories.

Choose one attribute for the required view. Note briefly what the colors make easier to see. Do not imply that a catalogue category necessarily captures a stable or complete historical identity.

## Step 2. Size edges by weight

1. Open **Appearance**.
2. Select **Edges**.
3. Select **Size**.
4. Choose the option to set size from an attribute.
5. Select `weight`.

Edges representing more catalogued letters should now appear thicker. Adjust the minimum and maximum sizes if necessary. Retain a visible minimum thickness so that edges with `weight = 1` do not disappear, but avoid making the largest edges obscure nearby connections.

Do not assume a thicker edge represents affection, influence, or agreement. It represents more catalogued letters. Direction will be interpreted in Part 12.

## Step 3. Display node names

1. Open **Appearance**.
2. Select **Labels**.
3. Select **Nodes**.
4. Under **Set label from**, select `Label`.

The node labels should now display the correspondents' names rather than their identifiers. Adjust the label size if names overlap heavily. You may temporarily hide labels while changing the layout and restore them afterward.

## Step 4. Display edge weights

1. Remain in **Appearance → Labels**.
2. Select **Edges**.
3. Under **Set label from**, select `weight`.

Each edge should now display the number of catalogued letters represented by that directed relationship. Edge labels can become crowded in the complete network. Zoom into a smaller part of the graph when checking particular values, and confirm important weights against `correspondence_edges.csv` rather than relying only on the display.

> **Checkpoint 9:** Node size represents degree, node color represents either city or gender, edge size represents weight, node labels show names, and edge labels show letter counts.

---

# Part 10. Filter the network

## Step 1. Save the complete-network view

At the top left of Gephi Lite, select **Workspace → Export image**. Save the complete network as:

```text
correspondence_network_full.png
```

## Step 2. Filter by degree

1. Open **Filters** in the left sidebar.
2. Select **Add filter**.
3. In the popup, find **Nodes attributes** and select **degree**.
4. Change the minimum value to `4`.
5. Confirm that the display now retains only nodes meeting the selected degree threshold. Filtering hides nodes from the current view; it does not delete them from the graph.
6. Select **Workspace → Export image** and save the filtered view as:

```text
correspondence_network_filtered.png
```

When finished, select **Delete filter**. Remove the degree filter before creating the ego network; otherwise the two filters will affect the display simultaneously.

## Step 3. Create an ego network

An **ego network** shows one selected node—the **ego**—together with the nodes directly connected to it and the edges among the displayed nodes. It changes the scale of inspection from the complete network to one person's immediate relational neighborhood.

1. Open **Filters**.
2. Select **Add filter**.
3. In the popup, find **Topology** and select **Ego network**.
4. Choose `P017`, Magdalena Frisch, as the central node.
5. Set **Depth** to `1`.
6. Set **Follow** to **Any edges** so that the first view includes connections regardless of direction.
7. Apply the filter.

The exact labels may vary with the Gephi Lite version. Confirm that the resulting view contains Magdalena Frisch and a limited set of connected correspondents rather than the complete graph.

### Compare depths

With **Follow** still set to **Any edges**, change **Depth** one step at a time:

1. At depth `1`, inspect Magdalena and her immediate neighbors.
2. Change the depth to `2`. The filter can now follow connections from Magdalena's neighbors to people two steps away.
3. Change the depth to `3`. The visible subgraph can now include people reached within three steps.

Note how quickly the visible network expands and at which depth it begins to resemble most of the complete graph.

A larger depth does not mean a stronger relationship. It means that the filter is permitted to travel through more consecutive edges.

### Compare edge directions

Return **Depth** to `1`. Change **Follow** among the three available choices:

- **Inbound edges** follows relationships pointing toward Magdalena and emphasizes recorded senders to her.
- **Outbound edges** follows relationships pointing away from Magdalena and emphasizes her recorded recipients.
- **Any edges** follows either direction and shows the broadest immediate neighborhood.

Compare the three views and identify at least one correspondent who appears only in an inbound or outbound result. An absent reverse edge may reflect preservation or cataloguing rather than the absence of a historical reply.

Set **Depth** to `1` and **Follow** to **Any edges**, then export the view as:

```text
correspondence_ego_network_P017.png
```

If time permits, repeat the exercise with another correspondent of your choice and compare the two ego networks. Their local views may help explain the distinction between many direct connections and a position linking broader parts of the network.

An ego network is useful for close inspection, but a shallow depth conceals most of the complete network. A node's betweenness depends on paths across the wider graph and may therefore be difficult to understand from a depth-one ego network alone.

> **Checkpoint 10:** You exported full, degree-filtered, and ego-network views and compared ego-network depth and direction settings.

---

# Part 11. Calculate and translate network metrics

You have now seen the complete graph, experimented with its layout, and encoded degree, city or gender, edge weight, names, and letter counts. The visualization provides a broad overview. The next step uses numerical measures to investigate particular structural positions in more detail.

The centrality values will initially look unfamiliar. You are not expected to calculate them by hand. Python will calculate them reproducibly. Your task is to understand what question each measure asks, inspect which people receive high values, and decide what the results can and cannot support historically.

## Why calculate the required metrics in Colab?

Gephi Lite can calculate several metrics, but the required calculations will remain in the notebook because the code:

- records exactly which graph model was used;
- distinguishes the directed graph from the unweighted undirected projection;
- preserves the calculations with the rest of the workflow;
- produces CSV tables that can be inspected and submitted;
- makes it easier to rerun the analysis after changing the data.

Divide the calculation into two smaller tasks rather than asking the LLM to generate everything at once.

## Task 1. Summarize the overall network

Continue in the same notebook used to create the directed graph `G`.

```text
Continue from the preceding task. The directed weighted NetworkX graph G already
exists. Do not reload the data and do not modify G.

For an introductory structural analysis, create an unweighted undirected
projection named G_undirected. Explain that it records whether two people are
connected by at least one catalogued letter in either direction. It ignores
arrow direction and letter frequency.

Write beginner-friendly Python that:

1. reports the number of nodes and undirected edges;
2. calculates undirected density;
3. calculates average degree;
4. reports the number and sizes of connected components;
5. tests whether G_undirected is connected;
6. if connected, calculates average shortest-path length and diameter;
7. if disconnected, calculates those two path measures only for the largest
   connected component and reports how many nodes were excluded;
8. stores the results in a one-row dataframe named network_summary;
9. adds a model column containing "unweighted undirected projection";
10. saves it as "correspondence_network_summary.csv" without a pandas index;
11. explains every printed result in plain language.

Do not use correspondence weight as a shortest-path distance. Preserve full
precision for calculations and round only displayed or exported values.
```

### Understanding the summary measures

- **Density** compares the observed connections with every undirected connection that could exist among the included nodes.
- **Average degree** is the average number of distinct correspondents connected to each node.
- A **connected component** is a group within which every node can be reached through some path.
- **Average shortest-path length** is the mean number of steps in the shortest routes between node pairs in the connected graph or selected component.
- **Diameter** is the largest of those shortest-path distances.

These values describe the modeled catalogue. They do not measure the completeness of historical communication.

## Task 2. Calculate four centrality measures

Use a second prompt after the graph summary works:

```text
Continue from the preceding task. G_undirected and network_summary already
exist. Do not recreate or modify the original directed graph G.

Write beginner-friendly Python that calculates these unweighted measures from
G_undirected:

1. degree centrality;
2. betweenness centrality;
3. closeness centrality;
4. eigenvector centrality.

If eigenvector centrality does not converge, print a clear warning and try a
larger iteration limit rather than substituting zero values.

Join the results to the existing node Id and Label values in a dataframe named
node_metrics. For each measure, display the five highest-scoring nodes with Id,
Label, and the score. Save the complete table as
"correspondence_node_metrics.csv" without a pandas index.

Round only the displayed or exported values to four decimal places. Preserve
full precision in the working calculations. After the code, explain in one or
two plain-language sentences what structural feature each measure describes.
Do not interpret any measure as historical importance or influence.
```

## What do the numbers mean?

The scores use different calculations and should not be compared as though they shared one unit.

### Degree centrality

Degree centrality is the easiest to connect to a count:

```text
degree centrality = number of connected neighbors ÷ maximum possible neighbors
```

There are 41 nodes, so each person has 40 possible neighbors. A degree-centrality score of `0.125` means:

```text
5 connected correspondents ÷ 40 possible correspondents = 0.125
```

It does not mean that she possesses “12.5% of the network's influence.”

### Betweenness centrality

Betweenness centrality increases when many shortest paths between other nodes pass through a person. The normalized decimal allows comparison within the network, but it is not a percentage of letters relayed or evidence that the person actually acted as a broker.

### Closeness centrality

Closeness centrality summarizes how few steps separate a node from the others. A larger value indicates shorter routes within the modeled graph. It does not measure the speed of historical communication.

### Eigenvector centrality

Eigenvector centrality becomes larger when a person is connected to people who are themselves well connected. The score is relative to this network and has no direct historical unit.

## Use an LLM to translate the tables

The calculations identify structural patterns, but the raw decimals are difficult to read. Use the LLM as a translator between numerical output and comprehensible prose.

Paste your four top-five tables into the same LLM conversation and use a prompt based on this template:

```text
The following tables contain degree, betweenness, closeness, and eigenvector
centrality calculated from an unweighted undirected projection of a synthetic
historical correspondence catalogue.

[PASTE THE FOUR TABLES HERE]

Translate these numerical results into clear prose for a history student who is
new to network analysis.

Requirements:

1. Explain what each measure describes before interpreting its ranking.
2. Explain degree centrality using the number of actual connections where
   possible. There are 41 nodes and therefore 40 possible neighbors per node.
3. Identify people who rank highly across several measures.
4. Compare two people whose rankings illustrate different kinds of structural
   centrality—for example, one with high degree and another with high
   betweenness.
5. Distinguish direct reach from a position on shortest paths.
6. Do not describe any score as a percentage of influence, importance, letters
   transmitted, or actual information flow.
7. Do not claim that a high-betweenness person historically acted as a broker.
8. State that the analysis uses an unweighted undirected projection: one letter
   and many letters create the same connection, and direction is ignored.
9. Separate network observations from possible historical interpretations and
   identify what would require checking in the underlying letters.
10. Do not write my final historical argument. Your purpose is to translate the
    figures and help me understand them.
```

The LLM is not performing the historical interpretation for you. Python produced the measures; the LLM is explaining what those figures mean in ordinary language. You must evaluate the explanation and return to the evidence.

## Evaluate the translation

Check whether the response:

- converts at least one degree-centrality score back into a count of connected neighbors using 40 possible neighbors;
- distinguishes broad direct reach from a position on many shortest paths;
- avoids translating betweenness, closeness, or eigenvector scores into misleading percentages;
- names people only where the supplied tables support the claim;
- remembers that the projection ignores direction and correspondence volume;
- distinguishes a structural observation from a claim about historical behavior;
- identifies claims that require inspecting the original letter records.

Correct the LLM if it overstates a result. Ask it to explain an unfamiliar sentence more simply rather than accepting technical language you cannot explain yourself.

Then write a short paragraph identifying the strongest direct and intermediary positions, followed by one limitation and one claim that would require checking the letters.

## Gephi Lite also has a Metrics panel

Gephi Lite's left sidebar includes **Metrics** alongside Layout, Appearance, and Filters. The current interface groups metrics such as:

- **Nodes:** Louvain community detection, PageRank, betweenness centrality, degree with incoming and outgoing options, and HITS;
- **Edges:** disparity, Simmelian strength, and chi-square measures;
- **Mixed:** Louvain edge ambiguity.

Calculating a metric does not automatically change the visualization. It adds a new data column to the graph. You must then inspect that column in **Data** or deliberately select it under **Appearance** to encode it through size or color.

For this tutorial, keep the Colab calculations as the required, reproducible analysis. You do not need to export another dataset from Gephi Lite. As an optional comparison, calculate **Degree** or **Betweenness centrality**, inspect the new node column in **Data**, and compare the ranking with the Python table. First confirm whether Gephi Lite used the directed graph, an incoming or outgoing degree, or a currently filtered graph; otherwise the values may answer a different question.

> **Checkpoint 11:** You calculated the network summary and four centrality measures, used an LLM to translate rather than replace your interpretation, checked the translation against the tables, and wrote your own evidence-conscious summary.

---

# Part 12. What changes when direction matters?

The undirected projection established the overall pattern of who is connected to whom. Return now to the original directed graph `G` and distinguish sending from receiving.

## Read the arrows

In this correspondence network:

```text
P001 → P002
```

means that at least one catalogued letter was sent by `P001` to `P002`. The reverse relationship requires a separate directed edge:

```text
P002 → P001
```

If both edges exist, the relationship is reciprocal in the catalogue. If only one exists, this does not prove that historical communication occurred only in one direction. Replies may not have been written, preserved, identified, catalogued, or included.

## Directed measures

| Measure | Question it addresses | Important limitation |
|---|---|---|
| Directed density | What proportion of all possible sender–recipient directions is recorded? | Treats each direction as a separate possibility |
| Average in-degree | From how many distinct correspondents does a person receive recorded letters on average? | Missing incoming letters lower the value |
| Average out-degree | To how many distinct correspondents does a person send recorded letters on average? | Does not count unrecorded recipients |
| In-degree centrality | How broadly does a particular person receive within this network? | Does not automatically measure popularity or authority |
| Out-degree centrality | How broadly does a particular person send within this network? | Does not measure all historical writing activity |
| Reciprocity | How often is a recorded directed connection accompanied by one in the reverse direction? | Highly sensitive to unequal preservation |
| Weak component | A group connected when arrow direction is ignored | Does not require two-way reachability |
| Strong component | A group in which directed paths run between every pair in both directions | Can be fragmented by a single missing direction |

## Ask the LLM to calculate directed measures

Continue from the code used in Part 11. `G` must still be the original directed graph, while `G_undirected` is the simplified projection.

```text
Continue from the preceding network analysis. The original directed graph G,
the undirected projection G_undirected, node_metrics, and network_summary all
already exist. Do not recreate or replace G.

Write beginner-friendly Python that introduces direction and compares the
directed graph with the undirected projection.

Requirements:

1. Confirm that G is directed and G_undirected is not.
2. Report the node count, directed edge count, and total correspondence weight
   in G.
3. Calculate directed density.
4. Calculate average in-degree, average out-degree, and average total degree.
5. Calculate reciprocity.
6. Report the number and sizes of weakly connected components.
7. Report the number and sizes of strongly connected components.
8. Calculate in-degree centrality and out-degree centrality for every node.
9. Add the in-degree and out-degree centrality values to node_metrics using Id.
10. Display the five highest-scoring nodes for each directed centrality.
11. Compare those lists with the undirected degree-centrality ranking.
12. Add clearly named directed measures as new columns in the existing one-row
    network_summary dataframe. Preserve the existing undirected results.
13. Save the updated node_metrics and network_summary dataframes to the same
    CSV filenames used previously, without pandas indexes.
14. Print several reciprocal pairs and several one-direction-only pairs.
15. Explain every result in plain language and identify how missing letters
    could affect it.

Do not calculate a single directed diameter for the whole graph unless the
graph is strongly connected. The undirected path measures from Part 11 remain
the required introductory diameter and average shortest-path results.
```

## Use an LLM to translate the directed results

The directed calculations add several unfamiliar values and categories. As with the earlier centrality tables, use the LLM to translate the results into accessible prose rather than asking it to produce the historical interpretation.

Copy the following from your notebook:

- directed density;
- average in-degree and average out-degree;
- reciprocity;
- the numbers and sizes of weakly and strongly connected components;
- the top five in-degree-centrality results;
- the top five out-degree-centrality results;
- representative reciprocal and one-direction-only pairs.

Then use a prompt based on this template:

```text
The following results were calculated from a directed network constructed from
a synthetic catalogue of historical correspondence.

[PASTE THE DIRECTED SUMMARY, RANKINGS, AND EXAMPLE PAIRS HERE]

Translate these results into clear prose for a history student who is new to
directed network analysis.

Requirements:

1. Explain directed density, average in-degree, average out-degree, reciprocity,
   weak components, strong components, in-degree centrality, and out-degree
   centrality in plain language.
2. Identify people who rank differently by incoming and outgoing connections.
3. Explain the difference between a weak component and a strong component.
4. Explain what the reciprocity value says about the recorded edges.
5. Compare the directed findings with the earlier undirected projection.
6. Do not describe in-degree as popularity, out-degree as influence, or
   reciprocity as proof of mutual exchange without qualification.
7. Explain how missing, unidentified, uncatalogued, or excluded letters could
   change every directed measure.
8. Separate observations calculated from the network from possible historical
   interpretations.
9. Identify which claims require checking the underlying letter records.
10. Do not write my final historical argument. Translate the numerical results
    so that I can evaluate and interpret them myself.
```

## Evaluate the directed translation

Check whether the response:

- distinguishes incoming from outgoing connections;
- explains reciprocity without assuming that every missing reverse edge represents an unanswered letter;
- distinguishes weak from strong connectivity;
- compares the directed and undirected models rather than treating them as interchangeable;
- names people only where your pasted rankings support the claim;
- avoids turning structural prominence into historical importance or influence;
- identifies evidence that must be checked in the original records.

Correct any overstatement. Then write your own short paragraph explaining:

1. which correspondents have the broadest recorded incoming and outgoing reach;
2. whether the recorded correspondence is strongly reciprocal;
3. what direction revealed that the undirected projection concealed;
4. which apparent asymmetries might result from preservation or cataloguing.

> **Checkpoint 12:** You calculated the directed measures, used an LLM to translate them into accessible prose, checked its explanation, and can distinguish degree, in-degree, out-degree, reciprocity, weak connectivity, and strong connectivity.

---

# Part 13. Check one edge against the letter records

Every edge in the network was created by aggregating rows from `synthetic_correspondence_records.csv`. Check one edge to see the connection between the visualization and its underlying records.

1. Choose one visible edge and note its sender, recipient, and displayed weight.
2. Find the same `Source` and `Target` in `correspondence_edges.csv`.
3. In the original letter table, filter for records with that `sender_id` and `recipient_id`.
4. Confirm that the number of matching letter rows equals the edge weight.
5. Inspect the dates, document forms, survival statuses, and repositories of those records.

Write two sentences using this structure:

> **Records:** The catalogue contains [number] letter records sent from [sender] to [recipient].  
> **Network:** These records appear as one directed edge with weight [number].  

This is the basic validation movement used throughout digital history: return from an aggregated result to the records that produced it.

> **Checkpoint 13:** One edge weight matches its underlying letter records, and its limitation has been stated.

---

# Part 14. Save and export the Gephi Lite project

Use the **Workspace** menu to preserve:

1. a Gephi Lite workspace file (`.json`), which retains project state such as appearance, filters, and layout settings;
2. an exported graph file (`.gexf`);
3. the required PNG images.

Name the workspace clearly, for example:

```text
week_07_correspondence_network_workspace.json
```

The workspace and graph file serve different purposes. The graph file preserves the graph and its attributes for exchange. The workspace also preserves more of the Gephi Lite interface state. Workspace compatibility can vary between versions, so keep the GEXF and CSV tables as well.

---

# Part 15. Interpret the network historically

Write one short paragraph proposing a reasonable historical inference based on the graph. State whether the supplied evidence is sufficient to support it. If it is not, identify the additional letters, contextual information, or source criticism needed to test the inference.

Present the inference as a claim to investigate, not as a fact established automatically by the visualization or centrality scores.

---

# Part 16. Optional extensions

## Extension A. Compare structural and geographic layouts

This extension builds directly on last week's work with geocoding and spatial analysis. The node table contains latitude and longitude coordinates for identified places. In Gephi Lite, select the geographic layout and use the latitude and longitude fields to position the nodes. You may need to significantly increase the scaling ratio to make the nodes readable on the map.

Compare:

- a force-directed structural layout;
- a geographic layout.

Briefly explain what each representation makes visible. Geographic proximity and network proximity answer different questions.

## Extension B. Create one alternative network

Choose one of the following changes:

- include only records where `survival_status == "text_surviving"`;
- include only one `subject_category`;
- divide the letters into two time periods.

Create a second edge table and GEXF using the selected subset, then compare it with the complete network. Identify the principal change in edges, layout, or centrality rankings and explain how the changed inclusion rule produced a different model.

Treat differences cautiously. They can result from historical behavior, preservation, catalogue categories, uncertain dates, or the boundaries you selected.

---

# Files to submit

Upload the following to `weekly-work/week-07` in your personal portfolio:

## Required data and code

- `synthetic_correspondence_records.csv`
- `week_07_network_preparation.ipynb`
- `correspondence_nodes.csv`
- `correspondence_edges.csv`
- `correspondence_network_summary.csv`
- `correspondence_node_metrics.csv`
- `correspondence_network.gexf`

## Required Gephi Lite outputs

- `week_07_correspondence_network_workspace.json`
- `correspondence_network_full.png`
- `correspondence_network_filtered.png`
- `correspondence_ego_network_P017.png`

## Journal

Complete your Week 7 journal entry. Address:

1. What material did you work with?
2. What transformations did Python perform?
3. What validation or methodological decision mattered most?
4. What did the network make visible, and what can it not establish?
5. Could network analysis contribute to your final project?

## Optional

Upload any additional comparison networks or images if you completed an extension.

---

# Troubleshooting

## Python reports a file-not-found error

Check the exact filename and the notebook file list. Colab runtime storage is temporary. If the runtime restarted, upload the CSV again and rerun the notebook from the beginning.

## Python creates more than 41 nodes

Check whether the sender and recipient fields were renamed to a common schema before stacking. Deduplicate by stable `Id`, not by the full row or by `Label` alone. Print conflicting attributes rather than silently selecting a version.

## Python creates only 40 nodes

Check whether the code dropped rows with missing coordinates or other attributes. `U001` must remain a node. A person does not cease to exist in the network because an attribute is missing.

## Python creates 304 edges

The code probably preserved one edge per letter rather than aggregating by directed sender–recipient pair. Group by `sender_id` and `recipient_id`, then count records to create `Weight`.

## The edge weights do not sum to 304

Check whether the code filtered out catalogue-only records, missing attributes, or the unidentified correspondent. The required model includes all 304 records. Print excluded records rather than silently dropping them.

## Reciprocal edges were combined

Do not sort the two endpoint identifiers within each row. `P001 → P002` and `P002 → P001` must remain separate directed pairs.

## The GEXF export fails because of missing values

Ask the LLM to omit missing node or edge attributes when adding them to NetworkX. Do not convert missing values to misleading strings or remove the entire node.

## Gephi Lite shows the wrong number of nodes or edges

Return to Python and reopen the saved GEXF with NetworkX. Confirm 41 nodes, 82 edges, and total weight 304. Inspect any import warning. Do not continue until the graph file and validated tables agree.

## The network appears as an unreadable cluster

In ForceAtlas2, first select **Generate settings that fit the current graph best** beside the Start button. Start the layout, wait for the major movement to slow, and stop it. If nodes remain crowded, raise **Scaling ratio** gradually from `10` to `20`, then to `30` if necessary, running and stopping the layout after each change. Keep the generated gravity settings unchanged at first. If the graph remains compressed, clear **Strong gravity mode** or reduce **Gravity** slightly, changing only one setting at a time. A smaller uniform node size and temporarily hidden labels can make the edges easier to see. Do not use **Slow down** as a spacing control.

## Arrows are difficult to see

Increase edge visibility or zoom into a small selection. Direction can be visually difficult in a dense graph. Confirm it in the edge table rather than relying only on arrowheads.

## Betweenness values differ after filtering

Determine whether Gephi Lite calculated the metric before or after the filter changed the active graph. Record the sequence of operations. A metric calculated on a filtered subgraph answers a different question from the same metric calculated on the complete graph.

## The saved workspace will not reopen normally

Gephi Lite workspace files are versioned. Open it in the same current version when possible. Preserve the GEXF, node table, edge table, notebook, and images so the analysis does not depend on one application-state file.

---

# Further reading and reference

- [Neo4j: What is a graph database?](https://neo4j.com/docs/getting-started/graph-database/) — a short introduction to nodes, relationships, and properties
- [Gephi Lite](https://lite.gephi.org/)
- [Official Gephi Lite quickstart](https://gephi.org/quickstart/)
- [Gephi Lite documentation](https://docs.gephi.org/lite/)
- [Gephi Lite file formats](https://docs.gephi.org/lite/user-manual/file-formats/)
- [NetworkX documentation](https://networkx.org/documentation/stable/)
- [pandas documentation](https://pandas.pydata.org/docs/)
