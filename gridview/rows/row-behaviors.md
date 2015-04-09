---
title: Row behaviors
page_title: Row behaviors
description: Row behaviors
slug: gridview-rows-row-behaviors
tags: row,behaviors
published: True
position: 15
---

# Row behaviors



## 

__RadGridView__ manages user *mouse* and *keyboard* input over its rows by __GridRowBehavior__. 
        Depending on the row type, __RadGridView__ introduces different behaviors, listed on the table below: 
<table><th><tr><td>

Row behavior</td><td>

Row type</td></tr></th><tr><td>

GridDataRowBehavior</td><td>

GridViewDataRowInfo</td></tr><tr><td>

GridHierarchyRowBehavior</td><td>

GridViewHierarchyRowInfo</td></tr><tr><td>

GridNewRowBehavior</td><td>

GridViewNewRowInfo</td></tr><tr><td>

GridGroupRowBehavior</td><td>

GridViewGroupRowInfo</td></tr><tr><td>

GridFilterRowBehavior</td><td>

GridViewFilteringRowInfo</td></tr><tr><td>

GridHeaderRowBehavior</td><td>

GridViewTableHeaderRowInfo</td></tr><tr><td>

GridDetailViewRowBehavior</td><td>

GridViewDetailsRowInfo</td></tr></table>

By implementing a specific custom row behavior, developers can change the default row functionality or supplement the existing one. 

Let’s start with constructing a hierarchical __RadGridView__ and populate it with data.

#### __[C#] Fill RadGridView with hierarchical data__

{{region FillHierarchicalData}}
	        
	        public RowBehaviorsForm()
	        {
	            InitializeComponent();
	            
	            //Fill data
	            DataTable items = new DataTable("Items");
	            items.Columns.Add("Id", typeof(int));
	            items.Columns.Add("Title", typeof(string));
	            items.Columns.Add("IsActive", typeof(bool));
	            
	            DataTable subItems = new DataTable("SubItems");
	            subItems.Columns.Add("Id", typeof(int));
	            subItems.Columns.Add("Name", typeof(string));
	            subItems.Columns.Add("ItemId", typeof(int));
	            
	            for (int i = 1; i <= 3; i++)
	            {
	                items.Rows.Add(i, "Item" + i, i % 2 == 0);
	                
	                for (int j = 1000; j <= 1005; j++)
	                {
	                    subItems.Rows.Add(j, "SubItem" + j, i);
	                }
	            }
	            
	            //Set up grid hierarchy
	            radGridView1.DataSource = items;
	            radGridView1.AutoSizeColumnsMode = GridViewAutoSizeColumnsMode.Fill;
	            
	            GridViewTemplate template = new GridViewTemplate();
	            template.ReadOnly = true;
	            template.AutoSizeColumnsMode = GridViewAutoSizeColumnsMode.Fill;
	            
	            GridViewRelation relation = new GridViewRelation(radGridView1.MasterTemplate, template);
	            relation.ParentColumnNames.Add("Id");
	            relation.ChildColumnNames.Add("ItemId");
	            radGridView1.MasterTemplate.Templates.Add(template);
	            radGridView1.Relations.Add(relation);
	            template.ReadOnly = false;
	            template.DataSource = subItems;
	        }
	        
	{{endregion}}



#### __[VB.NET] Fill RadGridView with hierarchical data__

