---
title: Advanced
page_title: Advanced
description: Advanced
slug: gridview-save-and-load-layout-advanced
tags: advanced
published: True
position: 2
---

# Advanced



## 

RadGridView Serialization API provides a wide range of options that enables the programmer to customize the serialized content in a flexible manner. Based on the principles of the standard Microsoft .NET XML serialization approach, the API can be easily used for implementing different serialization scenarios. 

This article demonstrates how to alter the default serialization procedure and explicitly define the properties to be serialized.

Here are several properties to have in mind:

* __radGridView1.XmlSerializationInfo__ - this property of RadGridView gets the serialization information used by the SaveLayout and LoadLayout methods. It contains all RadGridView data that should persist between application states.
            

* __radGridView1.XmlSerializationInfo.DisregardOriginalSerializationVisibility__ - this property determines whether the serialization engine should take the default serialization attributes or the custom serialization metadata provided by the programmer. To define a custom collection of properties, set the property to true.
            

* __radGridView1.XmlSerializationInfo.SerializationMetadata__ - this property returns a collection of PropertySerializationMetadata objects, which contains information about the type, name and serialization method of the corresponding property. You need to clear the collection in order to be sure that only the serialization instructions will be considered.
            

Here is a snippet which prepares the SerializationMedata for customization:

#### __[C#]__

{{region preparation}}
	            this.radGridView1.XmlSerializationInfo.DisregardOriginalSerializationVisibility = true;
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Clear();
	{{endregion}}



#### __[VB.NET]__

{{region preparation}}
	        Me.RadGridView1.XmlSerializationInfo.DisregardOriginalSerializationVisibility = True
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Clear()
	{{endregion}}



The first property to add for serialization is the MasterGridViewTemplate, which contains RadGridView’s data and its structure. Please note that the MasterGridViewTemplate property is of type GridViewTemplate and that the DesignerSerializationVisibilityAttribute works exactly as the standard Microsoft .NET Framework attribute. 

#### __[C#]__

{{region SerializationMetadataRadGridView}}
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(RadGridView),"MasterTemplate",DesignerSerializationVisibilityAttribute.Content);
	{{endregion}}



#### __[VB.NET]__

{{region SerializationMetadataRadGridView}}
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(RadGridView), "MasterTemplate", DesignerSerializationVisibilityAttribute.Content)
	{{endregion}}



You can use the same approach to add the child templates. Additionally, you may want to add the Caption property of the template.

#### __[C#]__

{{region SerializationMetadataChildTemplates}}
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "Templates", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "Caption", DesignerSerializationVisibilityAttribute.Visible);
	{{endregion}}



#### __[VB.NET]__

{{region SerializationMetadataChildTemplates}}
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "Templates", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "Caption", DesignerSerializationVisibilityAttribute.Visible)
	{{endregion}}



Since the Columns property points to a complex object, you should set the DesignerSerializationVisibilityAttribute attribute to Content. In this way, the serialization engine will automatically detect that Columns points to a collection and will enumerate its items. Further, for each column you should define the properties to be stored in the XML output. In the following snippet the serialized properties are UniqueName and Width. By specifying the GridViewDataColumn type you tell the serializer that this instruction should be considered each time an object of this type is found. Please note that the DesignerSerializationVisibilityAttribute attribute is set to Visibility, because the value of the property should be serialized.

#### __[C#]__

{{region SerializationMetadataGridViewDataColumn}}
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "Columns", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewDataColumn), "Name", DesignerSerializationVisibilityAttribute.Visible);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewDataColumn), "Width", DesignerSerializationVisibilityAttribute.Visible);
	{{endregion}}



#### __[VB.NET]__

{{region SerializationMetadataGridViewDataColumn}}
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "Columns", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewDataColumn), "Name", DesignerSerializationVisibilityAttribute.Visible)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewDataColumn), "Width", DesignerSerializationVisibilityAttribute.Visible)
	{{endregion}}



Further, you may want to save information concerning grouping, sorting, filtering. Here is a sample snippet:

#### __[C#]__

{{region SerializationMetadataOperations}}
	            //Groups Descriptors	            
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "GroupDescriptors", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GroupDescriptor), "GroupNames", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(SortDescriptor), "PropertyName", DesignerSerializationVisibilityAttribute.Visible);
	            //Sort Descriptors            
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "SortDescriptors", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(SortDescriptor), "Direction", DesignerSerializationVisibilityAttribute.Visible);
	            //Filter Descriptors           
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(GridViewTemplate), "FilterDescriptors", DesignerSerializationVisibilityAttribute.Content);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(FilterDescriptor), "PropertyName", DesignerSerializationVisibilityAttribute.Visible);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(FilterDescriptor), "Operator", DesignerSerializationVisibilityAttribute.Visible);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(FilterDescriptor), "Value", DesignerSerializationVisibilityAttribute.Visible);
	            this.radGridView1.XmlSerializationInfo.SerializationMetadata.Add(typeof(FilterDescriptor), "IsFilterEditor", DesignerSerializationVisibilityAttribute.Visible);
	{{endregion}}



#### __[VB.NET]__

{{region SerializationMetadataOperations}}
	        'Groups Descriptors
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "GroupDescriptors", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GroupDescriptor), "GroupNames", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(SortDescriptor), "PropertyName", DesignerSerializationVisibilityAttribute.Visible)
	        'Sort Descriptors            
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "SortDescriptors", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(SortDescriptor), "Direction", DesignerSerializationVisibilityAttribute.Visible)
	        'Filter Descriptors           
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(GridViewTemplate), "FilterDescriptors", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(FilterDescriptor), "PropertyName", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(FilterDescriptor), "Operator", DesignerSerializationVisibilityAttribute.Content)
	        Me.RadGridView1.XmlSerializationInfo.SerializationMetadata.Add(GetType(FilterDescriptor), "Value", DesignerSerializationVisibilityAttribute.Visible)
	{{endregion}}

