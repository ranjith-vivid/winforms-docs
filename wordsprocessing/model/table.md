---
title: Table
page_title: Table
description: Table
slug: wordsprocessing-model-table
tags: table
published: True
position: 4
---

# Table



__Table__ element is a __Block__ element that provides a grid-based organization. It accepts
        [TableRow]({%slug wordsprocessing-model-tablerow%}) objects as children. The __TableRow__ object contains
        [TableCell]({%slug wordsprocessing-model-tablecell%}).
      

* [Inserting a Table](#inserting-a-table)

* [Modifying a Table](#modifying-a-table)

* [Operating with a Table](#operating-with-a-table)

## Inserting a Table

Tables can be added as a child of a
          [BlockContainer](http://www.telerik.com/help/winforms/t_telerik_windows_documents_flow_model_blockcontainerbase.html)
          element – [Section]({%slug wordsprocessing-model-section%}), [TableCell]({%slug wordsprocessing-model-tablecell%}),
          [Headers and Footers]({%slug wordsprocessing-model-headers-and-footers%}), through the __Blocks__ collection.
        

The following code snippet creates and inserts a Table to a Section.
        

#### __[C#] __

{{region radwordsprocessing-model-table_0}}
	            Table emptyTable = new Table(document); // Table object with 0 rows and 0 columns.
	            section.Blocks.Add(emptyTable);
	
	            Table table = new Table(document, 10, 5); // Table object with 10 rows and 5 columns.
	            section.Blocks.Add(table);
	
	{{endregion}}



#### __[VB.NET] __

{{region radwordsprocessing-model-table_0}}
	            Dim emptyTable As New Table(document)
	            ' Table object with 0 rows and 0 columns.
	            section.Blocks.Add(emptyTable)
	
	            Dim table As New Table(document, 10, 5)
	            ' Table object with 10 rows and 5 columns.
	        section.Blocks.Add(table)
	
	            '#End Region
	        End Sub
	
	        Private Sub InsertTable(document As RadFlowDocument, section As Section)
	            '#Region "radwordsprocessing-model-table_1"
	            Dim table As New Table(document, 10, 5)
	            section.Blocks.Insert(0, table)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTablehMethod(section As Section)
	            '#Region "radwordsprocessing-model-table_2"
	            Dim table As Table = section.Blocks.AddTable()
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithRadFlowDocumentEditor()
	            '#Region "radwordsprocessing-model-table_3"
	            Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
	            Dim table As Table = editor.InsertTable(5, 3)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithContent()
	            '#Region "radwordsprocessing-model-table_4"
	            Dim document As New RadFlowDocument()
	
	            Dim table As Table = document.Sections.AddSection().Blocks.AddTable()
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId)
	        table.StyleId = BuiltInStyleNames.TableGridStyleId
	
	            Dim cellBackground As New ThemableColor(Colors.Beige)
	
	            For i As Integer = 0 To 4
	                Dim row As TableRow = table.Rows.AddTableRow()
	
	                For j As Integer = 0 To 9
	                    Dim cell As TableCell = row.Cells.AddTableCell()
	                    cell.Blocks.AddParagraph().Inlines.AddRun(String.Format("Cell {0}, {1}", i, j))
	                    cell.Shading.BackgroundColor = cellBackground
	                    cell.PreferredWidth = New TableWidthUnit(50)
	                Next
	            Next
	            '#End Region
	        End Sub
	    End Class



>tipThe parent BlockContainer element (in this case - the *section*) should belong to the same document that is
            passed to the constructor of the __Table__.
          

You can add a table at a specific index in the __Blocks__ collection of a __BlockContainer__
          using the __Insert()__ method. Here is how to add a table at the beginning of a section:
        

#### __[C#] __

{{region radwordsprocessing-model-table_1}}
	            Table table = new Table(document, 10, 5);
	            section.Blocks.Insert(0, table);
	{{endregion}}



#### __[VB.NET] __

{{region radwordsprocessing-model-table_1}}
	            Dim table As New Table(document, 10, 5)
	            section.Blocks.Insert(0, table)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTablehMethod(section As Section)
	            '#Region "radwordsprocessing-model-table_2"
	            Dim table As Table = section.Blocks.AddTable()
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithRadFlowDocumentEditor()
	            '#Region "radwordsprocessing-model-table_3"
	            Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
	            Dim table As Table = editor.InsertTable(5, 3)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithContent()
	            '#Region "radwordsprocessing-model-table_4"
	            Dim document As New RadFlowDocument()
	
	            Dim table As Table = document.Sections.AddSection().Blocks.AddTable()
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId)
	        table.StyleId = BuiltInStyleNames.TableGridStyleId
	
	            Dim cellBackground As New ThemableColor(Colors.Beige)
	
	            For i As Integer = 0 To 4
	                Dim row As TableRow = table.Rows.AddTableRow()
	
	                For j As Integer = 0 To 9
	                    Dim cell As TableCell = row.Cells.AddTableCell()
	                    cell.Blocks.AddParagraph().Inlines.AddRun(String.Format("Cell {0}, {1}", i, j))
	                    cell.Shading.BackgroundColor = cellBackground
	                    cell.PreferredWidth = New TableWidthUnit(50)
	                Next
	            Next
	            '#End Region
	        End Sub
	    End Class



You can also use the __AddTable()__ method of the __Blocks__ collection of a __BlockContainer__.
          The method creates a new __Table__ instance, adds it to the container and returns it:
        

#### __[C#] __

{{region radwordsprocessing-model-table_2}}
	            Table table = section.Blocks.AddTable();
	{{endregion}}



#### __[VB.NET] __

{{region radwordsprocessing-model-table_2}}
	            Dim table As Table = section.Blocks.AddTable()
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithRadFlowDocumentEditor()
	            '#Region "radwordsprocessing-model-table_3"
	            Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
	            Dim table As Table = editor.InsertTable(5, 3)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithContent()
	            '#Region "radwordsprocessing-model-table_4"
	            Dim document As New RadFlowDocument()
	
	            Dim table As Table = document.Sections.AddSection().Blocks.AddTable()
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId)
	        table.StyleId = BuiltInStyleNames.TableGridStyleId
	
	            Dim cellBackground As New ThemableColor(Colors.Beige)
	
	            For i As Integer = 0 To 4
	                Dim row As TableRow = table.Rows.AddTableRow()
	
	                For j As Integer = 0 To 9
	                    Dim cell As TableCell = row.Cells.AddTableCell()
	                    cell.Blocks.AddParagraph().Inlines.AddRun(String.Format("Cell {0}, {1}", i, j))
	                    cell.Shading.BackgroundColor = cellBackground
	                    cell.PreferredWidth = New TableWidthUnit(50)
	                Next
	            Next
	            '#End Region
	        End Sub
	    End Class



Inserting a new Table in the document can also be achieved with the [RadFlowDocumentEditor]({%slug wordsprocessing-editing-radflowdocumenteditor%}) class:
        

#### __[C#] __

{{region radwordsprocessing-model-table_3}}
	            RadFlowDocumentEditor editor = new RadFlowDocumentEditor(new RadFlowDocument());
	            Table table = editor.InsertTable(5, 3);
	{{endregion}}



#### __[VB.NET] __

{{region radwordsprocessing-model-table_3}}
	            Dim editor As New RadFlowDocumentEditor(New RadFlowDocument())
	            Dim table As Table = editor.InsertTable(5, 3)
	            '#End Region
	        End Sub
	
	        Private Sub CreateTableWithContent()
	            '#Region "radwordsprocessing-model-table_4"
	            Dim document As New RadFlowDocument()
	
	            Dim table As Table = document.Sections.AddSection().Blocks.AddTable()
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId)
	        table.StyleId = BuiltInStyleNames.TableGridStyleId
	
	            Dim cellBackground As New ThemableColor(Colors.Beige)
	
	            For i As Integer = 0 To 4
	                Dim row As TableRow = table.Rows.AddTableRow()
	
	                For j As Integer = 0 To 9
	                    Dim cell As TableCell = row.Cells.AddTableCell()
	                    cell.Blocks.AddParagraph().Inlines.AddRun(String.Format("Cell {0}, {1}", i, j))
	                    cell.Shading.BackgroundColor = cellBackground
	                    cell.PreferredWidth = New TableWidthUnit(50)
	                Next
	            Next
	            '#End Region
	        End Sub
	    End Class



