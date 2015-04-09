---
title: Troubleshooting
page_title: Troubleshooting
description: Troubleshooting
slug: gridview-exporting-data-export-to-troubleshooting
tags: troubleshooting
published: True
position: 7
---

# Troubleshooting



## Exported file is corrupt and will not open in Excel 2003 or Excel 2007

Check for invalid DateTime values, such as "0001-01-01T00:00:00.000". MS Excel does not support DateTime values before "1900-01-01". UPDATE: Since Q1 2010 the ExportToExcelML method validates the date values and throws a FormatException with message: “The DateTime value is not supported in Excel!”.

## RadGridView cannot export child templates and hierarchical data

RadGridView does not support exporting child templates, because Microsoft Excel does not support hierarchical data. Telerik is looking for a solution by flattening hierarchical data, and RadGridView may support this in the future. There is a community workaround which can be used as a temporary solution:  [Export To Excel Including Child Templates](http://www.telerik.com/community/forums/winforms/gridview/export-to-excel-including-child-templates.aspx)

## RadGridView cannot export images

Images cannot be exported to MS Excel, because Excel does not support embedded images in cells. In addition, the [ExportToExcelML]({%slug gridview-exporting-data-export-to-excel-via-excelml-format%}) method uses a special XML format which cannot include images by specification.

## Exporting data programmatically creates a blank Excel document

Since ExportToExcelML iterates through the grid elements, it does not export anything if the grid has not created its child elements yet (i.e. if there is a grid instance but it is not shown on a form). The solution is to use the __LoadElementTree__ method before running the export: 

#### __[C#] __

{{region inCaseOfBlankExcelDocument}}
	            this.radGridView1.LoadElementTree();
	{{endregion}}



#### __[VB.NET] __

{{region inCaseOfBlankExcelDocument}}
	        Me.RadGridView1.LoadElementTree()
	{{endregion}}



UPDATE: The issue has been addressed in Q1 2010 onwards. 

## Exporting two or more independent RadGridViews to one Excel file does not work

RadGridView does not supports exporting data from two or more grids in one and the same Excel file. 

## MS Excel does not open the file directly after exporting the data from RadGridView (it prompts to save the file instead)

The ExportToExcelML class does not support opening the excel file directly. However, you can easily implement similar functionality through the __Process.Start__ method:

#### __[C#]__

{{region openTheFileAfterExport}}
	            ExportToExcelML exporter = new ExportToExcelML(this.radGridView1);
	            exporter.RunExport(@"C:\Test.xls");
	            System.Diagnostics.Process.Start(@"C:\Test.xls");
	{{endregion}}



#### __[VB.NET]__

{{region openTheFileAfterExport}}
	        Dim exporter As ExportToExcelML = New ExportToExcelML(Me.RadGridView1)
	        exporter.RunExport("C:\Test.xls")
	        System.Diagnostics.Process.Start("C:\Test.xls")
	{{endregion}}



## Wrong text alignment for rows with conditional formatting

ExportToExcelML does not support all text alignment settings for the conditional formatting; because of this ExportToExcelML takes and applies current conditional formatting alignments, but RadGridView skips the default values. This will be improved in future versions of RadGridView, but there is a workaround we can suggest, so feel free to open a [support ticket](http://www.telerik.com/account/support-tickets.aspx) to request it.