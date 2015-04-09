---
title: GridViewTextBoxColumn
page_title: GridViewTextBoxColumn
description: GridViewTextBoxColumn
slug: gridview-columns-gridviewtextboxcolumn
tags: gridviewtextboxcolumn
published: True
position: 14
---

# GridViewTextBoxColumn



## 

__GridViewTextBoxColumn__ displays and allows editing of text data. Each cell in __GridViewTextBoxColumn__ column displays the text of cell __Value__ property according to the settings of the __TextAlignment__ (default is __ContentAlignment.MiddleLeft__) and __WrapText__(default is __false__) properties. The displayed text is formatted according to the value of the __FormatString__ property.

When a user begins editing a cell, a textbox editor is provided to handle the user’s input. The length of the text the user can enter is restricted to the value of __MaxLength__ property.

![gridview-columns-gridviewtextboxcolumn 001](images/gridview-columns-gridviewtextboxcolumn001.png)

#### __[C#] Adding GridViewTextBoxColumn__

{{region addTextBoxColumn}}
	            GridViewTextBoxColumn textBoxColumn = new GridViewTextBoxColumn();
	            textBoxColumn.Name = "TextBoxColumn";
	            textBoxColumn.HeaderText = "Product Name";
	            textBoxColumn.FieldName = "ProductName";
	            textBoxColumn.MaxLength = 50;
	            textBoxColumn.TextAlignment = ContentAlignment.BottomRight;
	            radGridView1.MasterTemplate.Columns.Add(textBoxColumn);
	{{endregion}}



#### __[VB.NET] Adding GridViewTextBoxColumn__

{{region addTextBoxColumn}}
	        Dim textBoxColumn As New GridViewTextBoxColumn()
	        textBoxColumn.Name = "TextBoxColumn"
	        textBoxColumn.HeaderText = "Product Name"
	        textBoxColumn.FieldName = "ProductName"
	        textBoxColumn.MaxLength = 50
	        textBoxColumn.TextAlignment = ContentAlignment.BottomRight
	        RadGridView1.MasterTemplate.Columns.Add(textBoxColumn)
	{{endregion}}



## Character casing

GridViewTextBoxColumn editor - *RadTextBoxEditor - *supports character casing. To enable this functionality you need to set *ColumnCharecterCasing* property of the desired GridViewTextBoxColumn column:

#### __[C#] Character casting in GridViewTextBoxColumn__

{{region characterCasting}}
	            ((GridViewTextBoxColumn)this.radGridView1.Columns[0]).ColumnCharacterCasing = CharacterCasing.Upper;
	{{endregion}}



#### __[VB.NET] Character casting in GridViewTextBoxColumn__

{{region characterCasting}}
	        DirectCast(Me.RadGridView1.Columns(0), GridViewTextBoxColumn).ColumnCharacterCasing = CharacterCasing.Upper
	{{endregion}}



>ColumnCarecterCasing property affects only the editor and does not change the values in your data base. For instance, if your data base contains text which is lower case or partially lower case and you set the ColumnCharecterCasing to upper case the data will not be change, but if the user starts editing a cell, the editor will only allow upper case symbols and all lower case symbols will be converted to upper case ones.