## Modifying a Table

__Properties__ exposes several properties that allow you to customize the layout of the elements placed underneath it. Here is a list
          of them:
        

* __Properties__:  Gets all table properties as TableProperties object. More info on how to use table properties can be
              found in [Style Properties]({%slug wordsprocessing-concepts-style-properties%}) article.
            

* __Rows__: Represents __TableRowCollection__ of the Table.
            

* __StyleId__: Represents the ID of the style applied on the Table element.
            

* __Alignment__: Specifies the alignment of the Table. *Style property.*

* __Borders__: Specifies the borders of the Table. *Style property.*

* __Shading__: Represents the shading applied to the table. It is a composite object and is read-only.
              You can obtain the following properties from it:
            

* __BackgroundColor__: Specifies the background color for the shading.
                  *Style property. The value is themable object.*

* __PatternColor__: Specifies the pattern color for the shading.
                  *Style property. The value is themable object.*

* __Pattern__: Specifies the pattern which is used to lay the pattern color over the background color for the shading.
                  *Style property.*

* __GridColumnsCount__: Returns the number of the columns in the table grid.
            

* __GridRowsCount__: Returns the number of the rows in the table grid.
            

* __TableCellSpacing__: Specifies the spacing between adjacent cells and the edges of the table.
              *Style property.*

