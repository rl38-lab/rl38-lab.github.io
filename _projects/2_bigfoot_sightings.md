---
name: "HW5: Bigfoot Sighting Reports"
tools: [Python, Altair, vega-lite]
description: Two plots of the BFRO bigfoot sighting reports made with Altair and vega-lite.
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# Bigfoot Sighting Reports

For this homework I used the BFRO bigfoot reports dataset. It has 4,747 reports from 49 states. Each report has a date, a season, a state and a class. Class A means the person clearly saw something, Class B means they heard or found something but did not get a clear look, and Class C means the story came from someone else.

## Plot 1: Reports per year and by season

<vegachart schema-url="{{ site.baseurl }}/assets/json/bigfoot_timeline_seasons.json" style="width: 100%"></vegachart>

The top chart shows how many reports there were each year from 1950 to 2018. Each line is one class of report. Reports were low until the 1970s, jumped up after 2000 and peaked in the mid 2000s, and then went down again. The bottom chart shows the number of reports in each season. At first it shows all years, and summer has the most reports.

For the top chart, year is on the x axis and the number of reports is on the y axis. Both are quantitative. I used a line mark because it makes the trend over time easy to follow. In the bottom chart, season is nominal and goes on the x axis in calendar order, and the bar height is the number of reports. In both charts I colored by report class. Class is a nominal variable, so I used Altair's default categorical color scheme. It gives each class a very different color, which is what you want for categories that have no order. I used the same colors in both charts so one legend works for the whole figure. Class C is hard to see because it only has 30 reports.

In my notebook I read the CSV from its GitHub URL and changed the date column into pandas dates so I could get the year. About 950 reports have no date, so I left them out of this plot. I also removed reports where the season is "Unknown" and only kept years from 1950 on, because before that there are only a few reports. Then I used groupby to count the reports for each year, season and class. Altair adds these counts up in the charts.

## Plot 2: Reports by state and season

<vegachart schema-url="{{ site.baseurl }}/assets/json/bigfoot_state_season_heatmap.json" style="width: 100%"></vegachart>

This heatmap shows the 20 states with the most reports and how many reports each state has in each season. The states are sorted so the ones with the most reports are at the top. Washington has the most reports, and most states have the most reports in summer. Some states are different. For example, Illinois has more reports in fall than in summer.

Season is on the x axis and state is on the y axis. Both are categories. The number of reports is shown with color. Since the count only goes from low to high, I used a sequential color scheme called "orangered". Light colors mean few reports and dark red means many reports. I did not use a diverging scheme because there is no middle value that matters here.

In the notebook I removed reports with an "Unknown" season and found the 20 states with the most reports. Then I used groupby to count reports for each state, season and class.

## Interactivity

The first plot has a brush. You can click and drag on the top chart to pick a range of years, and the bar chart below will only count reports from those years. Double-click to clear it. This makes the plot more useful because you can check if the season pattern changes over time. For example, you can see if the summer peak was already there in the 1970s or if it only showed up after 2000.

The second plot has a dropdown menu. You can pick All, Class A, Class B or Class C, and the heatmap will only show that class. This lets you check if a state has a lot of clear sightings or mostly less sure ones, without making a separate heatmap for each class. You can also hover over a box to see the exact number of reports.

## The Data and The Analysis

<div class="left">
{% include elements/button.html link="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/bfro_reports_fall2022.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/rl38-lab/rl38-lab.github.io/blob/main/python_notebooks/hw5_bigfoot.ipynb" text="The Analysis" %}
</div>