{{region FillHierarchicalData}}
	    Sub New()
	        InitializeComponent()
	
	        'Fill data
	        Dim items As New DataTable("Items")
	        items.Columns.Add("Id", GetType(Integer))
	        items.Columns.Add("Title", GetType(String))
	        items.Columns.Add("IsActive", GetType(Boolean))
	
	        Dim subItems As New DataTable("SubItems")
	        subItems.Columns.Add("Id", GetType(Integer))
	        subItems.Columns.Add("Name", GetType(String))
	        subItems.Columns.Add("ItemId", GetType(Integer))
	
	        For i As Integer = 1 To 3
	            items.Rows.Add(i, "Item" & i, i Mod 2 = 0)
	
	            For j As Integer = 1000 To 1005
	                subItems.Rows.Add(j, "SubItem" & j, i)
	            Next
	        Next
	
	        'Set up grid hierarchy
	        RadGridView1.DataSource = items
	        RadGridView1.AutoSizeColumnsMode = GridViewAutoSizeColumnsMode.Fill
	
	        Dim template As New GridViewTemplate()
	        template.[ReadOnly] = True
	        template.AutoSizeColumnsMode = GridViewAutoSizeColumnsMode.Fill
	
	        Dim relation As New GridViewRelation(RadGridView1.MasterTemplate, template)
	        relation.ParentColumnNames.Add("Id")
	        relation.ChildColumnNames.Add("ItemId")
	        RadGridView1.MasterTemplate.Templates.Add(template)
	        RadGridView1.Relations.Add(relation)
	        template.[ReadOnly] = False
	        template.DataSource = subItems
	    End Sub
	    '#End Region
	
	    Protected Overrides Sub OnLoad(e As EventArgs)
	        MyBase.OnLoad(e)
	
	        '#Region "RegisterBehavior"
	
	        'register the custom row  behavior
	        Dim gridBehavior As BaseGridBehavior = TryCast(RadGridView1.GridBehavior, BaseGridBehavior)
	        gridBehavior.UnregisterBehavior(GetType(GridViewHierarchyRowInfo))
	        gridBehavior.RegisterBehavior(GetType(GridViewHierarchyRowInfo), New CustomGridHierarchyRowBehavior())
	
	        '#End Region
	    End Sub
	
	    '#Region "ProcessDeleteKey"
	
	    Public Class CustomGridHierarchyRowBehavior
	    Inherits GridHierarchyRowBehavior
	        Protected Overrides Function ProcessDeleteKey(keys As KeyEventArgs) As Boolean
	            If Me.GridControl.CurrentRow.ChildRows.Count > 0 Then
	                Dim result As DialogResult = MessageBox.Show("The current row has child rows." & "Are you sure you want to delete the selected row?", "Confirmation", MessageBoxButtons.YesNo)
	                If result = DialogResult.No Then
	                    Return True
	                End If
	            End If
	
	            Return MyBase.ProcessDeleteKey(keys)
	        End Function
	
	    End Class
	
	    '#End Region
	
	    Public Class DummyBehavior
	    Inherits GridHierarchyRowBehavior
	        '#Region "MouseDownLeft"
	
	        Protected Overrides Function OnMouseDownLeft(e As MouseEventArgs) As Boolean
	            Dim cellElement As GridCellElement = Me.GetCellAtPoint(e.Location)
	            If cellElement IsNot Nothing AndAlso TypeOf cellElement Is GridCheckBoxCellElement Then
	                Dim rowElement As GridRowElement = cellElement.RowElement
	                Me.Navigator.BeginSelection(Me.GetMouseNavigationContext(e))
	                Me.Navigator.[Select](rowElement.RowInfo, cellElement.ColumnInfo)
	                Me.Navigator.EndSelection()
	
	                If Not cellElement.IsInValidState(True) Then
	                    cellElement = Me.GetCellAtPoint(e.Location)
	                End If
	                If cellElement IsNot Nothing Then
	                    GridViewElement.ContextMenuManager.ShowContextMenu(cellElement)
	                End If
	                Return True
	            End If
	
	            Return MyBase.OnMouseDownLeft(e)
	        End Function
	
	        '#End Region
	        
	        '#Region "ProcessKey"
	
	        Public Overrides Function ProcessKey(keys__1 As KeyEventArgs) As Boolean
	            If keys__1.KeyCode = Keys.Up OrElse keys__1.KeyCode = Keys.Down Then
	                Dim rowView As DataRowView = TryCast(Me.GridControl.CurrentRow.DataBoundItem, DataRowView)
	                If rowView IsNot Nothing AndAlso Me.GridControl.CurrentRow.ViewTemplate.Equals(Me.MasterTemplate) Then
	                    If CBool(rowView.Row("IsActive")) = False Then
	                        Return True
	                    End If
	                End If
	            End If
	            Return MyBase.ProcessKey(keys__1)
	        End Function
	
	        '#End Region
	    End Class
	End Class



