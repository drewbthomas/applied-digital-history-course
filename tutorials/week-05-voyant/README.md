# Week 5: Text Analysis and Distant Reading with Voyant Tools

## Overview

This tutorial introduces **text analysis**, **natural language processing (NLP)**, and **distant reading** through a corpus of 57 United States presidential inaugural addresses, from George Washington's first address in 1789 through Barack Obama's second address in 2013. You will use [Voyant Tools](https://voyant-tools.org/), a browser-based environment for exploring collections of texts.

The purpose is not to let software "read" speeches for you. You will learn how a computer transforms language into countable units, explore patterns across several documents, return to passages in their original context, and evaluate what those patterns can—and cannot—demonstrate historically.

The recurring workflow is:

> historical question → selected corpus → text processing → pattern → contextual reading → historical interpretation

Every arrow represents a decision. A result is shaped not only by the speeches but also by which speeches were selected, how the files were prepared, what Voyant counts as a word, which words are excluded, and how the resulting visualization is interpreted.

## Learning objectives

By the end of this tutorial, you should be able to:

1. Explain in plain language what text analysis and NLP are.
2. Identify several historical questions for which NLP may be useful.
3. Explain the difference between close reading and distant reading.
4. Describe the supplied corpus and explain why even a complete series within stated boundaries is not a transparent record of American political language.
5. Explain tokenization, word frequency, relative frequency, stopwords, and concordances.
6. Create and explore a multi-document corpus in Voyant Tools.
7. Compare the distribution of selected terms across documents.
8. Use a concordance to evaluate how a term is used in context.
9. Test how a change to the stopword list changes an analytical result.
10. Distinguish textual evidence, historical inference, interpretation, and speculation.

## Materials

You will need:

- the `inaugural_speeches` folder or `inaugural_speeches.zip` archive supplied with this tutorial;
- a modern web browser;
- access to [Voyant Tools](https://voyant-tools.org/);
- your Week 5 portfolio folder and journal.

The `inaugural_speeches` folder contains 57 numbered `.txt` files. Together they form a chronological series from `1_washington_1789.txt` through `57_obama_2013.txt`. The number at the beginning of each filename records the speech's position in the series; the rest identifies the president and year.

The corpus is complete according to a particular selection rule: it contains the formal inaugural addresses in this series through 2013. That does not make it representative of all presidential rhetoric, all political speech, or American political culture. Corpus completeness and historical representativeness are different questions.

---

# Part 1. What is text analysis?

**Text analysis** is the systematic examination of textual features. A historian might count words, identify recurring phrases, compare vocabulary across documents, examine which words tend to appear near one another, classify documents, or trace changes in language over time.

Some text analysis can be performed manually. For example, a historian can read a speech and record every occurrence of the word `union`. Computational text analysis applies explicit operations to many passages or documents consistently and quickly.

The computer does not encounter a speech as a historian does. It receives a sequence of characters. Before it can count or compare language, the text must be transformed into units that software can process.

A simplified pipeline looks like this:

```text
speech as a historical source
        ↓
digital transcription
        ↓
document boundaries and metadata
        ↓
tokens (countable units)
        ↓
counts or other measurements
        ↓
tables and visualizations
        ↓
historical interpretation
```

Each stage can alter the evidence. Transcription errors can change words. Document boundaries determine what is compared. Tokenization determines what is counted. A stopword list determines what is hidden from some results. A visualization highlights certain relationships while obscuring others.

## What is NLP?

**Natural language processing** is a broad field concerned with using computers to process, analyze, or generate human language. NLP includes relatively simple operations and much more complex models.

Examples include:

- dividing text into words or sentences;
- counting words and phrases;
- finding passages containing a search term;
- identifying words that occur near one another;
- reducing grammatical variants to a common base form;
- identifying names of people, places, or organizations;
- classifying documents by topic or genre;
- estimating sentiment;
- translating, summarizing, or generating language.

Large language models are one kind of NLP system, but **NLP is not synonymous with LLMs**. The frequency tables and concordances used in this tutorial are computational language processing even though they do not require a generative model.

## Why do historians use NLP?

Historians use NLP when the amount, repetition, or internal structure of textual material makes systematic manual comparison difficult. It can help scholars:

- identify patterns across more documents than one person could read closely at once;
- locate relevant passages within a large collection;
- compare vocabulary across authors, genres, institutions, places, or periods;
- trace the appearance or changing use of terms;
- investigate recurring phrases and formulaic language;
- generate questions and select passages for closer examination;
- test whether an impression formed through close reading appears elsewhere in a corpus;
- identify exceptions that complicate an apparent pattern.

NLP is most useful when the computational operation serves a historical question. Producing a word cloud is not by itself historical analysis.

## What NLP cannot establish by itself

A textual pattern does not automatically explain its historical cause or significance. Word counts alone cannot determine:

- what an author meant by a word in a particular passage;
- whether two authors used the same term in the same sense;
- whether a speech accurately represented public opinion;
- whether a concept was absent because a particular word was absent;
- why rhetoric changed;
- whether the selected corpus is representative of a broader population of texts;
- how an audience understood or responded to a speech.

NLP produces evidence about the processed texts. Moving from that evidence to an account of political culture, ideology, or historical change requires contextual knowledge and argument.

> **Checkpoint 1:** In your own words, explain the difference between NLP and an LLM. Give one historical question for which NLP could help, and identify one part of that question that a word count could not answer.

---

# Part 2. Close reading, distant reading, and scalable reading

**Close reading** examines a passage carefully: its vocabulary, rhetoric, structure, ambiguities, historical context, and relationship to other evidence.

**Distant reading** examines patterns across a collection by representing texts through selected features such as word frequencies or distributions. The name emphasizes analytical distance: the scholar temporarily stops treating every document as an indivisible work and instead compares formalized features across many texts.

These approaches are not opposites. A productive workflow moves between scales:

1. inspect the corpus as a whole;
2. identify a potentially meaningful pattern;
3. locate the documents and passages producing it;
4. read those passages closely;
5. revise or reject the initial interpretation;
6. test the revised interpretation elsewhere in the corpus.

This movement is sometimes described as **scalable reading**. Voyant supports it by linking corpus-level visualizations to documents and passages.

Consider the term `people`. A corpus-level graph might show that it appears more frequently in one address than another. Close reading is still needed to determine:

- who is included in "the people";
- whether the term refers to political authority, national unity, or a social group;
- whether the speaker is describing the people or addressing them;
- what exclusions are concealed by apparently universal language.

> **Checkpoint 2:** Why would a historian need to return to individual passages after identifying a frequency pattern?

---

# Part 3. Understand the corpus before analyzing it

A **corpus** is a collection of texts assembled for analysis. A corpus is made, not found. Its boundaries reflect decisions about inclusion, exclusion, transcription, metadata, and document structure.

## The unit of analysis

In this tutorial:

- the **corpus** is the complete supplied collection;
- each **document** is one inaugural address;
- the principal unit being compared is therefore one speech;
- the filename supplies basic document metadata: president and year.

This matters. A result comparing documents is not automatically a comparison of presidents, administrations, political parties, or historical eras. One speech cannot represent everything a president said or everything Americans thought at the time.

## Why use inaugural addresses?

Inaugural addresses offer several advantages for teaching text analysis:

- they belong to a recognizable political genre;
- they are attributed and dated;
- they cover a long chronological period;
- they address recurring subjects such as government, nation, conflict, citizenship, and political obligation;
- their relatively clean transcriptions make initial text analysis manageable.

The apparent consistency of the corpus can also mislead. The speeches differ in length, immediate political circumstances, audience, rhetorical purpose, authorship practices, media environment, and conventions of political language.

## Selection and representativeness

The supplied texts constitute a complete chronological series within their stated boundaries rather than a statistical sample. They are nevertheless a highly selective body of evidence. The corpus contains one ceremonial genre, produced for exceptional occasions by presidents and their collaborators. It excludes campaign speeches, ordinary policy statements, congressional debate, journalism, private correspondence, opposition rhetoric, audience responses, and most political speech produced by people who were not presidents.

This distinction matters:

- **Completeness:** all 57 formal addresses in the defined series from 1789 through 2013 are present.
- **Representativeness:** the corpus does not represent all presidential rhetoric or American political language.
- **Comparability:** the documents share a genre, but the genre, audience, authorship practices, media environment, and political circumstances changed across more than two centuries.

The corpus also sits at the end of a longer evidentiary chain:

```text
historical political activity
        ↓
speech composition and delivery
        ↓
contemporary recording and publication
        ↓
preservation and editorial transmission
        ↓
digital transcription
        ↓
selection for this teaching corpus
        ↓
Voyant's processing
        ↓
your analysis
```

At each stage, information may be selected, changed, or lost.

## Inspect the source files

Before opening Voyant:

1. Open `1_washington_1789.txt`, `20_lincoln_1865.txt`, and `57_obama_2013.txt` in a text editor.
2. Identify what appears at the beginning and end of each file.
3. Notice that each file is stored on one long line with three fields separated by tabs: president, date, and speech text.
4. Compare the date formats. Earlier files generally use a form such as `1789-04-30`, while later files include forms such as `1/20/2009`.
5. Search the files for remnants of web formatting, including `<i>`, `</i>`, and character entities such as `&#8212;`.
6. Check whether the files appear to contain one complete speech.
7. Compare the filenames. Determine how Voyant will know which document is which.
8. Record any inconsistency that could affect counting.

Do not clean or edit the files during this inspection. The names and dates at the start of each file may become part of Voyant's token counts, and residual HTML may be processed in unexpected ways. These are features of the supplied digital corpus and should be documented before anyone decides whether or how to remove them. First distinguish the source as supplied from any later transformation.

> **Checkpoint 3:** Confirm that the folder contains 57 documents covering 1789–2013. Record what one document represents and one reason the corpus should not be treated as a transparent record of American political language.

---

# Part 4. How Voyant transforms text

Voyant Tools is a web-based environment for reading and analyzing digital texts. Its default interface combines several connected tools, allowing you to move from corpus-level patterns to documents and passages.

Before interpreting its output, understand four basic operations.

## Tokenization

A **token** is a unit produced when software divides text into processable pieces. Tokens are often words, although punctuation and other character sequences may also require rules.

For example:

```text
We are, and always will be, a nation.
```

might be processed into word tokens resembling:

```text
we | are | and | always | will | be | a | nation
```

Tokenization appears mechanical, but it involves decisions. Consider:

- `self-government`;
- `United States`;
- `citizen's` and `citizens`;
- abbreviations;
- numbers and dates;
- spelling variants;
- transcription errors.

A computer does not know automatically which distinctions matter historically.

## Frequency

**Raw frequency** is the number of times a token occurs. If `nation` appears 10 times, its raw frequency is 10.

Raw counts are useful within a document but can be misleading when comparing documents of different lengths. A longer address has more opportunities to contain any word.

**Relative frequency** expresses a count in relation to document length. Voyant can use relative frequency when comparing term distributions. This makes differently sized documents more comparable, although normalization does not remove differences in genre, context, or rhetorical structure.

## Stopwords

**Stopwords** are words excluded from some calculations or displays, often because they occur frequently and are assumed to contribute little to a particular question. Common English examples include `the`, `and`, `of`, and `to`.

A stopword list is an analytical filter, not a list of meaningless words. Function words can be historically or stylistically significant. Removing them changes the representation of the corpus. A domain-specific word such as `government` might be so common in inaugural addresses that it dominates a word cloud, but excluding it would conceal precisely that feature.

## Concordance

A **concordance**, often called **keywords in context** (KWIC), displays each occurrence of a search term with words immediately before and after it. Concordances reconnect counts to usage. They help determine whether repeated occurrences have similar meanings and allow unusual or misleading cases to become visible.

Voyant's **Contexts** tool is a concordance. It will be central to the tutorial.

> **Checkpoint 4:** Explain why relative frequency may be preferable to raw frequency when comparing two speeches.

---

# Part 5. Create the Voyant corpus

## Step 1. Open Voyant

Open [Voyant Tools](https://voyant-tools.org/).

Voyant does not require an account for this tutorial. The precise appearance of the interface may change, but the principal tools and operations should remain recognizable.

## Step 2. Upload the texts

1. On the opening screen, choose the option to upload files.
2. Upload `inaugural_speeches.zip`. Voyant will treat the 57 text files inside it as separate documents.
3. If the archive does not work, open the supplied `inaugural_speeches` folder and select all 57 `.txt` files at once. Use `Ctrl+A`, or use `Ctrl` or `Shift` if your file picker requires it.
4. Confirm that every selected file is listed.
5. Select **Reveal**.
6. Wait for Voyant to create the corpus.

The supplied archive is only a convenient package of the same texts. Keep the uncompressed originals available for inspection and do not edit or delete them.

## Step 3. Confirm the corpus

Find the **Summary** panel and record:

- the number of documents, which should be **57**;
- the total number of words;
- the vocabulary density or other reported corpus statistics;
- the longest and shortest documents;
- the most frequent words shown.

Confirm that the document count matches the number of uploaded files. If it does not, stop and recreate the corpus before continuing.

## Step 4. Recognize the default tools

The default Voyant view normally contains five connected panels:

| Tool | What it displays | Question to ask |
|---|---|---|
| **Cirrus** | A word cloud of frequent terms | Which words are visually emphasized, and why? |
| **Reader** | The text of a selected document | What does the passage actually say? |
| **Trends** | Distribution of selected terms | In which documents or segments is a term frequent? |
| **Summary** | Corpus and document statistics | What is in the corpus, and how large are its documents? |
| **Contexts** | Occurrences of a term with surrounding words | How is the term used in particular passages? |

The panels interact. Selecting a word or document in one panel can update the others. Treat this as a way to move between analytical scales, not merely as an interface feature.

> **Checkpoint 5:** Voyant reports 57 documents. Record its total word count and confirm that `2_washington_1793.txt` is the shortest document and `14_harrison_1841.txt` is the longest. If Voyant reports something different, inspect the corpus before continuing.

---

# Part 6. Begin with description, not interpretation

## Step 1. Examine Cirrus

Look at the **Cirrus** word cloud.

Record the five largest words. Then answer:

1. What does word size represent?
2. Which common words appear to have been removed?

A word cloud represents frequency through text size. It is useful for rapid orientation but weak for precise comparison. Area and font size are difficult to compare accurately, and the arrangement can suggest relationships that the underlying counts do not establish.

## Step 2. Open Corpus Terms

Switch the Cirrus panel to **Corpus Terms** using its tab or tool menu. This provides a table rather than a cloud.

1. Record the raw counts of five frequent terms.
2. Identify one word whose prominence might reflect the genre of inaugural addresses.
3. Identify one word that could support a historical question.

The table is better than the cloud for verifying exact counts. Neither explains why a word is frequent.

## Step 3. Separate observation from explanation

Write two sentences using this structure:

> **Evidence:** In the processed corpus, the token `_____` occurs _____ times and ranks among the _____ most frequent displayed terms.

> **Possible interpretation:** This pattern might relate to _____, but establishing that interpretation would require examining _____ and comparing it with _____.

Do not write that a word was "important" merely because it was frequent. Importance is an interpretive claim that requires an argument.

---

# Part 7. Test the stopword list

This exercise demonstrates that an apparently simple visualization depends on a hidden analytical decision.

## Step 1. Inspect the default setting

1. In **Cirrus** or **Corpus Terms**, open the tool options (often represented by a slider or settings icon).
2. Find the stopword setting.
3. Record which list or automatic setting is active.

## Step 2. Remove the stopword filter

Temporarily select no stopword list or clear the stopword setting.

Record:

- the five most prominent words now;
- how the word cloud changes;
- which historically meaningful terms become less visible.

The texts have not changed. Their computational representation has.

## Step 3. Restore the English stopword list

Restore the default English or automatically detected stopword list.

## Step 4. Add one custom stopword

Choose one frequent corpus-specific word and add it to a custom stopword list. Do not choose the word arbitrarily: write down why someone might exclude it and why someone else might retain it.

For example, excluding `government` might allow less frequent vocabulary to become visible, but it would also hide evidence that governmental language structures the genre.

Record the top terms before and after the change.

## Interpretation

Answer:

1. Which output is the "correct" word cloud?
2. What analytical purpose might justify each version?
3. Why should a historian document the stopword list used?

The aim is not to discover a universally correct stopword list. The aim is to make the filtering decision explicit and justify it in relation to a question.

> **Checkpoint 6:** Preserve notes showing how at least one result changed when the stopword setting changed.

---

# Part 8. Compare selected concepts across speeches

## Historical question

For this exercise, investigate:

> How do selected inaugural addresses use vocabulary concerning political community, government, and national identity?

This is an exploratory question. The presence of a word is not identical to the presence of a concept. `Union`, `nation`, `country`, `republic`, and `people` may overlap, but they are not interchangeable.

## Step 1. Select terms

Begin with three terms:

```text
people
government
nation
```

You may later replace one with another term that emerges from your exploration.

## Step 2. Use Trends

1. Find the **Trends** panel.
2. Enter the three terms into its search box, pressing Enter as required.
3. Confirm that the horizontal axis displays documents rather than equal chronological intervals.
4. Turn on labels if they make the documents easier to identify.
5. Hover over points or bars and record the values for at least three speeches.

## Step 3. Compare raw and relative frequencies

Open the Trends options and locate the frequency mode.

1. View the terms using **raw frequency**.
2. Record which speeches appear most prominent.
3. Switch to **relative frequency**.
4. Record which comparisons change.
5. Use Summary to check whether document length helps explain the difference.

Do not interpret the connecting line as evidence of continuous historical development. The documents are discrete speeches rather than observations of language between inaugurations. Moreover, the corpus has four-year intervals between most successive addresses but includes interruptions and one-day shifts in inauguration dates. The line connects values visually; it does not demonstrate that change between speeches was gradual or continuous.

## Step 4. Form a provisional observation

Write one statement limited to what the graph shows. For example:

> In this corpus, the relative frequency of `_____` is higher in the _____ address than in the _____ address.

Then write two possible explanations. At least one should concern the immediate context of the speeches rather than a long-term historical trend.

## Step 5. Identify missing evidence

What additional texts would be needed to determine whether this is (choose two):

- a difference between individual speeches;
- a difference between presidents;
- a response to a particular political crisis;
- a broader chronological change;
- a feature of changing inaugural rhetoric?

> **Checkpoint 7:** You have compared raw and relative frequency, recorded a change or stability, and identified evidence needed to explain the pattern.

---

# Part 9. Return to context with a concordance

Frequency tells you how often a token occurs. It does not tell you whether every occurrence is being used in the same way. In this exercise, you are not expected to know the historical background of every address. The purpose is to inspect what lies behind a count and to identify questions that would require further historical research.

## Step 1. Open Contexts

Use the **Contexts** panel. Search for one term from the Trends exercise.

The table should show:

- the document containing each occurrence;
- words immediately to the left;
- the search term;
- words immediately to the right.

## Step 2. Examine occurrences

Read at least three occurrences distributed across different speeches. Expand the context (click the + sign on the left) or use Reader when the short window is insufficient.

For each occurrence, record only what you can observe directly in the passage:

- the document in which it appears;
- a short quotation containing the term;
- who or what the term appears to refer to in that sentence;
- any nearby words that help define or qualify it;
- whether the passage is making a positive statement, a negative statement, a contrast, or a question.

Use cautious language such as “in this sentence, `people` appears to refer to…” You are describing the passage, not claiming to have reconstructed the president's political philosophy.

> **Checkpoint 8:** You have examined at least three occurrences of one term in different speeches and recorded what the term appears to refer to, the nearby words that qualify it, and how it functions in each passage.

---

# Part 10. Conduct a small investigation

Formulate one question that can be investigated with the supplied corpus. It must involve comparison and contextual reading, not merely finding the most frequent word.

If you cannot think of a question, use one of these:

- How do selected speeches describe the relationship between `people` and `government`?
- How does the use of `union` vary across the selected addresses?
- In what contexts do speakers use `freedom` and `liberty`?
- How do references to conflict—such as `war`, `peace`, or `enemy`—vary across speeches?
- Which terms distinguish one speech from the rest of the corpus, and can immediate context explain them?

To complete the investigation:

1. Enter your chosen word or words in **Trends** and compare their relative frequencies across the speeches.
2. Use **Contexts** to read at least three occurrences in different speeches.
3. Write one short paragraph stating your question, what pattern you found, what the passages show, and one limitation of the result.

---

# Part 11. Written reflection

Respond to the following question in your journal:

> How could you use text analysis with Voyant in a research project? Briefly describe the corpus you might use, the question you would investigate, and one limitation you would need to consider.

---

# Part 12. Optional extension: Explore your own corpus

Create a new Voyant corpus using one or more text documents of your choice. You may use material related to your research interests, another course, or a set of public texts found online.

1. Upload one or more documents and confirm what each document represents.
2. Use **Cirrus** or **Corpus Terms** to identify frequent words.
3. Use **Trends** to investigate one or more terms. If you upload only one document, Trends will show their distribution across segments of that document rather than across several documents.
4. Use **Contexts** to inspect at least one term in its surrounding passages.
5. Note a few findings in your journal. Include what the corpus contains, one pattern you observed, and one limitation or question raised by the analysis.

---

# Part 13. Further tools for text analysis

Voyant is useful for introducing corpus exploration because it works in a browser and makes it easy to move between frequency patterns and passages. Other tools offer more control or more advanced forms of linguistic analysis.

## Sketch Engine

[Sketch Engine](https://www.sketchengine.eu/) is an online corpus-analysis platform. Like Voyant, it provides word-frequency lists and concordances, but it also supports more advanced searches and linguistically annotated corpora.

Its tools include:

- **Concordance:** searches for words, phrases, lemmas, or grammatical patterns and displays them in context;
- **Word Sketch:** summarizes the words and grammatical relationships that typically occur with a selected word;
- **Word Sketch Difference:** compares how two words are used;
- **keywords and terms:** identifies vocabulary that is unusually prominent in one corpus compared with another;
- **n-grams and collocations:** identifies recurring sequences and words that occur together more often than expected;
- **corpus building:** allows users to upload documents and create their own corpora.

Sketch Engine provides access to many prepared corpora in numerous languages and offers trial access, but continued access may require an individual or institutional subscription. Its advanced features also require more understanding of concepts such as lemmas, parts of speech, collocation, and reference corpora.

For historical research, the same cautions still apply. Automatic lemmatization and grammatical tagging may perform less reliably on historical spelling, OCR errors, unfamiliar genres, or language that differs from the material used to build the tool's models.

Further information:

- [Sketch Engine user guide](https://www.sketchengine.eu/guide/)
- [Sketch Engine concordance guide](https://www.sketchengine.eu/guide/concordance-a-tool-to-search-a-corpus/)

## Python and spaCy

[spaCy](https://spacy.io/) is a Python library for natural language processing. It can be used in Google Colab to:

- tokenize documents;
- divide text into sentences;
- assign parts of speech;
- reduce some grammatical forms to lemmas;
- identify possible people, places, organizations, and other named entities;
- create structured data that can be counted, compared, or exported to a CSV.

Using Python makes the processing pipeline more inspectable and reproducible than working only through a graphical interface. It also allows a historian to preserve filenames, customize rules, record intermediate results, and combine text analysis with tables or visualizations.

You do not need to learn spaCy for this tutorial. If you wanted to continue the analysis using last week's LLM-assisted Python workflow, you could give an LLM a prompt such as:

```text
I am a digital history student working in Google Colab. Write beginner-friendly Python code using spaCy to upload several English TXT files and preserve each filename.

For each document, report the total number of tokens and the most frequent lemmas after excluding punctuation, spaces, and stopwords. Also create a table of the people, places, and organizations identified by spaCy. Do not overwrite or modify the original files. Make all excluded categories explicit, print validation checks, and explain each step in plain language.

After the code, explain why a modern English spaCy model may make errors when applied to historical texts and why named entities must be checked against the original passages.
```

The LLM can help draft and explain the code, but it cannot verify that spaCy's linguistic classifications are historically correct. Any lemmas, grammatical labels, or named entities should be treated as model outputs requiring validation.

Further information:

- [spaCy 101](https://spacy.io/usage/spacy-101)
- [spaCy linguistic features](https://spacy.io/usage/linguistic-features)

---

# Troubleshooting

## Voyant reports the wrong number of documents

Return to the opening screen and upload the files again. Confirm that every `.txt` file is selected. If you uploaded a `.zip` file, confirm that it contains only the intended texts and not hidden copies or unrelated files.

## A filename is unclear or truncated

Open the Documents tool or inspect the file locally. Do not guess which speech a result represents.

## Common words dominate the display

Check the language and stopword settings. If the English stopword list was not applied, select it. Record the change as a transformation rather than silently proceeding.

## A term has no results

Check spelling and singular/plural forms. Consider whether the concept might be expressed through other words. A zero result refers to the searched token, not necessarily to the historical concept.

## Trends shows parts of one speech rather than all documents

You may have drilled down to document mode. Reset the tool or return to corpus mode. In document mode, Voyant divides a speech into segments; these are computational divisions, not necessarily rhetorical or historical sections.

## The graph seems to show a smooth change through time

Check the horizontal axis. The selected documents are categories and may be separated by unequal intervals. A line between them does not supply evidence about the missing years or speeches.

## The interface differs from the instructions

Voyant's interface may change. Look for the named tool and its options menu rather than relying only on the location or appearance of a button. Consult the official documentation linked below.

---

# Further reference

- [Voyant Tools](https://voyant-tools.org/)
- [Voyant Tools: Getting Started](https://voyant-tools.org/docs/tutorial-start.html)
- [Voyant Tools: Tutorial and Workshop](https://voyant-tools.org/docs/tutorial-tutorial.html)
- [Voyant Tools: Trends](https://docs.voyant-tools.org/docs/tutorial-trends.html)
- [Voyant Tools: Contexts](https://docs.voyant-tools.org/docs/tutorial-contexts.html)
- [Voyant Tools: List of Tools](https://voyant-tools.org/docs/tutorial-tools_.html)

Remember the larger evidentiary chain:

> historical activity → contemporary language and recording → preservation → editorial transmission → digitization → corpus selection → computational transformation → visualization → interpretation

Voyant helps you explore patterns in processed texts and move between scales of reading. It does not determine which questions matter, whether a corpus is representative, what a term meant historically, or why a pattern occurred.
