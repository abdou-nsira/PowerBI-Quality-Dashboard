# Power BI Quality Findings Dashboard

A small end-to-end quality management analytics project built with **Power BI Desktop**, **Power Query**, **DAX**, and **Power Automate Desktop**. It analyzes a synthetic dataset of quality findings and demonstrates a weekly email reporting workflow.

## Project overview

The starting dataset contains **500 simulated findings** with dates, regions, countries, defect types, severity scores (1–5), and descriptions. After cleaning and removing duplicate records, the report analyzes **490 findings**.

### Main features

- **Power Query:** removed duplicate rows, standardized country values (DE / Germany / Deutschland), and checked data types.
- **Data modeling:** star schema with `FactFindings`, `DimRegion`, `DimFehlerart`, and `DimDatum`, connected by one-to-many relationships.
- **DAX measures:** total findings, average severity, critical findings (severity ≥ 4), and critical rate.
- **Interactive report:** KPI cards, defect-type Pareto chart, defect-type × severity heatmap, and a Region slicer.
- **Desktop automation:** Power Automate Desktop sends a quality-summary email using Gmail SMTP; Windows Task Scheduler is configured to launch the flow every Monday at 09:00.

## Results

| Metric | Value |
| --- | ---: |
| Total Findings | 490 |
| Average Severity | 3.02 / 5 |
| Critical Findings (severity ≥ 4) | 163 |
| Critical Rate | 33.27% |

These are example results from the cleaned synthetic dataset, not live operational metrics.

## DAX measures

```dax
Total Findings = COUNTROWS(FactFindings)

Average Severity = AVERAGE(FactFindings[Schweregrad])

Critical Findings =
CALCULATE(
    [Total Findings],
    FactFindings[Schweregrad] >= 4
)

Critical Rate =
DIVIDE([Critical Findings], [Total Findings], 0)
```

## Opening the dashboard

Open the Power BI project file (`.pbix`) in **Power BI Desktop** after it is added to this repository. If its local CSV source path differs on your computer, update the data source path in Power Query.

## Automation scope and limitations

The email workflow was successfully tested, including a test launched through Windows Task Scheduler. The external launch still asks the user to click **Continue**, so this is **not an unattended or cloud-hosted scheduled flow**. The computer must be running with a signed-in user session, and the message currently contains **fixed example KPI values** rather than refreshing from the Power BI model.

The original exercise requested a Power Automate **cloud** scheduled flow. This project uses a **local desktop alternative** due to account access limitations.

> **Security:** Do not commit Gmail app passwords, SMTP credentials, authentication tokens, or unredacted account screenshots. The dataset is synthetic.

## Tools

Power BI Desktop · Power Query · DAX · Power Automate Desktop · Windows Task Scheduler · Gmail SMTP