By default, when the user hits the __Delete__ key over a certain row, the row is deleted. We will extend this functionality by notifying the user 
        when he tries to delete a parent row, which __ChildRows__ collection is not empty. For this purpose, it is necessary to create a custom grid behavior.
        To do this, create a new class named __CustomGridHierarchyRowBehavior__. As we are currently using a hierarchical grid, our class should inherit
        the __GridHierarchyRowBehavior__. Override the __ProcessDeleteKey__ method in order to display a MessageBox and proceed with the delete operation after confirmation only:![gridview-rows-row-behaviors 001](images/gridview-rows-row-behaviors001.gif)

#### __[C#]__

{{region ProcessDeleteKey}}
	        
	        public class CustomGridHierarchyRowBehavior : GridHierarchyRowBehavior
	        {
	            protected override bool ProcessDeleteKey(KeyEventArgs keys)
	            {
	                if (this.GridControl.CurrentRow.ChildRows.Count > 0)
	                {
	                    DialogResult result = MessageBox.Show("The current row has child rows." +
	                                                          "Are you sure you want to delete the selected row?", "Confirmation", MessageBoxButtons.YesNo);
	                    if (result == DialogResult.No)
	                    {
	                        return true;
	                    }
	                }
	            
	                return base.ProcessDeleteKey(keys);
	            }
	        }
	
	{{endregion}}



#### __[VB.NET]__

{{region ProcessDeleteKey}}
	
	    Public Class CustomGridHierarchyRowBehavior
	    Inherits GridHierarchyRowBehavior
	        Protected Overrides Function ProcessDeleteKey(keys As KeyEventArgs) As Boolean
	            If Me.GridControl.CurrentRow.ChildRows.Count > 0 Then
	                Dim result As DialogResult = MessageBox.Show("The current row has child rows." & "Are you sure you want to delete the selected row?", "Confirmation", MessageBoxButtons.YesNo)
	                If result = DialogResult.No Then
	                    Return True
	                End If
	            End If
	
	            Return MyBase.ProcessDeleteKey(keys)
	        End Function
	
	    End Class
	
	    '#End Region
	
	    Public Class DummyBehavior
	    Inherits GridHierarchyRowBehavior
	        '#Region "MouseDownLeft"
	
	        Protected Overrides Function OnMouseDownLeft(e As MouseEventArgs) As Boolean
	            Dim cellElement As GridCellElement = Me.GetCellAtPoint(e.Location)
	            If cellElement IsNot Nothing AndAlso TypeOf cellElement Is GridCheckBoxCellElement Then
	                Dim rowElement As GridRowElement = cellElement.RowElement
	                Me.Navigator.BeginSelection(Me.GetMouseNavigationContext(e))
	                Me.Navigator.[Select](rowElement.RowInfo, cellElement.ColumnInfo)
	                Me.Navigator.EndSelection()
	
	                If Not cellElement.IsInValidState(True) Then
	                    cellElement = Me.GetCellAtPoint(e.Location)
	                End If
	                If cellElement IsNot Nothing Then
	                    GridViewElement.ContextMenuManager.ShowContextMenu(cellElement)
	                End If
	                Return True
	            End If
	
	            Return MyBase.OnMouseDownLeft(e)
	        End Function
	
	        '#End Region
	        
	        '#Region "ProcessKey"
	
	        Public Overrides Function ProcessKey(keys__1 As KeyEventArgs) As Boolean
	            If keys__1.KeyCode = Keys.Up OrElse keys__1.KeyCode = Keys.Down Then
	                Dim rowView As DataRowView = TryCast(Me.GridControl.CurrentRow.DataBoundItem, DataRowView)
	                If rowView IsNot Nothing AndAlso Me.GridControl.CurrentRow.ViewTemplate.Equals(Me.MasterTemplate) Then
	                    If CBool(rowView.Row("IsActive")) = False Then
	                        Return True
	                    End If
	                End If
	            End If
	            Return MyBase.ProcessKey(keys__1)
	        End Function
	
	        '#End Region
	    End Class
	End Class



Next we will register this behavior in our grid. Add the following code after populating the grid with data:

#### __[C#]__

{{region RegisterBehavior}}
	            
	            //register the custom row  behavior
	            BaseGridBehavior gridBehavior = radGridView1.GridBehavior as BaseGridBehavior;
	            gridBehavior.UnregisterBehavior(typeof(GridViewHierarchyRowInfo));
	            gridBehavior.RegisterBehavior(typeof(GridViewHierarchyRowInfo), new CustomGridHierarchyRowBehavior());
	        
	{{endregion}}