* __HasCellSpacing__: Indicates whether there is TableCellSpacing applied in the table.
            

* __TableCellPadding__: Specifies the default padding of the cells inside the table.
              *Style property.*

* __Indent__: Represents the size of the indent added before the leading edge of the table.
              *Style property.*

* __FlowDirection__: Represents the flow direction of cells inside the table. The default value is LeftToRight.
            

* __PreferredWidth__: Specifies the preferred width of the table.
            

* __Looks__: Specifies the value indicating which components of the conditional style should be applied if such exists.
            

* __LayoutType__: Specifies the algorithm which is used to lay out the contants of this table. The possible values are
              __FixedWidth__ or __AutoFit__. The default is __AutoFit__.
            

* __Overlap__: Indicates whether this floating table allows other floating tables to overlap its extents.
            

>tipStyle properties are properties that can be inherited from a style. For more information about styles see
            [this article]({%slug wordsprocessing-concepts-style-properties%}).
          

>tipThemable objects are objects that can be inherited from a theme. For more information about themes check
            [this article]({%slug wordsprocessing-concepts-document-themes%}).
          

## Operating with a Table

### Creating a Table with Content

The following code snippet demonstrates how to add a __Table__ with 5 rows and 10 columns to a
              __RadFlowDocument__:
            

#### __[C#] __

{{region radwordsprocessing-model-table_4}}
	            RadFlowDocument document = new RadFlowDocument();
	
	            Table table = document.Sections.AddSection().Blocks.AddTable();
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId);
	            table.StyleId = BuiltInStyleNames.TableGridStyleId;
	
	            ThemableColor cellBackground = new ThemableColor(Colors.Beige);
	
	            for (int i = 0; i < 5; i++)
	            {
	                TableRow row = table.Rows.AddTableRow();
	
	                for (int j = 0; j < 10; j++)
	                {
	                    TableCell cell = row.Cells.AddTableCell();
	                    cell.Blocks.AddParagraph().Inlines.AddRun(string.Format("Cell {0}, {1}", i, j));
	                    cell.Shading.BackgroundColor = cellBackground;
	                    cell.PreferredWidth = new TableWidthUnit(50);
	                }
	            }
	{{endregion}}



#### __[VB.NET] __

{{region radwordsprocessing-model-table_4}}
	            Dim document As New RadFlowDocument()
	
	            Dim table As Table = document.Sections.AddSection().Blocks.AddTable()
	            document.StyleRepository.AddBuiltInStyle(BuiltInStyleNames.TableGridStyleId)
	        table.StyleId = BuiltInStyleNames.TableGridStyleId
	
	            Dim cellBackground As New ThemableColor(Colors.Beige)
	
	            For i As Integer = 0 To 4
	                Dim row As TableRow = table.Rows.AddTableRow()
	
	                For j As Integer = 0 To 9
	                    Dim cell As TableCell = row.Cells.AddTableCell()
	                    cell.Blocks.AddParagraph().Inlines.AddRun(String.Format("Cell {0}, {1}", i, j))
	                    cell.Shading.BackgroundColor = cellBackground
	                    cell.PreferredWidth = New TableWidthUnit(50)
	                Next
	            Next
	            '#End Region
	        End Sub
	    End Class



# See Also[Table API Reference](http://www.telerik.com/help/winforms/allmembers_t_telerik_windows_documents_flow_model_table.html)

 * [Section]({%slug wordsprocessing-model-section%})

 * [TableRow]({%slug wordsprocessing-model-tablerow%})

 * [TableCell]({%slug wordsprocessing-model-tablecell%})

 * [Style Properties]({%slug wordsprocessing-concepts-style-properties%})