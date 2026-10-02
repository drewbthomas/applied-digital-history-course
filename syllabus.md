# Applied Digital History: Methods and Projects

**Course number:** 603.613  
**Course type:** Project-based colloquium  
**Language of instruction:** English

## Course Description

This course provides practical experience in applying digital methods to historical research. Students will work with historical datasets and digital source collections to learn how data is acquired, prepared, analyzed, interpreted, and presented.

The course is organized around the sequence:

> **Materials → Processing → Presentation**

Each stage involves historical and methodological decisions. Students will therefore learn not only how to use digital tools, but also how to evaluate the assumptions, uncertainties, and distortions introduced by a digital research workflow.

Each week introduces a different tool or practical skill. Tutorials will normally be completed independently before class so that students can work at their own pace. Class meetings will focus on discussing difficulties, comparing results, examining published digital history projects, troubleshooting common problems, and developing individual projects.

No previous programming experience is required. Students will use browser-based tools wherever possible. A prepared Google Colab environment will be used for limited Python-based data transformations, so students will not need to install Python on their own computers.

## Course Objectives

By the end of the course, students will be able to:

1. Formulate a historically meaningful question that can be investigated with digital methods.
2. Evaluate the provenance, structure, representativeness, and limitations of a historical dataset.
3. Organize tabular data and prepare it for computational analysis.
4. Use OpenRefine to identify inconsistencies, normalize values, and document data-cleaning decisions.
5. Create and critically interpret data visualizations with RAWGraphs.
6. Conduct basic text analysis with Voyant Tools and validate quantitative patterns through close reading.
7. Normalize and geocode historical place names while documenting uncertain and rejected matches.
8. Construct and interpret a historical network with Gephi Lite.
9. Train and evaluate a simple image-classification model with Teachable Machine.
10. Use a large language model to help generate a limited Python script, run it in Google Colab, and validate its output.
11. Document a digital research workflow in a structured GitHub repository.
12. Communicate the results and limitations of a digital history project through an academic poster and short written report.

## Course Organization

### Weekly Tutorials

Before most class meetings, students will complete a guided exercise using a supplied historical dataset. Each exercise will produce one or more files that must be uploaded to the appropriate weekly folder in the student's GitHub repository.

The exercises are intended to encourage experimentation and problem-solving. A carefully documented unsuccessful attempt may be more valuable than an apparently successful output that the student cannot explain.

### Class Meetings

Class meetings will normally include:

- Discussion of problems encountered during the tutorial.
- Comparison and interpretation of student results.
- Examination of historical research projects using the week's method.
- A short demonstration or troubleshooting exercise where necessary.
- Time to consider how the method might contribute to the final project.

### GitHub Portfolio and Journal

Each student will create a GitHub repository from the course template. GitHub will be used as a web-based course portfolio; no command-line Git knowledge is required. Students will upload files and edit short Markdown documents through the browser.

For each weekly exercise, students will preserve the supplied material, upload their results, and complete a brief journal entry addressing:

1. What material did I work with?
2. What transformation did the tool perform?
3. Which decisions did I make?
4. What failed, remained unclear, or produced an unexpected result?
5. What does the output allow me to infer historically?
6. What does the output not allow me to infer?
7. Could this method contribute to my final project?

## Weekly Schedule

| Week | Topic | Principal tool or skill | Project development |
|---|---|---|---|
| 1 | Introduction to Applied Digital History | GitHub setup; course workflow; demonstration of one dataset analyzed in several ways | Explore the course dataset catalogue |
| 2 | From Tables to Visual Arguments | CSV structure, rows and columns, tidy data, and RAWGraphs | Identify two potentially interesting datasets |
| 3 | Cleaning and Normalizing Historical Data | OpenRefine: facets, clustering, transformations, original and normalized values | Evaluate the quality and limitations of the candidate datasets |
| 4 | Automating Data Transformations with AI | Prompting an LLM to generate a Python script; running it in Google Colab; parsing, reshaping, and validating data | Submit two possible question–dataset–method combinations |
| 5 | Text Analysis and Distant Reading | Corpus construction, OCR quality, stopwords, frequency, comparison, and Voyant Tools | Test a possible text-based question or dataset |
| 6 | Geocoding and Spatial History | Place-name normalization, automated coordinate retrieval, ambiguous matches, and mapping | Select a provisional project dataset and method |
| 7 | Historical Network Analysis | Creating node and edge tables; layouts, centrality, filtering, and Gephi Lite | Submit a preliminary historical research question |
| 8 | Images as Data | Training examples, classification, testing, bias, and Teachable Machine | Confirm the project dataset, question, and primary method |
| 9 | Designing a Digital History Project | Data biography, workflow design, validation, documentation, and project clinic | Submit the project plan and workflow diagram |
| 10 | Project and Poster Workshop | Troubleshooting, interpreting results, poster design, and peer review | Present a preliminary result and draft poster |
| 11 | Poster Symposium | Presentation and discussion of final projects | Submit the poster, repository, and short report |

The schedule may be adjusted in response to the needs and progress of the class.

## Final Project

Each student will complete an independent digital history project using either a dataset from the course catalogue or an approved dataset of their own. The project must include:

- A historically meaningful research question.
- A clearly identified body of source material or data.
- A discussion of provenance, coverage, and significant absences.
- At least one substantial digital method introduced in the course.
- Documentation of the processing workflow and important decisions.
- Validation of computational results against the underlying data or sources.
- A historically grounded interpretation of the results.
- Explicit discussion of uncertainty and limitations.

The project will be presented through an academic poster. Students will also submit their organized GitHub repository and a short written report explaining the historical question, materials, method, principal findings, validation, and limitations.

Students are not expected to use every tool introduced in the course. A carefully designed project using one method is preferable to a superficial project combining several methods.

## Assessment

| Component | Weight | Basis of assessment |
|---|---:|---|
| Weekly practical exercises and journal | 30% | Completion, organization, documentation, reflection, and evidence of troubleshooting |
| Preparation and participation | 15% | Engagement in discussions, comparison of results, peer feedback, and project workshops |
| Project plan and data biography | 10% | Historical question, source criticism, feasibility, workflow design, and proposed validation |
| Final GitHub repository and short report | 20% | Organization, reproducibility, methodological explanation, validation, interpretation, and limitations |
| Academic poster and presentation | 25% | Clarity of argument, appropriate use of evidence and visualization, historical interpretation, and response to questions |
| **Total** | **100%** | |

Assessment prioritizes substantive historical and methodological reasoning over technical sophistication. Students will receive credit for identifying genuine evidentiary or computational problems, explaining why they matter, and documenting reasonable attempts to address them.

## Use of Artificial Intelligence

Generative AI may be used when explicitly permitted for an exercise or project. Students remain responsible for all submitted material and must document consequential uses of AI, including prompts used to generate or revise data-processing scripts.

AI-generated code and structured data must be treated as unverified output. Students must inspect the results, preserve the original material, report failed or unmatched records, and explain how they evaluated the accuracy of the transformation. AI output must not be presented as historical evidence without independent verification.