#### __[VB.NET]__

{{region RegisterBehavior}}
	
	        'register the custom row  behavior
	        Dim gridBehavior As BaseGridBehavior = TryCast(RadGridView1.GridBehavior, BaseGridBehavior)
	        gridBehavior.UnregisterBehavior(GetType(GridViewHierarchyRowInfo))
	        gridBehavior.RegisterBehavior(GetType(GridViewHierarchyRowInfo), New CustomGridHierarchyRowBehavior())
	
	        '#End Region
	    End Sub
	
	    '#Region "ProcessDeleteKey"
	
	    Public Class CustomGridHierarchyRowBehavior
	    Inherits GridHierarchyRowBehavior
	        Protected Overrides Function ProcessDeleteKey(keys As KeyEventArgs) As Boolean
	            If Me.GridControl.CurrentRow.ChildRows.Count > 0 Then
	                Dim result As DialogResult = MessageBox.Show("The current row has child rows." & "Are you sure you want to delete the selected row?", "Confirmation", MessageBoxButtons.YesNo)
	                If result = DialogResult.No Then
	                    Return True
	                End If
	            End If
	
	            Return MyBase.ProcessDeleteKey(keys)
	        End Function
	
	    End Class
	
	    '#End Region
	
	    Public Class DummyBehavior
	    Inherits GridHierarchyRowBehavior
	        '#Region "MouseDownLeft"
	
	        Protected Overrides Function OnMouseDownLeft(e As MouseEventArgs) As Boolean
	            Dim cellElement As GridCellElement = Me.GetCellAtPoint(e.Location)
	            If cellElement IsNot Nothing AndAlso TypeOf cellElement Is GridCheckBoxCellElement Then
	                Dim rowElement As GridRowElement = cellElement.RowElement
	                Me.Navigator.BeginSelection(Me.GetMouseNavigationContext(e))
	                Me.Navigator.[Select](rowElement.RowInfo, cellElement.ColumnInfo)
	                Me.Navigator.EndSelection()
	
	                If Not cellElement.IsInValidState(True) Then
	                    cellElement = Me.GetCellAtPoint(e.Location)
	                End If
	                If cellElement IsNot Nothing Then
	                    GridViewElement.ContextMenuManager.ShowContextMenu(cellElement)
	                End If
	                Return True
	            End If
	
	            Return MyBase.OnMouseDownLeft(e)
	        End Function
	
	        '#End Region
	        
	        '#Region "ProcessKey"
	
	        Public Overrides Function ProcessKey(keys__1 As KeyEventArgs) As Boolean
	            If keys__1.KeyCode = Keys.Up OrElse keys__1.KeyCode = Keys.Down Then
	                Dim rowView As DataRowView = TryCast(Me.GridControl.CurrentRow.DataBoundItem, DataRowView)
	                If rowView IsNot Nothing AndAlso Me.GridControl.CurrentRow.ViewTemplate.Equals(Me.MasterTemplate) Then
	                    If CBool(rowView.Row("IsActive")) = False Then
	                        Return True
	                    End If
	                End If
	            End If
	            Return MyBase.ProcessKey(keys__1)
	        End Function
	
	        '#End Region
	    End Class
	End Class



The next modification we are going to introduce is to override the __OnMouseDownLeft__ method and show the context menu for the 
        __GridCheckBoxCellElement__, associated with the mouse location. First, it is necessary to use the grid navigator to process selection
        of the cell element, positioned at the mouse location. Afterwards, show the context menu for the specific cell:

#### __[C#]__

{{region MouseDownLeft}}
	                
	            protected override bool OnMouseDownLeft(MouseEventArgs e)
	            {
	                GridCellElement cellElement = this.GetCellAtPoint(e.Location);
	                if (cellElement != null && cellElement is GridCheckBoxCellElement)
	                {
	                    GridRowElement rowElement = cellElement.RowElement;
	                    this.Navigator.BeginSelection(this.GetMouseNavigationContext(e));
	                    this.Navigator.Select(rowElement.RowInfo, cellElement.ColumnInfo);
	                    this.Navigator.EndSelection();
	                        
	                    if (!cellElement.IsInValidState(true))
	                    {
	                        cellElement = this.GetCellAtPoint(e.Location);
	                    }
	                    if (cellElement != null)
	                    {
	                        GridViewElement.ContextMenuManager.ShowContextMenu(cellElement);
	                    }
	                    return true;
	                }
	            
	                return base.OnMouseDownLeft(e);
	            }
	            
	{{endregion}}



