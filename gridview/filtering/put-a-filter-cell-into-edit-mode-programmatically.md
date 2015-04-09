---
title: Put a filter cell into edit mode programmatically
page_title: Put a filter cell into edit mode programmatically
description: Put a filter cell into edit mode programmatically
slug: gridview-filtering-put-a-filter-cell-into-edit-mode-programmatically
tags: put,a,filter,cell,into,edit,mode,programmatically
published: True
position: 4
---

# Put a filter cell into edit mode programmatically



## 

You can easily put a filter cell into edit mode by code. You should simply call the BeginEdit method of the desired cell:

#### __[C#] Put a filter cell in edit mode programmatically__

{{region putFilterCellIntoEditModeProgramatically}}
	            this.radGridView1.MasterView.TableFilteringRow.Cells[1].BeginEdit();
	{{endregion}}



#### __[VB.NET] Put a filter cell in edit mode programmatically__

{{region putFilterCellIntoEditModeProgramatically}}
	        Me.RadGridView1.MasterView.TableFilteringRow.Cells(0).BeginEdit()
	{{endregion}}

![gridview-filtering-put-a-filter-cell-into-edit-mode-programatically 001](images/gridview-filtering-put-a-filter-cell-into-edit-mode-programatically001.png)![gridview-filtering-put-a-filter-cell-into-edit-mode-programatically 002](images/gridview-filtering-put-a-filter-cell-into-edit-mode-programatically002.png)