# FASTA Sequence Analyser

## Research Report I

---

**Supervisor**
[Supervisor Name]

**Submitted by**
[Student Name]
[AG #]

---

BS [Computer Science / IT / Software Engineering]
Department of Computer Science
University of Agriculture, Faisalabad.

---

# Table of Contents

1. INTRODUCTION ............................................................................... 1
   1.1 Background ................................................................................. 1
   1.2 Description ................................................................................. 2
   1.3 Scope .......................................................................................... 3
   1.4 Objectives ................................................................................... 4
2. REQUIREMENTS ............................................................................... 5
   2.1 Functional Requirements ............................................................ 5
   2.2 Non-Functional Requirements .................................................... 15
   2.3 Hardware Requirements ............................................................. 18
   2.4 Software Requirements .............................................................. 19
3. METHODOLOGY ............................................................................... 20
   3.1 Process Model ............................................................................. 20
   3.2 Tools & Technologies .................................................................. 23
4. TIMELINE ............................................................................................. 28

---

# List of Figures

Figure 1: Home Page – Initial View of FASTA Sequence Analyser ....................... 2
Figure 2: Paste Sequence Textarea (Active State) .............................................. 6
Figure 3: File Upload Section with Single File Selected ..................................... 7
Figure 4: Multiple Files Accumulated in the Upload List .................................... 8
Figure 5: Cross Icon for Removing an Uploaded File .......................................... 9
Figure 6: Mutual Disable – Paste Box Disabled When Files Are Uploaded ....... 10
Figure 7: Mutual Disable – File Upload Disabled When Text Is Pasted ............. 10
Figure 8: Multiple Sequence Output Stacked Cards .......................................... 11
Figure 9: GC/AT Bar Chart per Sequence ............................................................ 12
Figure 10: Fetch Relative Sequence Button on Result Card .............................. 13
Figure 11: BLAST Modal – Submitting Stage ...................................................... 13
Figure 12: BLAST Modal – Pending Status with Countdown ............................. 14
Figure 13: BLAST Modal – Ready Status ............................................................. 14
Figure 14: BLAST Modal – Final Results with Hits ............................................. 15
Figure 15: Download CSV / JSON Buttons in Results Modal ............................. 16
Figure 16: Navbar – Initial Full Width Layout ..................................................... 17
Figure 17: Navbar – Scrolled Compact Pill Shape .............................................. 17
Figure 18: About Page – Animated Gradient Background ................................ 21
Figure 19: Contact Us Page Layout ..................................................................... 22
Figure 20: RAD Activities ..................................................................................... 22
Figure 21: Tentative Timeline of the Project Activities ...................................... 28

---

# List of Tables

Table 1: Functional Requirements Summary ......................................................... 5
Table 2: Non-Functional Requirements Summary ............................................... 15
Table 3: Hardware Requirements ......................................................................... 18
Table 4: Software Requirements .......................................................................... 19
Table 5: Tools & Technologies Used in the Project ............................................. 23
Table 6: NCBI BLASTN API Endpoints Used ........................................................ 26
Table 7: Project Module Breakdown .................................................................... 28

---

# 1. INTRODUCTION

## 1.1 Background

Deoxyribonucleic acid, commonly known as DNA, is the fundamental molecule that carries the genetic information of every known living organism. Within the field of bioinformatics, the analysis of DNA sequences plays a vital role in understanding biological functions, diagnosing genetic disorders, identifying species, and conducting evolutionary research. The most widely accepted text-based format for representing nucleotide and protein sequences is the FASTA format, which begins with a single-line description prefixed by a greater-than symbol followed by lines of sequence data. Despite the widespread use of this format, students, researchers, and laboratory technicians often face challenges in quickly extracting meaningful information from raw FASTA files. Tools that exist online are either too complex for casual use, require local installation of heavy bioinformatics suites such as BLAST+, EMBOSS or Biopython, or do not provide an integrated visualization of the results.

In academic environments such as university laboratories, students of biotechnology, molecular biology, and computer science frequently work with DNA sequence files for class assignments, semester projects, and undergraduate research. The repetitive task of opening sequences in external tools, calculating GC and AT content manually, identifying the source organism from the header line, and submitting the sequence to public databases for homology searches is both time-consuming and error-prone. A unified, web-based platform that combines validation, statistical analysis, visualization, and remote database querying within a single interface would significantly streamline this workflow and reduce the entry barrier for users who are new to bioinformatics.

The FASTA Sequence Analyser project addresses this gap by providing a modern, browser-based application that accepts one or more DNA sequences in FASTA format, performs essential composition analyses, displays the results visually using interactive charts, and integrates directly with the National Center for Biotechnology Information (NCBI) BLASTN service to fetch related sequences from public databases. The application has been built using the latest Next.js framework with TypeScript, Tailwind CSS, and Chart.js, ensuring high performance, type safety, and a responsive user experience. By proxying external BLAST requests through internal API routes, the application also resolves the Cross-Origin Resource Sharing (CORS) restrictions that typically prevent client-side browsers from interacting directly with NCBI servers.

The need for such a tool was further reinforced by the observation that most existing FASTA analysis utilities are command-line driven, lack a graphical user interface, or are restricted to a single sequence at a time. The proposed system extends usability by supporting batch analysis of multiple sequence files, presenting each result in its own dedicated stacked card with a per-sequence chart, and offering downloadable BLAST results in both CSV and JSON formats. The successful implementation of this project will provide a complete, end-to-end bioinformatics workflow accessible to any user with a standard web browser and an internet connection.

## 1.2 Description

The FASTA Sequence Analyser is a web-based bioinformatics tool that enables users to analyze DNA sequences provided in the standard FASTA format. The application accepts input through two complementary methods: a paste-text area where users can directly enter or paste a single or multiple FASTA sequences, and a file-upload mechanism that supports the accumulation of multiple files added incrementally. To prevent ambiguity and conflicting inputs, the application enforces a mutual-exclusion rule: when text is present in the paste area, the file upload control is automatically disabled, and conversely, when one or more files are queued for analysis, the paste area becomes read-only. Each uploaded file is rendered as a discrete chip with the file name and a cross icon, allowing the user to remove individual files at will.

Upon clicking the analyze button, the application iterates through every input sequence — whether pasted or read from the uploaded files — and performs the following operations: it validates that the input begins with a proper FASTA header, splits multi-FASTA inputs at each greater-than delimiter, calculates the total length of the nucleotide sequence, computes the guanine-cytosine (GC) content and adenine-thymine (AT) content as percentages, and attempts to identify the source organism by parsing the header line for organism markers such as bracketed scientific names or common species keywords. Each analyzed sequence is then displayed in its own dedicated stacked output card that contains the sequence label, the four computed statistics, and a per-sequence GC/AT bar chart powered by Chart.js.

Beyond local statistical analysis, the application integrates with the publicly available NCBI BLASTN Common URL API to allow users to fetch sequences that are evolutionarily or structurally related to their input. A Fetch Relative Sequence button appears at the top-right corner of every result card. Clicking this button opens a modal dialog that handles the three-step BLAST workflow: first, the sequence is submitted to NCBI via a server-side proxy and a Request ID (RID) is extracted from the response; second, the application polls the BLAST status endpoint every thirty seconds, displaying a pending state with a countdown timer; third, when the status becomes ready, the user clicks a button to fetch the final results in JSON format. The top ten related sequences are then displayed in the modal, each showing the accession number, scientific organism name, sequence title, bit score, e-value, hit length, alignment length, and percentage identity. The user may then download the entire result set as either a CSV file (for spreadsheet analysis) or a JSON file (for programmatic processing).

The user interface is enhanced by a sticky animated navigation bar that smoothly transitions from a full-width banner to a compact pill-shaped capsule when the user scrolls down. The active route is highlighted with a distinctive blue background, and the navbar slides in from the top of the viewport on page load. The application also includes an About page with an animated gradient background and floating orbs that introduces the project, and a Contact Us page that provides supervisor and developer contact information. All page content uses staggered fade-in and slide-up animations to deliver a polished, modern feel consistent with contemporary single-page applications.

## 1.3 Scope

The scope of the FASTA Sequence Analyser project encompasses the complete design, development, and deployment of a single-page web application capable of accepting FASTA-formatted DNA sequences, performing core compositional analyses, visualizing results, and querying external bioinformatics databases. The project is bounded by the following inclusions and exclusions.

The application shall include input mechanisms for both pasted text and file uploads. The file-upload feature shall support the accumulation of multiple files added across separate upload events, with each file represented as an individually removable item. The application shall validate that all input conforms to the FASTA format and shall reject sequences that contain characters outside the DNA alphabet (A, T, G, C). For valid sequences, the system shall calculate the sequence length, GC content percentage, AT content percentage, and a best-effort organism identification based on header content. Each analyzed sequence shall be rendered as a separate stacked card containing both numeric statistics and a graphical bar chart.

The application shall also include an integration layer with the NCBI BLASTN service that handles the three-step asynchronous workflow of submission, polling, and result retrieval. The integration shall be implemented as server-side proxy routes to bypass the browser's same-origin policy and to centralize the parsing of BLAST responses. The final results shall be presented in a modal dialog and shall be downloadable in CSV and JSON formats. The navigation system shall include a Home page (where the analysis takes place), an About page (containing project information), and a Contact Us page (containing relevant contact details).

The scope explicitly excludes protein sequence analysis, multiple sequence alignment, phylogenetic tree construction, primer design, restriction site analysis, motif discovery, and any analysis on databases other than the NCBI refseq_select_rna dataset queried through BLASTN MEGABLAST mode. The project also excludes user account management, sequence history persistence, server-side storage of uploaded files, and any kind of authentication or authorization mechanism. All analyses are performed within the current browser session, and no data is permanently retained by the application server.

## 1.4 Objectives

The overarching goal of this project is to provide an accessible, integrated, web-based tool for analyzing DNA sequences in the FASTA format. To achieve this goal, the following specific objectives have been defined.

- **Objective 1:** To design and implement a clean and responsive user interface that accepts FASTA sequences via both paste-text and file-upload mechanisms while preventing ambiguous dual input.
- **Objective 2:** To implement a robust parser that correctly handles single and multi-FASTA inputs, identifies header lines, extracts nucleotide sequences, and validates that the sequence conforms to the DNA alphabet.
- **Objective 3:** To compute and display per-sequence statistics including sequence length, GC content percentage, AT content percentage, and best-effort organism detection from the FASTA header.
- **Objective 4:** To visualize the nucleotide composition using interactive bar charts, with each analyzed sequence receiving its own independent chart for direct visual comparison.
- **Objective 5:** To present multiple analyzed sequences in vertically stacked output cards, where each card contains the sequence label, computed statistics, and the corresponding chart.
- **Objective 6:** To integrate with the NCBI BLASTN Common URL API through server-side proxy routes that handle submission, status polling, and result retrieval while bypassing CORS restrictions.
- **Objective 7:** To provide a modal dialog that walks the user through the asynchronous BLAST workflow, including a pending stage with a thirty-second polling interval and a visible countdown.
- **Objective 8:** To parse the JSON2_S response from NCBI and display the top ten related sequences with relevant fields such as accession, organism, bit score, e-value, and percentage identity.
- **Objective 9:** To allow users to download the BLAST result set as either a CSV file or a JSON file using browser-side blob construction without any server round-trip.
- **Objective 10:** To implement a sticky animated navigation bar with three pages (Home, About, Contact Us), where the navbar smoothly transitions from a full-width banner to a compact pill on scroll, and the active route is visually highlighted.
- **Objective 11:** To enhance perceived performance and visual appeal through staggered slide-up and fade-in animations on page content, as well as an animated gradient background with floating orbs on the About page.
- **Objective 12:** To deliver a complete, well-documented, type-safe implementation using Next.js, React, TypeScript, and Tailwind CSS that compiles cleanly and runs without errors on standard modern browsers.

---

# 2. REQUIREMENTS

## 2.1 Functional Requirements

Functional requirements describe the specific behaviors, features, and operations that the FASTA Sequence Analyser must support. They define what the system shall do when given specific inputs or actions from the user, and they form the basis for testing and verification of the implemented application. The following functional requirements have been derived from the project objectives and the agreed scope.

**Table 1:** Functional Requirements Summary

| ID    | Title                                  | Priority |
| ----- | -------------------------------------- | -------- |
| FR01  | Paste FASTA Sequence Input             | High     |
| FR02  | Upload FASTA File(s)                   | High     |
| FR03  | Accumulate Multiple Uploaded Files     | High     |
| FR04  | Remove Uploaded File                   | High     |
| FR05  | Mutual Exclusion of Input Methods      | High     |
| FR06  | Parse Multi-FASTA Input                | High     |
| FR07  | Validate DNA Sequence                  | High     |
| FR08  | Compute Sequence Length                | High     |
| FR09  | Compute GC Content                     | High     |
| FR10  | Compute AT Content                     | High     |
| FR11  | Detect Source Organism                 | Medium   |
| FR12  | Render Stacked Output Cards            | High     |
| FR13  | Render GC/AT Bar Chart per Sequence    | High     |
| FR14  | Submit Sequence to NCBI BLASTN         | High     |
| FR15  | Poll BLAST Status Periodically         | High     |
| FR16  | Display Pending and Ready States       | High     |
| FR17  | Fetch and Display BLAST Hits           | High     |
| FR18  | Download Results as CSV                | Medium   |
| FR19  | Download Results as JSON               | Medium   |
| FR20  | Animated Responsive Navigation Bar     | Medium   |
| FR21  | About Page with Animated Background    | Low      |
| FR22  | Contact Us Page                        | Low      |
| FR23  | Clear All Results                      | Medium   |
| FR24  | Display Error Messages                 | High     |

### FR01: Paste FASTA Sequence Input

The system shall provide a textarea element where the user can directly paste or type one or more FASTA-formatted sequences.

| Sub-ID    | Requirement                                                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR01-01   | The system shall provide a multi-line textarea on the Home page labelled "Paste your FASTA sequence".                                          |
| FR01-02   | The system shall accept any text input including newlines, header lines beginning with `>`, and nucleotide characters.                          |
| FR01-03   | The system shall display a placeholder hint of the expected format (e.g., `>sequence_header` followed by a sample sequence).                    |
| FR01-04   | The system shall preserve the pasted content until the user explicitly clears it or analyzes it.                                                |
| FR01-05   | The system shall allow the textarea to be resized vertically by the user.                                                                       |

### FR02: Upload FASTA File(s)

The system shall provide an HTML file-input control that allows the user to browse and select one or more FASTA files from the local file system.

| Sub-ID    | Requirement                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| FR02-01   | The system shall provide a file input control labelled "OR Upload FASTA file(s)".                                                            |
| FR02-02   | The system shall read the contents of each selected file using the browser's FileReader API.                                                 |
| FR02-03   | The system shall reject duplicate file names by silently ignoring any file whose name matches one already in the upload list.                |
| FR02-04   | The system shall clear the underlying native input value after each successful read so that the user may upload the same file again later.   |

### FR03: Accumulate Multiple Uploaded Files

The system shall maintain an in-memory list of all files that have been uploaded across multiple upload events.

| Sub-ID    | Requirement                                                                                                                                                                |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR03-01   | The system shall add each newly uploaded file to the existing list rather than replacing the prior contents.                                                                |
| FR03-02   | The system shall render each accumulated file as a bordered chip displaying the file name.                                                                                  |
| FR03-03   | The system shall arrange uploaded file chips vertically in a stacked layout.                                                                                                |

### FR04: Remove Uploaded File

The system shall allow the user to remove an individual file from the accumulated upload list.

| Sub-ID    | Requirement                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| FR04-01   | Each uploaded file chip shall display a circular cross icon in its right corner.                                                              |
| FR04-02   | Clicking the cross icon shall remove that specific file from the upload list while leaving all other files intact.                          |
| FR04-03   | The cross icon shall display a tooltip ("Remove file") on hover.                                                                              |

### FR05: Mutual Exclusion of Input Methods

The system shall prevent the user from supplying input through both the paste textarea and the file upload simultaneously.

| Sub-ID    | Requirement                                                                                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR05-01   | When the paste textarea contains any non-whitespace text, the system shall disable the file input control and visually fade it.                                    |
| FR05-02   | When at least one file has been uploaded, the system shall disable the paste textarea and visually fade it.                                                        |
| FR05-03   | The associated section headings shall change color to indicate the disabled state.                                                                                 |
| FR05-04   | The user shall be able to re-enable the inactive input only by clearing all entries from the currently active input.                                               |

### FR06: Parse Multi-FASTA Input

The system shall correctly identify and separate multiple sequences when more than one FASTA record is provided within a single text block or file.

| Sub-ID    | Requirement                                                                                                                                          |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR06-01   | The system shall split the input text at every line beginning with the greater-than character.                                                       |
| FR06-02   | The system shall associate each sequence body with its immediately preceding header line.                                                            |
| FR06-03   | The system shall analyze each split sequence independently and produce a separate result card per record.                                            |

### FR07: Validate DNA Sequence

The system shall verify that every analyzed sequence is composed exclusively of the four canonical DNA bases.

| Sub-ID    | Requirement                                                                                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR07-01   | The system shall require that the input begin with a `>` header line. If absent, the system shall emit an error stating that the FASTA header is missing.        |
| FR07-02   | The system shall strip header lines and whitespace before validation.                                                                                              |
| FR07-03   | The system shall reject any sequence that contains characters outside the set `{A, T, G, C}` (case-insensitive) with an error message "Not a DNA sequence".      |
| FR07-04   | The system shall continue processing other sequences in a multi-FASTA input even if one record fails validation.                                                   |

### FR08: Compute Sequence Length

The system shall calculate and display the total length of each valid DNA sequence.

| Sub-ID    | Requirement                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| FR08-01   | The system shall compute the length as the count of non-header, non-whitespace nucleotide characters in the sequence body.               |
| FR08-02   | The system shall display the length in the result card with the suffix "bases".                                                          |

### FR09: Compute GC Content

The system shall calculate the percentage of guanine and cytosine bases in each valid DNA sequence.

| Sub-ID    | Requirement                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| FR09-01   | The system shall count the number of `G` and `C` characters (case-insensitive) in the sequence body.                          |
| FR09-02   | The system shall divide the count by the total sequence length and multiply by 100.                                          |
| FR09-03   | The system shall round the result to two decimal places and display it as a percentage in the result card.                   |

### FR10: Compute AT Content

The system shall calculate the percentage of adenine and thymine bases in each valid DNA sequence.

| Sub-ID    | Requirement                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| FR10-01   | The system shall count the number of `A` and `T` characters (case-insensitive) in the sequence body.                          |
| FR10-02   | The system shall divide the count by the total sequence length and multiply by 100.                                          |
| FR10-03   | The system shall round the result to two decimal places and display it as a percentage in the result card.                   |

### FR11: Detect Source Organism

The system shall attempt a best-effort identification of the source organism from the FASTA header line.

| Sub-ID    | Requirement                                                                                                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR11-01   | If the header contains a bracketed organism name (e.g., `[Homo sapiens]`), the system shall extract and display the bracketed name verbatim.                                              |
| FR11-02   | If no bracketed name is present, the system shall search the header (case-insensitive) for known keywords such as `homo`, `mus`, `rattus`, `drosophila`, `escherichia`, and `saccharomyces`. |
| FR11-03   | The system shall map matched keywords to a human-readable name such as "Human", "Mouse", "Rat", "Fruit Fly", "E. coli", and "Yeast".                                                       |
| FR11-04   | If neither method succeeds, the system shall display the organism as "Unknown".                                                                                                            |

### FR12: Render Stacked Output Cards

The system shall present each analyzed sequence as a separate result card stacked vertically below the analysis controls.

| Sub-ID    | Requirement                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| FR12-01   | Each card shall display the sequence label as its heading.                                                                               |
| FR12-02   | Each card shall display four statistic rows: Sequence Length, GC Content, AT Content, and Organism.                                      |
| FR12-03   | Cards shall be visually distinguished by a rounded border, semi-transparent background, and a separator below the heading.                |
| FR12-04   | If multiple sequences are analyzed, their cards shall appear in the order in which the sequences were provided.                          |

### FR13: Render GC/AT Bar Chart per Sequence

The system shall render an independent bar chart inside each result card showing the GC and AT percentages for that sequence.

| Sub-ID    | Requirement                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| FR13-01   | The chart shall display two bars: one labelled "GC Content" and the other "AT Content".                                                       |
| FR13-02   | The chart axis shall range from 0 to 100 percent.                                                                                            |
| FR13-03   | Each bar shall display its percentage value as a tooltip on hover.                                                                           |
| FR13-04   | The GC bar shall be rendered in blue and the AT bar in red for visual distinction.                                                            |

### FR14: Submit Sequence to NCBI BLASTN

The system shall provide a button on each result card that submits the corresponding sequence to the NCBI BLASTN service for homology searching.

| Sub-ID    | Requirement                                                                                                                                            |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR14-01   | Each result card shall display a button labelled "Fetch Relative Sequence" at the top-right of its heading.                                            |
| FR14-02   | Clicking the button shall open a modal dialog and begin the BLAST workflow for that specific sequence.                                                  |
| FR14-03   | The system shall submit the request via an internal API route (`/api/blast/submit`) which proxies the request to `https://blast.ncbi.nlm.nih.gov/Blast.cgi` to avoid CORS restrictions. |
| FR14-04   | The system shall include the parameters `CMD=Put`, `PROGRAM=blastn`, `MEGABLAST=on`, `DATABASE=refseq_select_rna`, and `HITLIST_SIZE=10`.                |
| FR14-05   | The system shall parse the HTML response and extract the Request Identifier (RID) using a regular expression.                                          |

### FR15: Poll BLAST Status Periodically

The system shall periodically check the status of a submitted BLAST job until it is either ready or has failed.

| Sub-ID    | Requirement                                                                                                                                              |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR15-01   | The system shall make a status request immediately after the RID is received.                                                                            |
| FR15-02   | The status request shall hit the internal API route `/api/blast/status?rid=...` which queries `Blast.cgi` with `FORMAT_OBJECT=SearchInfo`.                |
| FR15-03   | If the status is `WAITING` or `SEARCHING`, the system shall schedule another check after 30 seconds.                                                     |
| FR15-04   | The system shall display a visible countdown timer ("Next check in N seconds") that decrements every second until the next request fires.                |
| FR15-05   | If the status is `FAILED` or `UNKNOWN`, the system shall stop polling and present an error message.                                                      |
| FR15-06   | If the status is `READY`, the system shall stop polling and present a button labelled "Fetch Relative Sequences".                                        |

### FR16: Display Pending and Ready States

The system shall present clear visual feedback during each stage of the BLAST workflow inside the modal dialog.

| Sub-ID    | Requirement                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| FR16-01   | The submitting stage shall show a spinning blue indicator and the text "Submitting sequence to BLAST...".                                |
| FR16-02   | The pending stage shall show a spinning yellow indicator, the label "Status: Pending", and the RID.                                       |
| FR16-03   | The ready stage shall show a green check mark, the label "Status: Ready", and a primary action button to fetch the results.              |
| FR16-04   | The fetching stage shall show a spinning blue indicator and the text "Fetching relative sequences...".                                    |
| FR16-05   | The error stage shall show a red warning icon and the descriptive error message returned by the API.                                      |

### FR17: Fetch and Display BLAST Hits

The system shall retrieve the final BLAST results when the user clicks the fetch button on the ready stage.

| Sub-ID    | Requirement                                                                                                                                                                       |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR17-01   | The system shall hit the internal API route `/api/blast/results?rid=...` which requests the NCBI `Blast.cgi` endpoint with `FORMAT_TYPE=JSON2_S` (single-file JSON, no zip).        |
| FR17-02   | The system shall parse the JSON response and extract the array of hits from `BlastOutput2.report.results.search.hits`.                                                            |
| FR17-03   | For each hit, the system shall extract the accession, title, scientific organism name, taxonomic ID, hit length, bit score, e-value, identity, alignment length, and percentage identity. |
| FR17-04   | The system shall render each hit as a separate card inside the modal with all extracted fields clearly labelled.                                                                  |
| FR17-05   | If the search produces no hits, the system shall display the message "No significant hits found".                                                                                  |

### FR18: Download Results as CSV

The system shall allow the user to download the BLAST result set in comma-separated-values format.

| Sub-ID    | Requirement                                                                                                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR18-01   | The results modal shall display a button labelled "Download CSV" when at least one hit is present.                                                                       |
| FR18-02   | Clicking the button shall generate a CSV file in memory with the columns: Rank, Accession, Title, Organism, Tax ID, Hit Length, Bit Score, E-value, Identity, Align Length, % Identity. |
| FR18-03   | The system shall escape fields that contain commas, quotes, or newlines according to RFC 4180.                                                                            |
| FR18-04   | The CSV shall be prepended with a UTF-8 byte-order-mark so that Excel renders Unicode characters correctly.                                                              |
| FR18-05   | The downloaded file shall be named `<sequence_label>_blast.csv` with special characters sanitized to underscores.                                                         |

### FR19: Download Results as JSON

The system shall allow the user to download the BLAST result set in JavaScript Object Notation format.

| Sub-ID    | Requirement                                                                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FR19-01   | The results modal shall display a button labelled "Download JSON" when at least one hit is present.                                                                          |
| FR19-02   | Clicking the button shall generate a JSON file in memory containing the RID, query name, ISO timestamp of generation, total hit count, and the full array of hit objects.   |
| FR19-03   | The JSON shall be pretty-printed with an indent of two spaces.                                                                                                               |
| FR19-04   | The downloaded file shall be named `<sequence_label>_blast.json`.                                                                                                            |

### FR20: Animated Responsive Navigation Bar

The system shall provide a sticky top navigation bar that responds to the user's scroll position.

| Sub-ID    | Requirement                                                                                                                                            |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR20-01   | The navbar shall be visible on all pages and shall contain three links: Home, About, and Contact Us.                                                   |
| FR20-02   | In its default (top-of-page) state the navbar shall span the full width of the viewport with a dark translucent background.                            |
| FR20-03   | When the user scrolls more than 30 pixels from the top, the navbar shall smoothly transition to a compact pill-shaped capsule positioned 2 pixels from the top edge. |
| FR20-04   | The navbar shall slide in from above on initial page load using a CSS keyframe animation.                                                              |
| FR20-05   | The currently active route shall be highlighted with a solid blue rounded background.                                                                  |
| FR20-06   | Inactive links shall reveal a subtle hover background and color change.                                                                                 |

### FR21: About Page with Animated Background

The system shall provide an About page that introduces the project and the developer.

| Sub-ID    | Requirement                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| FR21-01   | The About page shall display a project title, an introductory paragraph, a feature list, and a technology list.                       |
| FR21-02   | The background shall use a linear gradient that animates its position over 8 seconds in an infinite loop.                            |
| FR21-03   | The page shall include three semi-transparent floating orbs that move independently on different timing functions.                   |
| FR21-04   | All textual content shall enter the viewport with a staggered slide-up animation.                                                    |

### FR22: Contact Us Page

The system shall provide a Contact Us page with relevant contact information.

| Sub-ID    | Requirement                                                                                                                            |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| FR22-01   | The Contact Us page shall display the supervisor's name and email address.                                                              |
| FR22-02   | The Contact Us page shall display the developer's name, registration number, and email address.                                         |
| FR22-03   | The Contact Us page shall display the institution name and address.                                                                     |

### FR23: Clear All Results

The system shall allow the user to reset the analysis state with a single click.

| Sub-ID    | Requirement                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| FR23-01   | The Home page shall provide a button labelled "Clear All Results".                                                                       |
| FR23-02   | Clicking the button shall empty the paste textarea, the uploaded files list, the results array, and any displayed error messages.        |

### FR24: Display Error Messages

The system shall provide clear, user-friendly error feedback for all failure scenarios.

| Sub-ID    | Requirement                                                                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| FR24-01   | Errors raised during validation (missing header, non-DNA characters, empty body) shall be displayed in red text below the analysis controls.                  |
| FR24-02   | Network or server errors from the BLAST proxy routes shall be displayed inside the modal in the error stage.                                                  |
| FR24-03   | If the user clicks Analyze without providing any input, the system shall display the message "Please paste a FASTA sequence or upload FASTA file(s)".         |

## 2.2 Non-Functional Requirements

Non-functional requirements describe the quality attributes that the FASTA Sequence Analyser must satisfy. They constrain how the system behaves rather than what specific operations it performs. The following non-functional requirements have been identified for the project.

**Table 2:** Non-Functional Requirements Summary

| ID    | Quality Attribute     | Description                                                                                                  |
| ----- | --------------------- | ------------------------------------------------------------------------------------------------------------ |
| NFR01 | Availability          | System shall be operational whenever the host server is online and the user has internet connectivity.        |
| NFR02 | Performance           | Local analysis shall complete within 1 second for sequences up to 100,000 bases.                              |
| NFR03 | Responsiveness        | UI shall render correctly on screens from 320 px (mobile) to 2560 px (desktop).                              |
| NFR04 | Usability             | All controls shall have tooltips, hover states, and clear visual feedback.                                    |
| NFR05 | Reliability           | The application shall not crash when given invalid FASTA input.                                              |
| NFR06 | Security              | No user data shall be persisted on the server.                                                               |
| NFR07 | Maintainability       | The codebase shall use TypeScript for type safety and shall follow a modular component architecture.        |
| NFR08 | Portability           | The application shall run on any modern web browser (Chrome, Firefox, Edge, Safari).                          |
| NFR09 | Scalability           | The application shall handle at least 10 simultaneously uploaded files without UI degradation.                |
| NFR10 | Interoperability      | The application shall correctly consume the NCBI BLASTN Common URL API.                                       |
| NFR11 | Recoverability        | Network failures during BLAST polling shall be presented as recoverable errors with a Close action.          |
| NFR12 | Accessibility         | All interactive controls shall be keyboard reachable and shall have appropriate ARIA-equivalent semantics.    |

### NFR01: Availability

The system shall remain available to its users for as long as the hosting infrastructure is online. As the application is stateless and depends only on the NCBI BLASTN public API for external integration, no internal database needs to be maintained for availability. The application is designed to gracefully degrade if the NCBI service is temporarily unreachable: in such a scenario, only the BLAST integration becomes unavailable while the local analysis features (parsing, GC/AT computation, charts) continue to work normally.

### NFR02: Performance

The system shall complete local sequence analysis within 1 second for inputs of up to 100,000 nucleotide bases. Larger sequences shall not block the user interface thread for more than 2 seconds. The application shall render the bar chart for each sequence using Chart.js's optimized canvas rendering pipeline. BLAST submission and result retrieval are bound by the external NCBI service and may take from 30 seconds to several minutes; the user interface shall remain responsive throughout this period.

### NFR03: Responsiveness

The system shall provide a responsive layout that adapts to a wide range of screen sizes. On mobile devices the controls shall stack vertically, the chart shall scale to the container width, and the modal dialog shall fill the available viewport. On desktop devices the application shall use a constrained content width (max 2xl) for optimal readability.

### NFR04: Usability

The system shall provide consistent, intuitive controls. Every interactive element shall provide hover and active states. Buttons that trigger destructive or irreversible actions shall have tooltips that describe their effect. Disabled controls shall be visually distinct (opacity 40%) and shall not respond to clicks.

### NFR05: Reliability

The system shall not crash, hang, or produce unhandled exceptions in response to malformed input. All FASTA parsing and BLAST response parsing shall be wrapped in defensive error handlers that produce user-readable error messages rather than internal stack traces.

### NFR06: Security

The system shall not persist any user-supplied data on the server. All uploaded files are read in the browser using the FileReader API and the resulting content is held only in client-side React state. The BLAST proxy routes simply forward requests to NCBI and return the parsed response without writing to disk or to any external storage.

### NFR07: Maintainability

The codebase shall be written entirely in TypeScript with strict mode enabled. All public functions and React components shall declare explicit parameter and return types. The project shall follow the Next.js App Router convention with a clear separation between `app/` (routes), `components/` (reusable UI), and `utils/` (pure logic).

### NFR08: Portability

The system shall run on any modern web browser that supports ES2020, the Fetch API, the FileReader API, and the URL.createObjectURL API. This includes Chrome 90+, Firefox 88+, Edge 90+, and Safari 14+.

### NFR09: Scalability

The system shall handle at least ten files queued for analysis simultaneously without any perceived degradation of UI responsiveness. Each FASTA file is processed independently in a tight loop, and the resulting cards are rendered using React's virtual DOM diffing.

### NFR10: Interoperability

The system shall correctly consume the NCBI BLASTN Common URL API in accordance with the documented `CMD=Put`, `CMD=Get` (`FORMAT_OBJECT=SearchInfo`), and `CMD=Get` (`FORMAT_TYPE=JSON2_S`) workflows.

### NFR11: Recoverability

If a transient network failure occurs during BLAST polling or result retrieval, the modal shall display an error message and offer a Close action. The user may then re-attempt the operation from the result card.

### NFR12: Accessibility

All interactive controls shall be reachable via the keyboard Tab key. Focused elements shall display a visible focus ring. Color contrast ratios shall meet WCAG AA standards for text-on-background combinations used throughout the application.

## 2.3 Hardware Requirements

The FASTA Sequence Analyser is a client-server web application. The client-side hardware requirements are minimal because all heavy computation either runs on the user's browser (for local analysis) or on the remote NCBI BLAST servers (for homology searching). The following hardware specifications are the minimum recommended for a smooth user experience.

**Table 3:** Hardware Requirements

| Resource    | Minimum Specification                                        |
| ----------- | ------------------------------------------------------------ |
| Processor   | Pentium(R) Core i3 1.6 GHz or higher                          |
| Memory      | 4 GB RAM or more                                             |
| Storage     | 200 MB free disk space (for browser cache)                    |
| Display     | 1280 × 720 resolution or higher                              |
| Network     | Stable broadband connection (at least 1 Mbps for BLAST API)  |
| Input       | Standard keyboard and mouse / touchscreen                     |

## 2.4 Software Requirements

The application is delivered as a web page and runs in any standards-compliant web browser. The following software is required on the user's side to access the FASTA Sequence Analyser.

**Table 4:** Software Requirements

| Component        | Minimum Specification                                                       |
| ---------------- | --------------------------------------------------------------------------- |
| Operating System | Windows 10, Windows 11, macOS 11, Ubuntu 20.04, or any modern Linux distro  |
| Web Browser      | Google Chrome 90+, Mozilla Firefox 88+, Microsoft Edge 90+, or Safari 14+   |
| JavaScript       | JavaScript must be enabled in the browser                                   |
| Network Access   | Outbound access to `https://blast.ncbi.nlm.nih.gov` on port 443             |
| Cookies          | First-party cookies should be allowed for session continuity (optional)     |

For development purposes, the following additional software is required on the developer's machine.

| Component         | Specification                                                                |
| ----------------- | ---------------------------------------------------------------------------- |
| Node.js           | Version 20 or later                                                          |
| Package Manager   | npm 10+ or pnpm 8+                                                           |
| Code Editor       | Visual Studio Code with TypeScript and ESLint extensions                     |
| Git               | Version 2.30 or later for source control                                     |

---

# 3. METHODOLOGY

## 3.1 Process Model

The Rapid Application Development (RAD) model has been selected as the most appropriate software process for the FASTA Sequence Analyser project. RAD is an incremental software development methodology that prioritizes rapid prototyping and iterative delivery over heavy upfront planning. Unlike the traditional waterfall model — where each phase (requirements, design, implementation, testing) must be completed in sequence — RAD encourages parallel development of smaller components, frequent user feedback, and continuous refinement of working prototypes. This makes RAD particularly suitable for projects with evolving requirements, single-developer teams, and a strong emphasis on user-facing features.

The FASTA Sequence Analyser project aligns naturally with RAD for several reasons. First, the requirements were not fully crystallized at the project's inception; instead, features such as multi-file upload accumulation, BLAST integration, and result downloading were progressively added as the project matured. Second, the project consists of clearly separable modules — input handling, sequence parsing, statistical analysis, charting, BLAST proxy, modal UI, and navigation — each of which can be prototyped, tested, and integrated independently. Third, the front-end nature of the application benefits from continuous visual feedback: every new feature can be observed in the browser within seconds of being written, allowing the developer to refine the user experience iteratively.

The RAD lifecycle, as applied to this project, consists of the following four broad activities arranged in a loop rather than a strict sequence.

1. **Business Modelling** — At the start of each iteration, the developer identifies a single concrete user-facing capability to be delivered next. For example, "the user should be able to paste a FASTA sequence and see GC content" was the first iteration's deliverable.
2. **Data Modelling** — Type definitions, interfaces, and component contracts are sketched out. In the case of this project, the `AnalysisResult` interface, the `BlastHit` interface, and the props of each React component constitute the data model.
3. **Process Modelling** — The interaction flow is mapped out. For BLAST integration, this meant designing the three-stage submission/polling/retrieval workflow before any code was written.
4. **Application Generation** — The code is written, executed, observed in the browser, and refined until the iteration's goal is met. After delivery, the cycle repeats for the next capability.

The RAD model fits this project particularly well because the developer is operating in a single-person team without the need for the heavy ceremony of Scrum sprints or the rigid gates of waterfall. The model also accommodates the fact that the developer is simultaneously a domain learner (learning bioinformatics concepts such as FASTA parsing and BLAST workflows) and an implementer: by building small prototypes and verifying them against real data, the developer naturally absorbs the domain knowledge needed for the next iteration.

![RAD Activities](placeholder-rad-activities.png)

*Figure 20: RAD Activities*

## 3.2 Tools & Technologies

The FASTA Sequence Analyser is built on a modern web stack that prioritizes developer productivity, type safety, and performance. The following tools and technologies were selected for the project.

**Table 5:** Tools & Technologies Used in the Project

| Tool / Technology  | Version | Purpose                                                                                  |
| ------------------ | ------- | ---------------------------------------------------------------------------------------- |
| Next.js            | 16.x    | React framework providing App Router, server-side routing, and API route handlers        |
| React              | 19.x    | UI library for building component-based interfaces                                       |
| TypeScript         | 5.x     | Statically typed superset of JavaScript                                                  |
| Tailwind CSS       | 4.x     | Utility-first CSS framework for rapid styling                                            |
| Chart.js           | 4.x     | Canvas-based charting library                                                            |
| react-chartjs-2    | 5.x     | React wrapper for Chart.js                                                               |
| Node.js            | 20+     | JavaScript runtime for development and production server                                 |
| npm                | 10+     | Package manager and script runner                                                        |
| Visual Studio Code | 1.85+   | Source code editor                                                                       |
| Git                | 2.30+   | Distributed source control                                                               |
| NCBI BLASTN API    | Public  | Remote sequence similarity search service                                                |

### Next.js

Next.js is a full-stack React framework that provides file-based routing, server-side rendering, static site generation, and API route handlers out of the box. In this project Next.js is used in App Router mode where each folder inside `src/app/` represents a route. The application leverages Next.js API routes to implement the BLAST proxy endpoints under `src/app/api/blast/`, which run on the server side and forward requests to NCBI without exposing the front-end to CORS restrictions.

### React 19

React is the foundational UI library used to build component-based interfaces. The application uses functional components with hooks such as `useState`, `useEffect`, and `useRef` to manage local state, side effects, and DOM references. React 19's improved automatic batching and concurrent rendering features contribute to the smooth animations and responsive interactions throughout the application.

### TypeScript

TypeScript is used to add static type checking on top of JavaScript. Every interface, function signature, and component prop is explicitly typed, which catches an entire class of runtime errors at compile time and provides excellent IDE auto-completion. The `AnalysisResult`, `BlastHit`, `FormattedHit`, and `UploadedFile` interfaces are central to the type system of the project.

### Tailwind CSS

Tailwind CSS is a utility-first framework that allows styles to be applied directly in JSX through atomic class names such as `flex`, `p-4`, `text-white`, and `rounded-lg`. This eliminates the need for separate stylesheet files and enables rapid prototyping. The project also defines a small number of custom keyframe animations (`navbar-slide-down`, `fade-in`, `slide-up`, `gradient-shift`, `float-1`, `float-2`, `float-3`) in `globals.css` to power the visual effects.

### Chart.js and react-chartjs-2

Chart.js renders the GC/AT bar charts shown in each result card. The `react-chartjs-2` wrapper provides a declarative React API around Chart.js's imperative canvas-based engine. The chart configuration is encapsulated inside the `GCChart` component, which accepts the GC percentage, AT percentage, and sequence name as props.

### NCBI BLASTN Common URL API

The application integrates with the National Center for Biotechnology Information's BLASTN service, which is offered as a publicly accessible HTTP API. The integration follows a three-step asynchronous workflow.

**Table 6:** NCBI BLASTN API Endpoints Used

| Step | URL                                                                                                                                                                                                | Purpose                                                              |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| 1    | `https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Put&PROGRAM=blastn&MEGABLAST=on&DATABASE=refseq_select_rna&HITLIST_SIZE=10&QUERY=...`                                                                  | Submit a sequence and receive a Request Identifier (RID).             |
| 2    | `https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=...&FORMAT_OBJECT=SearchInfo`                                                                                                                | Check the status of an ongoing BLAST job.                            |
| 3    | `https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&RID=...&FORMAT_TYPE=JSON2_S`                                                                                                                     | Retrieve the final results as a single JSON file (no zip archive).   |

The `JSON2_S` format was chosen over `JSON2` because the latter returns a zip archive that would require a server-side unzip library; the single-file variant returns clean JSON that can be parsed directly with `JSON.parse`.

### Project Architecture

The application follows a clean three-tier architecture suitable for a single-page web app.

1. **Presentation Layer** — React components in `src/components/` (e.g., `Navbar`, `GCChart`, `BlastModal`) and route pages in `src/app/` handle all UI concerns.
2. **Logic Layer** — Pure TypeScript modules in `src/utils/` (e.g., `fastaAnalyzer.ts`) contain the sequence parsing, validation, and statistical computation logic. These functions are framework-agnostic and could be reused in a Node.js CLI or a Vite app without modification.
3. **Integration Layer** — Server-side API routes in `src/app/api/blast/` proxy requests to the external NCBI service, hide the CORS complexity from the browser, and centralize response parsing.

**Table 7:** Project Module Breakdown

| Module                                       | Type           | Responsibility                                                  |
| -------------------------------------------- | -------------- | --------------------------------------------------------------- |
| `src/app/page.tsx`                           | Page           | Home page with input controls, results stack, and modal mount   |
| `src/app/about/page.tsx`                     | Page           | About page with animated background                             |
| `src/app/contact/page.tsx`                   | Page           | Contact information page                                        |
| `src/app/layout.tsx`                         | Layout         | Root layout that wraps every page and mounts the navbar         |
| `src/app/api/blast/submit/route.ts`          | API Route      | Submits sequence to NCBI and returns the RID                    |
| `src/app/api/blast/status/route.ts`          | API Route      | Polls NCBI for BLAST job status                                 |
| `src/app/api/blast/results/route.ts`         | API Route      | Retrieves and parses the final BLAST results                    |
| `src/components/Navbar.tsx`                  | Component      | Sticky animated navigation bar                                  |
| `src/components/GCChart.tsx`                 | Component      | GC/AT bar chart rendered with Chart.js                          |
| `src/components/BlastModal.tsx`              | Component      | Multi-stage BLAST workflow modal with download buttons          |
| `src/utils/fastaAnalyzer.ts`                 | Logic Module   | FASTA parsing, validation, and GC/AT/organism computation       |
| `src/app/globals.css`                        | Stylesheet     | Tailwind import and custom keyframe animations                  |

---

# 4. TIMELINE

The FASTA Sequence Analyser project was planned to be delivered across a single academic semester comprising approximately 16 weeks. The work was decomposed into seven major phases following the iterative RAD methodology described in Section 3.1. A Gantt chart summarizing the tentative schedule is shown in Figure 21 below. Each phase is described in the paragraphs that follow.

**Phase 1 — Project Initiation and Requirements Gathering (Weeks 1–2)**

The first phase concentrated on understanding the problem domain. The student studied the FASTA format specification, the basics of nucleotide composition analysis, and the structure of common bioinformatics workflows. Discussions with the supervisor refined the project scope, identified the primary user persona (undergraduate bioinformatics students), and produced an initial list of functional requirements.

**Phase 2 — Technology Selection and Project Setup (Weeks 2–3)**

The second phase focused on choosing the technology stack and bootstrapping the project. After comparing Vite, Create React App, and Next.js, the latest version of Next.js was selected because it offers built-in API route handlers (required for the BLAST proxy), TypeScript support, and a developer-friendly file-based routing system. The base project was scaffolded using `create-next-app` with the App Router, TypeScript, ESLint, and Tailwind CSS options enabled.

**Phase 3 — Core Analysis Engine (Weeks 4–6)**

The third phase produced the framework-agnostic FASTA analysis module (`fastaAnalyzer.ts`). The module exposes pure functions for splitting multi-FASTA input, validating DNA content, computing GC and AT percentages, and detecting the source organism. The Home page was wired up with the paste textarea, the file-upload control, and the result card layout. The `GCChart` component was implemented next, completing the local analysis workflow.

**Phase 4 — Multi-Input Enhancements (Weeks 6–8)**

The fourth phase added the mutual-exclusion behaviour between paste and upload inputs, the multi-file accumulation feature, the per-file remove button, and the per-sequence stacked output cards. Each enhancement was prototyped, tested with synthetic sequences, and refined based on usability observations.

**Phase 5 — Navigation and Aesthetic Polish (Weeks 8–10)**

The fifth phase introduced the multi-page navigation, the animated navbar (full-width banner to compact pill on scroll), and the staggered slide-up animations on the Home page content. The About page was created with its gradient-shift background and floating orbs, and the Contact Us page was added with supervisor and developer information.

**Phase 6 — BLAST Integration (Weeks 10–13)**

The sixth phase implemented the NCBI BLASTN integration. Three internal API routes were created (`submit`, `status`, `results`), each proxying a specific stage of the BLAST workflow. The `BlastModal` component was developed with five distinct stages (submitting, pending, ready, fetching, results) and a thirty-second polling timer with a visible countdown. The CSV and JSON download buttons were added in the final iteration of this phase.

**Phase 7 — Testing, Documentation, and Deployment (Weeks 13–16)**

The final phase focused on end-to-end testing against real-world FASTA inputs, error-handling refinement, type-safety verification through `next build`, and the preparation of this research report. The application was deployed to a Vercel preview environment for supervisor review.

![Tentative Timeline of the Project Activities](placeholder-gantt.png)

*Figure 21: Tentative Timeline of the Project Activities*

---

*End of Research Report I*