#### __[VB.NET]__

{{region MouseDownLeft}}
	
	        Protected Overrides Function OnMouseDownLeft(e As MouseEventArgs) As Boolean
	            Dim cellElement As GridCellElement = Me.GetCellAtPoint(e.Location)
	            If cellElement IsNot Nothing AndAlso TypeOf cellElement Is GridCheckBoxCellElement Then
	                Dim rowElement As GridRowElement = cellElement.RowElement
	                Me.Navigator.BeginSelection(Me.GetMouseNavigationContext(e))
	                Me.Navigator.[Select](rowElement.RowInfo, cellElement.ColumnInfo)
	                Me.Navigator.EndSelection()
	
	                If Not cellElement.IsInValidState(True) Then
	                    cellElement = Me.GetCellAtPoint(e.Location)
	                End If
	                If cellElement IsNot Nothing Then
	                    GridViewElement.ContextMenuManager.ShowContextMenu(cellElement)
	                End If
	                Return True
	            End If
	
	            Return MyBase.OnMouseDownLeft(e)
	        End Function
	
	        '#End Region
	        
	        '#Region "ProcessKey"
	
	        Public Overrides Function ProcessKey(keys__1 As KeyEventArgs) As Boolean
	            If keys__1.KeyCode = Keys.Up OrElse keys__1.KeyCode = Keys.Down Then
	                Dim rowView As DataRowView = TryCast(Me.GridControl.CurrentRow.DataBoundItem, DataRowView)
	                If rowView IsNot Nothing AndAlso Me.GridControl.CurrentRow.ViewTemplate.Equals(Me.MasterTemplate) Then
	                    If CBool(rowView.Row("IsActive")) = False Then
	                        Return True
	                    End If
	                End If
	            End If
	            Return MyBase.ProcessKey(keys__1)
	        End Function
	
	        '#End Region
	    End Class
	End Class

![gridview-rows-row-behaviors 002](images/gridview-rows-row-behaviors002.png)

__RadGridView__ supports rows/cells navigation by default, using the arrow keys. It is possible to customize this behavior as well.
        In the __CustomGridHierarchyRowBehavior__ class override the __ProcessKey__ method and stop the base grid logic for navigation upwards/downwards
        if the current row belongs to the __MasterTemplate__ and its *“IsActive”* cell value is set to *false*:

#### __[C#]__

{{region ProcessKey}}
	                
	            public override bool ProcessKey(KeyEventArgs keys)
	            {
	                if (keys.KeyCode == Keys.Up || keys.KeyCode == Keys.Down)
	                {
	                    DataRowView rowView = this.GridControl.CurrentRow.DataBoundItem as DataRowView;
	                    if (rowView != null && this.GridControl.CurrentRow.ViewTemplate == this.MasterTemplate)
	                    {
	                        if ((bool)rowView.Row["IsActive"] == false)
	                        {
	                            return true;
	                        }
	                    }
	                }
	                return base.ProcessKey(keys);
	            }
	
	{{endregion}}



#### __[VB.NET]__

{{region ProcessKey}}
	
	        Public Overrides Function ProcessKey(keys__1 As KeyEventArgs) As Boolean
	            If keys__1.KeyCode = Keys.Up OrElse keys__1.KeyCode = Keys.Down Then
	                Dim rowView As DataRowView = TryCast(Me.GridControl.CurrentRow.DataBoundItem, DataRowView)
	                If rowView IsNot Nothing AndAlso Me.GridControl.CurrentRow.ViewTemplate.Equals(Me.MasterTemplate) Then
	                    If CBool(rowView.Row("IsActive")) = False Then
	                        Return True
	                    End If
	                End If
	            End If
	            Return MyBase.ProcessKey(keys__1)
	        End Function
	
	        '#End Region
	    End Class
	End Class



Following the demonstrated approach, developers can customize not only the hierarchy rows, but the new row for example,
        implementing a custom __GridNewRowBehavior__ and registering it for the __GridViewNewRowInfo__.