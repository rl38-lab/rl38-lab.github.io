---
name: "HW5: Bigfoot Sighting Reports"
tools: [Python, Altair, vega-lite]
description: Two Altair + vega-lite visualizations of BFRO Bigfoot sighting reports, with a linked year brush and a class dropdown.
---

<!-- vega libraries matching the Altair 6 / vega-lite v6 specs exported in the notebook -->
<script src="https://cdn.jsdelivr.net/npm/vega@6"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@6"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@7"></script>

<style>
  .vis { margin: 1.5em 0; overflow-x: auto; }
</style>

# Bigfoot Sighting Reports (BFRO)


Raymond Lu, IS 445 Data Visualization, Homework 5

This page shows two Python + Altair visualizations of the Bigfoot Field Researchers Organization (BFRO) sighting reports dataset (4,747 reports across 49 US states). The plots were built in a Python notebook and exported to vega-lite JSON, which is rendered here with vega-embed. Links to the data and the notebook are at the bottom of the page.

## Plot 1: Reports over time, linked to a breakdown by season

<div id="vis1" class="vis"></div>

**What is shown.** The top chart shows how many Bigfoot reports were filed for sightings in each year from 1950 to 2018, split by BFRO's report class. Class A reports are clear sightings where the witness says they saw the animal; Class B reports are less certain encounters such as sounds, footprints or a sighting at a distance; Class C reports are second-hand stories (only 30 in the whole dataset, so that line stays near zero). The bottom chart counts the reports in each season for whichever years are selected in the top chart. With nothing selected it shows all years, and summer clearly has the most reports, followed by fall.

**Encodings and color.** In the top chart, year is a quantitative variable on the x-axis (position), the number of reports is a quantitative variable on the y-axis (position), and the line mark connects the years so the trend is easy to follow. In the bottom chart, season is a nominal variable on the x-axis (kept in calendar order: spring, summer, fall, winter) and the bar length encodes the number of reports. Both charts use color for report class. Report class is a nominal (categorical) variable, so I used a qualitative color scheme with three clearly different hues (orange for Class A, green for Class B, purple for Class C) taken from the ColorBrewer "Dark2" palette. A sequential or diverging scheme would wrongly suggest that the classes have an order or a midpoint. These three hues also differ in lightness, so they generally stay distinguishable for readers with common forms of color-vision deficiency, and the same colors are used in both charts so the legend applies to the whole figure.

**Data transformations.** In the notebook I read the CSV from its GitHub URL, converted the `date` column to pandas timestamps and extracted the year. About 950 reports have no usable date, so they are left out of this plot. I kept years 1950 to 2018, because before 1950 there are only a handful of reports per decade, which would stretch the axis without adding information. I also dropped the 91 reports with an "Unknown" season. Finally, I grouped the reports by year, season and class and counted them, which reduces the remaining reports to a 451-row summary table; Altair then sums those counts for each chart. This keeps the exported JSON small and makes the page load quickly.

## Plot 2: Which states report the most sightings, and in which season

<div id="vis2" class="vis"></div>

**What is shown.** This heatmap shows the 20 states with the most reports (rows) and the number of reports in each season (columns). States are sorted from most to fewest reports, so Washington, California and Florida sit at the top. The number in each cell gives the exact count. The pattern from Plot 1 holds in most states (summer is the busiest season), but there are exceptions: Florida's reports are spread much more evenly across the four seasons than in other states, and Illinois has more fall reports than summer reports.

**Encodings and color.** Season is mapped to the x-axis and state to the y-axis (both categorical, position encodings), and the number of reports is encoded with color in each rectangle. The count is a quantitative variable that only goes from low to high, so I used the sequential "yellow-orange-brown" color scheme: light yellow means few reports and dark brown means many. A sequential scheme fits because there is no meaningful midpoint (a diverging scheme would be misleading) and the change in lightness is easy to read even in grayscale. Because it is hard to compare exact values from color alone, I also added text labels with the count; the label switches from black to white on the darkest cells so it stays readable.

**Data transformations.** I removed reports with an "Unknown" season, counted reports per state, and kept the 20 states with the most reports (the remaining states have too few reports for a readable heatmap). I then counted reports by state, season and class in pandas. Inside the chart, a filter keeps only the class chosen in the dropdown and an aggregate sums the counts per state and season, so the colors and labels always match the current selection.

## Interactivity

Plot 1 is the main interactive figure. Clicking and dragging across the top chart creates an interval selection over a range of years, and the bottom chart immediately updates to show the season breakdown for only those years (drag the shaded box to move it, double-click to clear it). This lets a reader ask questions the static chart cannot answer, for example whether the big rise in reports after 2000 was concentrated in one season, or whether the summer peak was already there in the 1970s. Clicking a class in the legend highlights that class in both charts and fades the others (shift-click to add classes), which makes it easier to follow one class where the Class A and Class B lines cross. Hovering over any point or bar shows a tooltip with the exact year, class and count.

Plot 2 adds a dropdown menu ("Report class") that filters the heatmap to all reports, Class A only, Class B only or Class C only. This makes it possible to check whether a state's high count comes mostly from clear sightings or from less certain Class B encounters, without needing a separate heatmap for each class. Tooltips show the state, season and count for each cell.

## The Data and The Analysis

<div class="left">
{% include elements/button.html link="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/bfro_reports_fall2022.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/rl38-lab/rl38-lab.github.io/blob/main/python_notebooks/hw5_bigfoot.ipynb" text="The Analysis" %}
</div>

<script>
  (function () {
    var opts = { actions: false };
    vegaEmbed('#vis1', '{{ site.baseurl }}/assets/json/bigfoot_timeline_seasons.json', opts).catch(console.error);
    vegaEmbed('#vis2', '{{ site.baseurl }}/assets/json/bigfoot_state_season_heatmap.json', opts).catch(console.error);
  })();
</script>
