---
title: Pie Charts
page_title: Pie Charts
description: Pie Charts
slug: chart-undestanding-radchart-types-pie-charts
tags: pie,charts
published: True
position: 4
---

# Pie Charts



## 

Pie charts are used to display the contribution of fractional parts to a whole. A Pie chart uses a single series of data.  Multiple series of data can be defined and are each displayed in a separate pie chart.  

You can tailor the look of each pie slice individually by working with the pie chart series __Appearance__ property:

* You can "explode" (i.e. offset) a single slice of the pie to emphasize a portion of the data by setting the____pie chart series item__Appearance.Exploded__property to __true__.

* The __Appearance.FillStyle FillSettings__property allows you to set the fill for each slice to Solid, Gradient, ComplexGradient, Hatch or Image. You also have control over gradients (colors, direction and angle), images (alignment, orientation), colors and shadows.

* Control the label format and appearance for each slice using the pie chart series item __Label__property.  The __Label.TextBlock.Text__can make use of built-in formats, such as "#Y" to show the values for the Y axis or "#%" to show the percentage value of a slice.  The figure below uses a format similar to "Qtr 1 - #%". 



To create a Pie chart set the RadChart __DefaultType__ property or __ChartSeries__.__Type____Pie__.

![chart-undestanding-radchart-types-pie-charts 001](images/chart-undestanding-radchart-types-pie-charts001.png)

![chart-undestanding-radchart-types-pie-charts 002](images/chart-undestanding-radchart-types-pie-charts002.png)