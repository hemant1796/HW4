HW4-Data-Visualization
================

**Due:** Thursday, March 2, 2023 by 11:59pm **Submitted by : ** Hemant
kumar

**UIN** 01243485 \## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

The goal of this assignment is to propose and implement charts based on
questions asked about real-world data. *Read through the entire
assignment before starting.*

## Datasets

We are going to use two datasets from the [Virginia Department of
Health’s COVID-19
Resources](https://www.vdh.virginia.gov/coronavirus/covid-19-in-virginia/).
The datasets are made available through the [Virginia Open Data
Portal](https://data.virginia.gov).

1.  Weekly cases by vaccination status by health region  
    <https://data.virginia.gov/dataset/VDH-COVID-19-PublicUseDataset-Cases-by-Vaccination/vsrk-d6hx>
    \> This dataset includes the number of COVID-19 infections,
    hospitalizations, and deaths for each health region in Virginia by
    the week of onset/specimen collection and by vaccination status.

2.  Daily vaccines administered by health district  
    <https://data.virginia.gov/dataset/VDH-COVID-19-PublicUseDataset-Vaccines-DosesAdmini/28k2-x2rj>
    \> This dataset includes the number of COVID-19 vaccine doses
    administered for each locality in Virginia by administration date
    and by facility type. The data set increases in size daily and as a
    result, the dataset may take longer to update; however, it is
    expected to be available by 12:00 noon daily.

For each dataset, you can click the Export button on the dataset page to
download the CSV, or you can use the API endpoint to access the data
([example Observable
notebook](https://observablehq.com/d/2b7bf74dbcb9dacf), [example R
Google Colab
notebook](https://github.com/odu-cs625-datavis/public/blob/main/Spr22/R_SODA_API_for_VDH_data.ipynb),
[example Python/Pandas Google Colab
notebook](https://github.com/odu-cs625-datavis/public/blob/main/Spr22/Va_Open_Data_Portal_API_Example.ipynb)).

Since one dataset is aggregated on health regions and the other is
aggregated on localities (but includes health districts), you may need
this information about [Virginia health regions, districts, and
localities](https://www.vdh.virginia.gov/content/uploads/sites/182/2020/08/VA-regions_districts_localities.pdf)
(pdf)

## Assignment

As you work through this, pay attention to detail and the visualization
principles we have discussed in class when designing your charts. Charts
must have appropriately labeled axes and appropriate unit formats. In
your report ([see below](#report)), you will describe the design
decisions you have made, so take notes along the way as you work through
your design process.

You may use whatever tool(s) you wish to manipulate the data and create
the charts.

### Part 1

Use dataset 1 to create a chart to answer the question, “How has the
weekly rate of COVID-19 cases in Virginia changed over time based on
vaccination status?”. (Though I’ll use cases here, you may show cases,
hospitalizations, or deaths.)

#### Calculating the Weekly Rate

Since there are different numbers of people in various vaccination
states over time, we want to normalize the values instead of just
presenting the raw numbers. For instance, if there are 10 people in each
category that contract COVID, but there are 100 times more people who
are fully vaccinated than unvaccinated, that makes a big difference in
the case rate.

Typically these numbers are presented as rates per 100,000 people.
Dataset 1 contains an attribute that can help us calculate this rate.
The `population_denominator` attribute provides an estimate of the
number of people in the state with a certain vaccination status for that
week.

The general formula is
`rate_per_100k = (cases / population_denominator) * 100000`. For
example, if there were 100 COVID cases among unvaccinated in the state
in one week and there were 2,000,000 people unvaccinated (represented in
`population_denominator`), then

`rate_per_100k = (100 / 2,000,000) * 100000 = 5`

This means that this rate is equivalent to 5 cases in a population of
100,000.#

**Answer 1 **

OpenRefine:

1.  I Opened the dataset in OpenRefine.
2.  Created a new column named “rate_per_100k” using the expression:
    value(‘Deaths’) / value(‘population_denominator’) \* 100000
3.  Filtered the dataset to only include cases.
4.  Grouped the data by vaccination status and date.
5.  Aggregated the data by summing the cases and taking the mean of the
    rate per 100k for each group and date.

Tableau:

1.  I Opened Tableau and connect to the exported CSV file from
    OpenRefine.
2.  Dragged the “Date” field to the Columns shelf.
3.  Dragged the “rate_per_100k” field to the Rows shelf.
4.  Dragged the “Vaccination Status” field to the Color shelf.
5.  Dragged the “Deaths” field to the Size shelf.
6.  Changed the mark type to “Line”.

Following the above steps I got a line chart in Tableau that shows the
weekly rate of COVID-19 Deaths in Virginia over time based on
vaccination status, with each line representing a different vaccination
status group.

![HW4-Q1](https://user-images.githubusercontent.com/31125760/222594334-e44151be-4e2b-483b-9a67-9fa2018c1175.png)

### Part 2

Use dataset 2 to create a chart to answer the question, “For each health
district in Virginia, what proportion of all 1st doses were of the
Pfizer vaccine?”.

Remove or ignore the “health districts” labeled “Out of State” and “Not
Reported”.

**Answer 2: ** 1. Connect to the dataset: Open Tableau and click on
“Connect to Data”. Select “Microsoft Excel” as the file type and
navigate to the file containing the Virginia Department of Health’s
COVID-19 Public Use Dataset on Vaccines Doses Administered that you
provided earlier.

2.  Create a calculated field: Right-click on the “1st Dose \#” field in
    the “Measures” pane, and select “Create Calculated Field”.

3.  Create a new sheet: Click on the “New Worksheet” button in the
    bottom-left corner of the Tableau window.

4.  Drag and drop fields onto the view: In the “Data” pane, drag the
    “Health District” and “Facility Type” fields to the “Columns” shelf.
    Drag the “Pfizer 1st Dose \#” field to the “Rows” shelf.

5.  Change the chart type to a pie chart: Click on the “Show Me” button
    in the top-right corner of the Tableau window, and select the “Pie”
    chart type.

6.  Filter the data: Drag the “Health District” field to the “Filters”
    shelf. In the filter dialog box, uncheck the boxes next to “Out of
    State” and “Not Reported” to remove those health districts from the
    view.

7.  Color the chart by health district: Drag the “Health District” field
    to the “Color” shelf.

8.  Label the slices: Click on the “Label” button in the “Marks” card,
    and select “Value”.

9.  Adjust the chart formatting: Use the formatting options in the
    “Marks” card and “Format” pane to adjust the chart colors, labels.

![HW4-Q2](https://user-images.githubusercontent.com/31125760/222600736-737e2430-4925-4d7b-a264-c890265063ce.PNG)

### Part 3

Propose two questions that require data from dataset 1 and dataset 2 to
be combined to answer. Describe what data manipulation would need to be
done to answer each question. *Sketch* a chart that could be used the
answer each question. Justify your visualization idiom choice.

When merging the two data sets, two questions came out.

**Question 1:** On bases of region, does the total number of doses
admininsted affect rate of infections, hosptilalizations, and deaths in
that particular region?

To merge both data sets, there should be one common attribute. That
attribute for me was Health Region. At first, I have created a new
column on the second data set that represents Health Region. In that
column, I have found the Health Region by using the data from Health
District based on the data from the VDH. Therefore, I filtered Health
Region and picked certain columns from it. The columns that I have
picked were Hospitalizations, Deaths, and Vaccine Doses Administered
count. I have used scatter plot to plot between Vaccine Administered
count and Hospitalizations, and Vaccine Administered count and Deaths.
Scatter plot is the right option due to having two quantitative values.
For instance, Hospitalization has increased when Vaccine Administered
count was low. But, Deaths decreased when Vaccine Administered count per
Health Region increased. Below is the sketch for both Hospitalizations
and deaths.

![1](https://user-images.githubusercontent.com/31125760/222619633-38656f9b-6cc0-4837-b6b3-384433215298.jpeg)

![2](https://user-images.githubusercontent.com/31125760/222619645-50b8ac15-1071-47dc-a3ab-8011577d68b5.jpeg)

**Question 2:** Based on the number of doses, which Health Region had
the rate of the third dose?

Based on the merged data set,I have filtered the dose number to be equal
to 3. From there,I have aggregated by “Health Region” and summed it
up.Then, got only the Vaccine Administered count and plot it. The best
option to plot this is bar graph. Bar graph is well-suitable for this
problem due to having a categorical value and ordinal value. For
instance, Northern Region had the highest third dose in all regions.
Below is the sketch for third dose between regions.

![3](https://user-images.githubusercontent.com/31125760/222619712-d8909288-2baa-49ff-88d0-ef82302afc6e.jpeg)

## Files to Include

You must include all of these files in your GitHub repo (as applicable):

- edited data file(s) - *you may edit the original datafiles as needed*
- exported chart image (not screenshot) for each chart generated
- Excel: spreadsheet
- R: R code to generate the chart (can be in a RMarkdown notebook)

In addition, for online formats include links to the source in your
report:

- R or Python: if you used Google Colab, include the link to your
  notebook (save a copy to your GitHub repo)
- Vega-Lite: link to Observable notebook that generates the chart
- Google Sheets: link to shared Google Sheets (share with
  poursardar@<nolink>cs.odu.edu and <sdabh002@odu.edu>)
- Tableau: publish your Tableau notebook to our class Tableau online
  site and include the link to your workbook in your report
  - see
    <https://help.tableau.com/current/pro/desktop/en-us/publish_workbooks_share.htm>
  - *set permissions so that only you and I can view the workbook*, see
    <https://help.tableau.com/current/pro/desktop/en-us/publish_workbooks_permissions_add.htm>

## Report

For this project, you can either directly write in Markdown in
`report.md` or your can use R Markdown in `report.Rmd` and the Knit
process to generate your `report.md`. (In any case, `report.md` is the
file that I will use for grading.)

Your report should be a narrative, not just a set of bullet points.
Treat this as a stand-alone document, not something that needs the
assignment to be understood. *Describe any insights you gained by
exploring this data. Did it prompt you to ask further questions?*

Directly include your charts as images (or generated R charts) in your
report. At this stage, your charts do not need any interactive elements,
so static images are acceptable.

Other items to include: \* description of data used (specify attributes
and their data types) \* for each chart \* explain any data manipulation
you performed \* describe the marks and channels used and what
attributes are mapped to which channels, justifying why these are
appropriate choices \* justify the choice of the chart idiom based on
the data \* describe the design decisions you made in creating the chart
\* describe how the chart you created answers the question that was
asked \* list of references - links to any examples that you used in
completing this assignment

**Important:** Your report is an important part of this assignment. I
have not provided a template, but I expect your report to include your
name, CS625-HW4, due date, and appropriate headings and Markdown markup
for clarity and neatness. You will lose points if there are many
spelling or formatting errors.

## Submission

Make sure that you have committed and pushed your local repo to GitHub.
Include “Ready to grade @Faryaneh @saumyadabhi” in your final commit
message.

Submit the URL of your report (*not the URL of your repo*) in Canvas: \*
Click on HW4 under Assignments in Canvas \* Under “Assignment
Submission”, click the “Write Submission” button. \* Copy/paste the URL
of your `report.md` file into the edit box (should be something like
https<nolink>://github.com/odu-cs625-datavis/fall22-hw4-*username*/blob/master/report.md)
\* Make sure to “Submit” your assignment. - Unstack the group by in
Python,
<https://stackoverflow.com/questions/51034291/in-python-how-do-i-create-a-line-plot-based-on-groupby-of-two-categories-wit> -
Plotting Multi-index columns,
<https://towardsdatascience.com/how-to-make-multi-index-index-charts-with-plotly-4d3984cd7b09> -
Sum Columns in the Multi-index in panads,
<https://stackoverflow.com/questions/48272452/sum-columns-by-level-in-a-pandas-multiindex-dataframe> -
Selecting the n rows from a dataframe,
<https://datascienceparichay.com/article/pandas-select-first-n-rows-dataframe/> -
Excluding several columns from the groupby,
<https://stackoverflow.com/questions/32751229/pandas-sum-by-groupby-but-exclude-certain-columns>
\*Undestanding what is level=0 in groupby,
<https://stackoverflow.com/questions/49859182/understanding-level-0-and-group-keys> -
\* R markdown, <https://r4ds.had.co.nz/r-markdown.html>

## Including Code

You can include R code in the document as follows:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

## Including Plots

You can also embed plots, for example:

![](report_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
