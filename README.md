# Lab 1 - Fama and French Asset Pricing Replication Lab

## Submission Deadline - EOD, 17 September, 2026
## Purpose of the Lab

The objective of this lab is to understand how asset-pricing models are evaluated using real portfolio returns and to reproduce selected results from Fama and French (2012) as closely as possible using the current Kenneth French datasets.

By completing the lab, you should learn how to interpret important asset-pricing indicators such as intercepts, factor loadings, t-statistics, R-squared values, the GRS test and the Sharpe ratio of the intercepts. You will examine whether the Capital Asset Pricing Model, the Fama-French three-factor model and the four-factor model that includes momentum can explain the returns of portfolios formed on size, book-to-market equity and momentum. The lab will also help you compare global and local factor models and understand why a model may perform well for some portfolios or regions but not for others.

The broader aim is to connect the asset-pricing concepts covered in class with the way they are applied and tested in an influential empirical finance paper. The lab is therefore not limited to reproducing numerical results. You should be able to explain what the results reveal about the strengths and limitations of each model.

Each notebook provides a suggested structure, a sequence of tasks and some guidance to help you complete the replication. This structure is not the only correct way to solve the lab. You may organize your code differently, create additional cells or use alternative Python methods.

Your submission will be evaluated primarily on whether:

1. The relevant results from the paper are replicated to a reasonably close extent.
2. The objectives of each task are completed.
3. The calculations and code are correct and reproducible.
4. The results are compared properly with the paper.
5. The interpretations demonstrate your own understanding of the results.

Small numerical differences from the paper are acceptable because the Kenneth French datasets have been revised since the paper was published.

## Reference Paper

This lab replicates selected results from Fama and French (2012).

Paper link:

https://www.sciencedirect.com/science/article/abs/pii/S0304405X12000931

To access the paper, select “Access through your organization.” BITS Pilani provides access through OpenAthens. Sign in using your BITS Pilani credentials.

### Citation

Eugene F. Fama, Kenneth R. French,
“Size, Value, and Momentum in International Stock Returns,”
Journal of Financial Economics,
Volume 105, Issue 3,
2012,
Pages 457–472,
ISSN 0304-405X.
https://doi.org/10.1016/j.jfineco.2012.05.011

Full-text link:

https://www.sciencedirect.com/science/article/pii/S0304405X12000931

## Dataset

The data are obtained from the Kenneth R. French Data Library:

https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html

The Data Library is updated periodically. Therefore, results calculated using the latest files may differ slightly from the values published in 2012.

## Instructions

### Step 1: Download and prepare the data

Read `data.docx` carefully.

Follow its instructions to:

1. Download the required datasets.
2. Extract and rename the CSV files.
3. Place the files in the `raw_data` folder.
4. Run `00_clean_data.ipynb`.
5. Check that the cleaned files have been created correctly.

Do not begin the replication notebooks until the data-cleaning checks are complete.

### Step 2: Read the paper

Read the complete reference paper before starting the replication.

Understand:

1. The purpose of the study.
2. How the portfolios and factors are constructed.
3. What each table reports.
4. Why the authors compare global and local factor models.
5. How the authors interpret the results.

### Step 3: Complete the notebooks

Complete the notebooks one at a time and in the following order:

1. `01_replicate_table1.ipynb`
2. `02_replicate_table2.ipynb`
3. `03_replicate_table3_table4.ipynb`
4. `04_replicate_table6_table7.ipynb`

For every task:

1. Write and run the required Python code.
2. Check that the output is reasonable.
3. Compare the output with the relevant table in the paper.
4. Note any differences from the published values.
5. Understand what the result shows before proceeding.

Small numerical differences are acceptable because the Kenneth French datasets have been updated since the paper was published.

## Final Deliverable

Submit one ZIP folder containing the following 9 files:

1. `01_replicate_table1.ipynb`
2. `02_replicate_table2.ipynb`
3. `03_replicate_table3_table4.ipynb`
4. `04_replicate_table6_table7.ipynb`
5. `01_replicate_table1.html`
6. `02_replicate_table2.html`
7. `03_replicate_table3_table4.html`
8. `04_replicate_table6_table7.html`
9. One report in Word or PDF format

Before exporting a notebook:

1. Run every cell from top to bottom.
2. Confirm that all code outputs, tables and figures are visible.
3. Save the notebook.
4. Select `File → Save and Export Notebook As → HTML`.

Open each exported HTML file once before submission and confirm that all code, tables, figures and written responses are visible.


## Report Structure

Organize the report by notebook and task.

Use the following structure:

### Notebook 01

#### Task 1

Insert the relevant table or figure produced by the notebook.

Write a detailed interpretation of the result.

#### Task 2

Insert the relevant table or figure produced by the notebook.

Write a detailed interpretation of the result.

Continue this structure for every task in Notebook 01. Then repeat it for Notebooks 02, 03 and 04.

For each result, discuss:

1. What the table or figure reports.
2. Whether the result is close to the corresponding result in the paper.
3. Any important numerical differences.
4. The main patterns visible in the result.
5. What you understand from those patterns.

The interpretation prompts given in the notebooks are only for reference. You may report any additional meaningful patterns you observe.

Your interpretations must be written in your own words and in clear, understandable language. Explain the results based on how you understood them. Do not simply copy the discussion from the paper or AI, your interpretations must be your own (because you already know what the final numerical result should be).
