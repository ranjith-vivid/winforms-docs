---
title: Using PdfFormatProvider
page_title: Using PdfFormatProvider
description: Using PdfFormatProvider
slug: wordsprocessing-formats-and-conversion-docx-using-pdfformatprovider
tags: using,pdfformatprovider
published: True
position: 1
---

# Using PdfFormatProvider



__PdfFormatProvider__ makes it easy to import and export __RadFlowDocument__ to PDF format,
        preserving the entire document structure and formatting.
      

All you have to do in order to use __PdfFormatProvider__ is add references to the assemblies listed below:
      

* Telerik.Windows.Documents.Core.dll
          

* Telerik.Windows.Documents.Flow.dll
          

* Telerik.Windows.Zip.dll
          

* Telerik.Windows.Documents.Flow.FormatProviders.Pdf.dll
          

* Telerik.Windows.Documents.Fixed.dll
          

## Export

In order to export a document to PDF you need to use the __Export()__ method of __PdfFormatProvider__.
      

The code snippet in __Example 1__ shows how to create a __PdfFormatProvider__ instance and use it to export __RadFlowDocument__ to a file.
      

#### __[C#] Example 1:__





#### __[VB] Example 1:__

{{region radwordsprocessing-formats-and-conversion-pdf-pdfformatprovider_0}}
	
	        Dim provider As New Telerik.Windows.Documents.Flow.FormatProviders.Pdf.PdfFormatProvider()
	        Using output As Stream = File.OpenWrite("sample.pdf")
	            Dim document As Telerik.Windows.Documents.Flow.Model.RadFlowDocument = CreateRadFlowDocument()
	            provider.Export(document, output)
	        End Using
	
	        '#End Region
	    End Sub
	
	    Private Function CreateRadFlowDocument() As Telerik.Windows.Documents.Flow.Model.RadFlowDocument
	        Return New Telerik.Windows.Documents.Flow.Model.RadFlowDocument()
	    End Function
	End Class



# See Also

 * [Settings]({%slug pdfprocessing-formats-and-conversion-pdf-settings%})

 * [How to Comply with PDF/A Standard]({%slug pdfprocessing-how-to-comply-with-pdfa-standard%})